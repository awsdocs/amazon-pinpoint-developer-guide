# Step 5: Create the Lambda function that processes the incoming records<a name="tutorials-importing-data-lambda-function-process-incoming"></a>

The next Lambda function that you create processes the records in the incoming files that were created by the function that you created in [Step 4](tutorials-importing-data-lambda-function-input-split.md)\. Specifically, it does the following things:
+ Changes the headers in the incoming file to values that Amazon Pinpoint expects to see\.
+ Converts some of the contact information in the input spreadsheet into a format that Amazon Pinpoint expects\. For example, the value "United States" in the `Mailing Country` column is converted to "US" in the `Location.Country` column of the output file\. This is because Amazon Pinpoint expects countries to be represented in ISO\-3166\-1 format\.
+ Sends every phone number that it finds to the phone number validation service\. This step ensures that all phone numbers are converted to E\.164 format\. It also determines the phone number type\. Mobile phone numbers are created as SMS endpoints, while all other phone numbers are created as voice endpoints\.
+ Writes separate rows for each endpoint that it finds\. For example, if one row in the input file contains a phone number and an email address, then the output file contains a separate row for each of those endpoints\. However, the two records are united by a common user ID\.
+ Checks to see if endpoint IDs in the incoming file already exist in the Amazon Pinpoint project\. If they do, the Lambda function updates the existing records rather than creating new ones\.

When the function finishes processing the input files, it sends them to a folder in the `processed` directory\.

## Step 5\.1: Create the function<a name="tutorials-importing-data-lambda-function-process-incoming-create"></a>

The process of creating this function is similar to the process that you completed in [Step 4](tutorials-importing-data-lambda-function-input-split.md) of this tutorial\. First, you upload the \.zip file that contains the necessary libraries\. Next, you create two Python files\. 

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Choose **Author from scratch**\. Under **Basic information**, do the following:
   + For **Function name**, enter **CustomerImport\_ProcessInput**\.
   + For **Runtime**, choose **Python 3\.7**\.
   + For **Execution role**, choose **Use an existing role**\.
   + For **Existing role**, choose **ImporterPinpointRole**\.
   + Choose **Create function**\. 

1. Under **Function code**, for **Code entry type**, choose **Upload a \.zip file**\. Under **Function package**, choose **Upload**\. Choose the `pinpoint-importer.zip` file that you created in [Step 3](tutorials-importing-data-create-python-package.md)\. After you select the file, choose **Save**\. 
**Note**  
After you choose **Save**, you receive an error message stating that Lambda couldn’t open the file `lambda_function.py`\. Dismiss this error—you create this file in the next step\.

1. In the function editor, on the **File** menu, choose **New File**\. The editor creates a new file named `Untitled1`\.

1. Paste the following code in the editor:

   ```
   import io
   import os
   import csv
   import time
   import uuid
   import boto3
   import s3fs
   import input_internationalization as i18n
   from datetime import datetime
   from botocore.exceptions import ClientError
   
   # You might need to change some things to fit your specific needs.
   
   # If incoming data doesn't specify a country, you have to pass a default value.
   # Specify a default country code in ISO 3166-1 alpha-2 format.
   defaultCountry = "US"
   
   # The column header names that are applied to the output file. You might need to 
   # change the order of the items in this list to suit the data that's in the file
   # that you want to import. Column numbers are included as comments here to make it
   # easier to align the columns.
   # If you add columns here, you also need to add them in the process_incoming_file
   # function below. Specifically, you need to add them to the lists that the
   # Filewriter object uses to write the processed files. See the sections that 
   # begin at lines 137 and 175 below.
   header =    [                                               #Col num in input
                   'ChannelType',                              #not in input
                   'Address',                                  #9 (phone), 10 (email)
                   'Id',                                       #9 (phone), 10 (email)
                   'User.UserAttributes.City',                 #5
                   'User.UserAttributes.Region',               #6
                   'User.UserAttributes.PostalCode',           #7
                   'Location.Country',                         #8
                   'User.UserAttributes.Salutation',           #0
                   'User.UserAttributes.FirstName',            #1
                   'User.UserAttributes.LastName',             #2
                   'User.UserAttributes.Title',                #3
                   'User.UserAttributes.StreetAddress',        #4
                   'User.UserAttributes.ContactRecordType',    #11
                   'User.UserAttributes.AccountName',          #12
                   'User.UserAttributes.AccountOwner',         #13
                   'User.UserAttributes.LeadSource',           #14
                   'User.UserId'                               #not in input
               ]
   
   # You probably don't need to change any variables below this point.
   AWS_REGION = os.environ['region']
   projectId = os.environ['projectId']
   processed_folder = "processed"
   startTime = datetime.now()
   s3 = s3fs.S3FileSystem(anon=False)
   
   def lambda_handler(event, context):
       print("Received event: " + str(event))
       
       for record in event['Records']:
           # Create some variables that make it easier to work with the data in the
           # event record.
           bucket = record['s3']['bucket']['name']
           key = record['s3']['object']['key']
           input_file = os.path.join(bucket,key)
           output_file_name = os.path.splitext(os.path.basename(input_file))[0] + "_processed.csv"
           processed_subfolder = os.path.basename(input_file).split("__part",1)[0]
           output_fullpath = os.path.join(bucket,processed_folder,processed_subfolder,output_file_name)
           # Start the function that processes the incoming data.
           process_incoming_file(input_file, output_fullpath)
   
   # Check the current project to see if an endpoint ID already exists.
   def check_endpoint_exists(endpointId):
       client = boto3.client('pinpoint',region_name=AWS_REGION)
   
       try:
           response = client.get_endpoint(
               ApplicationId=projectId,
               EndpointId=endpointId
           )
       except ClientError as e:
           endpointInfo = [ False, "" ]
       else:
           userId = response['EndpointResponse']['User']['UserId']
           endpointInfo = [ True, userId ]
   
       return endpointInfo
   
   # Change the column names, validate and reformat some of the input, and then 
   # write to output files.
   def process_incoming_file(input_file, output_file):
       # Counters for tracking the number of records and endpoints processed.
       line_count = 0
       create_count = 0
       update_count = 0
       
       folder = os.path.split(output_file)[0]
       
       with s3.open(output_file, 'w', newline='', encoding='utf-8-sig') as outFile:
           fileWriter = csv.writer(outFile)
           with s3.open(input_file, 'r', newline='', encoding='utf-8-sig') as inFile:
               fileReader = csv.reader(inFile)
       
               for row in fileReader:
                   # Sleep to prevent throttling errors.
                   time.sleep(.025)
                   # Write the header row.
                   if (line_count == 0):
                       fileWriter.writerow(header)
                       line_count += 1
                   # Write the rest of the data.
                   else:
                       # Generate a new UUID.
                       userId = str(uuid.uuid4())
       
                       # Varibles that make things easier to read.
                       inputEmail = row[10]
                       inputPhone = row[9]
                       inputCountry = row[8]
       
                       # If a country is included in the incoming record, create a
                       # variable that contains the ISO 3166-1 alpha-2 country code.
                       if inputCountry:
                           country = i18n.get_country_code(inputCountry, defaultCountry)
                       # If no country code is provided, create a variable that contains a
                       # default country code. Change this if you want to use a different
                       # default value.
                       elif not inputCountry:
                           country = defaultCountry
       
                       if inputEmail:
       
                           emailEndpointInfo = check_endpoint_exists(inputEmail)
       
                           if emailEndpointInfo[0]:
                               userId = emailEndpointInfo[1]
                               update_count += 1
                           else:
                               create_count += 1
       
                           fileWriter.writerow([
                                                   "EMAIL",
                                                   row[10],
                                                   row[10],
                                                   row[5],     #City
                                                   row[6],     #Region
                                                   row[7],     #Postal code
                                                   country,    #Country
                                                   row[0],     #Salutation
                                                   row[1],     #First name
                                                   row[2],     #Last name
                                                   row[3],     #Title
                                                   row[4],     #Street address
                                                   row[11],    #Contact record type
                                                   row[12],    #Account name
                                                   row[13],    #Account owner
                                                   row[14],    #Lead source
                                                   userId
                                               ])
       
                       if inputPhone:
                           phoneInfo = i18n.check_phone_number(country, inputPhone, AWS_REGION)
                           cleansedPhone = phoneInfo[0]
                           phoneType = phoneInfo[1]
       
                           if (phoneType == "MOBILE") or (phoneType == "PREPAID"):
                               phoneType = "SMS"
                           else:
                               phoneType = "VOICE"
       
                           phoneEndpointInfo = check_endpoint_exists(cleansedPhone)
       
                           if phoneEndpointInfo[0]:
                               userId = phoneEndpointInfo[1]
                               update_count += 1
                           else:
                               create_count += 1
       
                           fileWriter.writerow([
                                                   phoneType,
                                                   cleansedPhone,
                                                   cleansedPhone,
                                                   row[5],     #City
                                                   row[6],     #Region
                                                   row[7],     #Postal code
                                                   country,    #Country
                                                   row[0],     #Salutation
                                                   row[1],     #First name
                                                   row[2],     #Last name
                                                   row[3],     #Title
                                                   row[4],     #Street address
                                                   row[11],    #Contact record type
                                                   row[12],    #Account name
                                                   row[13],    #Account owner
                                                   row[14],    #Lead source
                                                   userId
                                               ])
                       line_count += 1
       
       # Calculate the amount of time the script ran.
       duration = datetime.now() - startTime
       
       # Print the number of records processed. Subtract 1 to account for the header.
       print("Processed " + str(line_count - 1) + " records in " + str(duration)
               + ". ", end="")
       
       # Print the numbers of endpoints created and updated.
       print("Found " + str(create_count) + " new endpoints and "
               + str(update_count) + " existing endpoints.")
   
       s3.rm(input_file)
   ```

1. In the function editor, on the **File** menu, choose **Save As**\. Save the file as `lambda_function.py` in the root directory of the function\.

1. In the function editor, on the **File** menu, choose **New File**\. The editor creates a new file named `Untitled1`\.

1. Paste the following code in the editor:

   ```
   import re
   import boto3
   from botocore.exceptions import ClientError
   
   def get_country_code(country, default):
       # The Location.Country attribute expects an ISO 3166-1 Alpha 2 formatted
       # country name. This function attempts to identify several possible
       # variations of each country name and convert them to the required format.
       # This function exists so that the program doesn't need to rely on non-
       # standard libraries. Update this section to suit the data in your existing
       # content management system.
       # This section includes common aliases for the 10 most populous countries
       # in the world, and some additional countries where Amazon Pinpoint is often used to
       # send SMS messages. If necessary, expand this section to include
       # additional countries, or additional aliases for the countries that are
       # already listed.
       country = country.strip().lower()
   
       bangladeshAliases = [ "bd", "bgd", "bangladesh", "বাংলাদেশ" ]
       brazilAliases     = [ "br", "bra", "brazil", "brasil",
                             "república federativa do brasil" ]
       canadaAliases     = [ "ca", "can", "canada" ]
       chinaAliases      = [ "cn", "chn", "prc", "proc", "china",
                             "people's republic of china", "中华人民共和国", "中国" ]
       indiaAliases      = [ "in", "ind", "india", "republic of india" ]
       indonesiaAliases  = [ "id", "idn", "indonesia", "republic of indonesia",
                             "republik indonesia" ]
       irelandAliases    = [ "ie", "irl", "ireland", "éire" ]
       japanAliases      = [ "jp", "jpn", "japan", "日本" ]
       mexicoAliases     = [ "mx", "mex", "mexico", "méxico",
                             "estados unidos mexicanos" ]
       nigeriaAliases    = [ "ng", "nga", "nigeria" ]
       pakistanAliases   = [ "pk", "pak", "pakistan", "پاکستان" ]
       russiaAliases     = [ "ru", "rus", "russian federation", "росси́я",
                             "росси́йская федера́ция" ]
       ukAliases         = [ "uk", "gb", "gbr", "united kingdom", "britain",
                             "england", "wales", "scotland", "northern ireland" ]
       usAliases         = [ "us", "usa", "united states",
                             "united states of america" ]
   
       if country in bangladeshAliases:
           countryISO3166 = "BD"
       elif country in brazilAliases:
           countryISO3166 = "BR"
       elif country in canadaAliases:
           countryISO3166 = "CA"
       elif country in chinaAliases:
           countryISO3166 = "CN"
       elif country in indiaAliases:
           countryISO3166 = "IN"
       elif country in indonesiaAliases:
           countryISO3166 = "ID"
       elif country in irelandAliases:
           countryISO3166 = "IE"
       elif country in japanAliases:
           countryISO3166 = "JP"
       elif country in mexicoAliases:
           countryISO3166 = "MX"
       elif country in nigeriaAliases:
           countryISO3166 = "NG"
       elif country in pakistanAliases:
           countryISO3166 = "PK"
       elif country in russiaAliases:
           countryISO3166 = "RU"
       elif country in ukAliases:
           countryISO3166 = "GB"
       elif country in usAliases:
           countryISO3166 = "US"
       else:
           countryISO3166 = default
   
       return countryISO3166
   
   def check_phone_number(country, phone, region):
   
       client = boto3.client('pinpoint',region_name=region)
   
       # Phone number validation can generate an E.164-compliant phone number as
       # long as you provide it with the correct country code. This function looks
       # for the appropriate country code at the beginning of the phone number. If
       # the country code isn't present, it adds it to the beginning of the phone
       # number that was provided to the function, and then sends it to the phone
       # number validation service. The phone number validation service performs
       # additional cleansing of the phone number, removing things like
       # unnecessary leading digits. It also provides metadata, such as the phone
       # number type (mobile, landline, etc.).
       # Add more countries and regions to this function if necessary.
       phone =  re.sub("[^0-9]", "", phone)
   
       if (country == 'BD') and not phone.startswith('880'):
           phone = "+880" + phone
       elif (country == 'BR') and not phone.startswith('55'):
           phone = "+55" + phone
       elif (country == 'CA' or country == 'US') and not phone.startswith('1'):
           # US and Canada (country code 1) area codes and phone numbers can't use
           # 1 as their first digit, so it's fine to search for a 1 at the
           # beginning of the phone number to determine whether or not the number
           # contains a country code.
           phone = "+1" + phone
       elif (country == 'CN') and not phone.startswith('86'):
           phone = "+86" + phone
       elif (country == 'IN') and not phone.startswith('91'):
           phone = "+91" + phone
       elif (country == 'ID') and not phone.startswith('62'):
           phone = "+62" + phone
       elif (country == 'IE') and not phone.startswith('353'):
           phone = "+353" + phone
       elif (country == 'JP') and not phone.startswith('81'):
           phone = "+81" + phone
       elif (country == 'MX') and not phone.startswith('52'):
           phone = "+52" + phone
       elif (country == 'NG') and not phone.startswith('234'):
           phone = "+234" + phone
       elif (country == 'PK') and not phone.startswith('92'):
           phone = "+92" + phone
       elif (country == 'RU') and not phone.startswith('7'):
           # No area codes in Russia begin with 7. However, Kazakhstan also uses
           # country code 7, and Kazakh area codes can begin with 7. If your
           # contact database contains Kazakh phone numbers, you might have to
           # use some additional logic to identify them.
           phone = "+7" + phone
       elif (country == 'GB') and not phone.startswith('44'):
           phone = "+44" + phone
   
       try:
           response = client.phone_number_validate(
               NumberValidateRequest={
                   'IsoCountryCode': country,
                   'PhoneNumber': phone
               }
           )
       except ClientError as e:
           print(e.response)
       else:
           returnValues = [
               response['NumberValidateResponse']['CleansedPhoneNumberE164'],  response['NumberValidateResponse']['PhoneType']
           ]
           return returnValues
   ```

1. In the function editor, on the **File** menu, choose **Save As**\. Save the file as `input_internationalization.py` in the root directory of the function\.

1. Under **Environment variables**, do the following:
   + In the first row, create a variable with a key of **projectId**\. Next, set the value to the unique ID of the project that you specified in the IAM policy in [Step 2\.2](tutorials-importing-data-create-iam-roles.md#tutorials-importing-data-create-iam-roles-pinpoint)\.
   + In the second row, create a variable with a key of **region**\. Next, set the value to the AWS Region that you specified in the IAM policy in [Step 2\.2](tutorials-importing-data-create-iam-roles.md#tutorials-importing-data-create-iam-roles-pinpoint)\.

   When you finish, the **Environment Variables** section should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_LAM_Step4.1_7.png)

1. Under **Basic settings**, set the **Timeout** value to 6 minutes\.
**Note**  
Each call to the phone number validation service typically takes about 0\.5 seconds to complete, but it can occasionally take longer\. If you don't increase the **Timeout** value, the function might time out before it finishes processing the incoming records\. This setting gives the Lambda function plenty of time to finish running\.

1. Under **Concurrency**, choose **Reserve concurrency**, and then set the value to `20`\.
**Note**  
A higher concurrency value might cause the files to be processed faster\. However, if the concurrency value is too high, you start to generate a number of PUT events that exceeds Amazon S3 quotas\. As a result, the import process doesn't capture all of the data in your input spreadsheet\. This is because many of the requests that are generated by this function end in failure\.

1. At the top of the page, choose **Save**\.

## Step 5\.2: Test the function<a name="tutorials-importing-data-lambda-function-process-incoming-test"></a>

After you create the function, you should test it to make sure that it's set up correctly\.

**To test the Lambda function**

1. Choose **Test**\. On the **Configure test event** window, for **Event name**, enter **TestEvent**\. Then, in the editor, paste the following code:

   ```
   {
     "Records": [
       {
         "s3": {
           "bucket": {
             "name": "bucket-name",
             "arn": "arn:aws:s3:::bucket-name"
           },
           "object": {
             "key": "to_process/testfile__part1__of1.csv"
           }
         }
       }
     ]
   }
   ```

   In the preceding example, replace *bucket\-name* with the name of the Amazon S3 bucket that you created in [Step 1](tutorials-importing-data-create-iam-roles.md)\. When you finish, choose **Create**\.

1. Choose **Test** again\. The function executes with the test event that you provided\.

   If the function runs as expected, proceed to the next step\.

   If the function fails to complete, do the following: 
   + Make sure that you specified the correct bucket name in the IAM policy that you created in [Step 1: Create an Amazon S3 bucket](tutorials-importing-data-create-s3-bucket.md)\.
   + Make sure that the Lambda test event that you created in step 1 of this section refers to the correct bucket and file name\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   Choose the bucket that you created in [Step 1: Create an Amazon S3 bucket](tutorials-importing-data-create-s3-bucket.md)\.

   Open the folders in the bucket and take note of the contents of each one\. If all of the following statements are true, then the Lambda function worked as expected:
   + The `to_process` folder doesn't contain any files\.
   + The processed folder contains a subfolder named `testfile`\. Within the `testfile` folder, there is a file named `testfile__part1__of1_processed.csv`\. 

   Don't delete any of the newly generated files\. The Lambda function that you create in the next step uses the files in the `to_process` folder\.

**Next**: [Create the Lambda function imports records into Amazon Pinpoint](tutorials-importing-data-lambda-function-import-job.md)
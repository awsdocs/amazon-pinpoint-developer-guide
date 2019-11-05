# Step 4: Create the Lambda Function That Splits Input Data<a name="tutorials-importing-data-lambda-function-input-split"></a>

The solution that's described in this tutorial uses three Lambda functions\. The first Lambda function is triggered when you upload a file to a specific Amazon S3 bucket\. This function reads the content of the input file, and then breaks it into smaller parts\. Other functions \(which you create later in this tutorial\) process these incoming files concurrently\. Processing the files concurrently reduces the amount of time that it takes to import all of the endpoints into Amazon Pinpoint\.

## Step 4\.1: Create the Function<a name="tutorials-importing-data-lambda-function-input-split-create"></a>

To create the first Lambda function for this tutorial, you first have to upload the \.zip file that you created in [Step 3](tutorials-importing-data-create-python-package.md)\. Next, you set up the function itself\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Choose **Author from scratch**\. Under **Basic information**, do the following:
   + For **Function name**, enter **CustomerImport\_ReadIncomingAndSplit**\.
   + For **Runtime**, choose **Python 3\.7**\.
   + For **Execution role**, choose **Use an existing role**\.
   + For **Existing role**, choose **ImporterS3Role**\.
   + Choose **Create function**\. 

1. Under **Function code**, for **Code entry type**, choose **Upload a \.zip file**\. Under **Function package**, choose **Upload**\. Choose the `pinpoint-importer.zip` file that you created in [Step 3](tutorials-importing-data-create-python-package.md)\. After you select the file, choose **Save**\. 
**Note**  
After you choose **Save**, you receive an error message stating that Lambda couldn’t open the file `lambda_function.py`\. Dismiss this error—you create this file in the next step\.

1. In the function editor, on the **File** menu, choose **New File**\. The editor creates a new file named `Untitled1`\.

1. Paste the following code in the editor:

   ```
   import os
   import boto3
   import s3fs
   from botocore.exceptions import ClientError
   
   input_archive_folder = "input_archive"
   to_process_folder = "to_process"
   file_row_limit = 50
   file_delimiter = ','
   
   # S3 bucket info
   s3 = s3fs.S3FileSystem(anon=False)
       
   def lambda_handler(event, context):
       print("Received event: \n" + str(event))
       for record in event['Records']:
           # Assign some variables that make it easier to work with the data in the 
           # event record.
           bucket = record['s3']['bucket']['name']
           key = record['s3']['object']['key']
           input_file = os.path.join(bucket,key)
           archive_path = os.path.join(bucket,input_archive_folder,os.path.basename(key))
           folder =  os.path.split(key)[0]
           s3_url = os.path.join(bucket,folder)
           output_file_template = os.path.splitext(os.path.basename(key))[0] + "__part"
           output_path = os.path.join(bucket,to_process_folder)
           
           # Set a variable that contains the number of files that this Lambda 
           # function creates after it runs.
           num_files = file_count(s3.open(input_file, 'r'), file_delimiter, file_row_limit)
           
           # Split the input file into several files, each with 50 rows.
           split(s3.open(input_file, 'r'), file_delimiter, file_row_limit, output_file_template, output_path, True, num_files)
           
           # Send the unchanged input file to an archive folder.
           archive(input_file,archive_path)
       
   # Determine the number of files that this Lambda function will create.
   def file_count(file_handler, delimiter, row_limit):
       import csv 
       reader = csv.reader(file_handler, delimiter=delimiter)
       # Figure out the number of files this function will generate.
       row_count = sum(1 for row in reader) - 1
       # If there's a remainder, always round up.
       file_count = int(row_count // row_limit) + (row_count % row_limit > 0)
       return file_count
   
   # Split the input into several smaller files.
   def split(filehandler, delimiter, row_limit, output_name_template, output_path, keep_headers, num_files):
       import csv 
       reader = csv.reader(filehandler, delimiter=delimiter)
       
       current_piece = 1
       current_out_path = os.path.join(
            output_path,
            output_name_template + str(current_piece) + "__of" + str(num_files) + ".csv"
       )
       current_out_writer = csv.writer(s3.open(current_out_path, 'w'), delimiter=delimiter)
       current_limit = row_limit
       if keep_headers:
           headers = next(reader)
           current_out_writer.writerow(headers)
       for i, row in enumerate(reader):
           if i + 1 > current_limit:
               current_piece += 1
               current_limit = row_limit * current_piece
               current_out_path = os.path.join(
                  output_path,
                  output_name_template + str(current_piece) + "__of" + str(num_files) + ".csv"
               )
               current_out_writer = csv.writer(s3.open(current_out_path, 'w'), delimiter=delimiter)
               if keep_headers:
                   current_out_writer.writerow(headers)
           current_out_writer.writerow(row)
   
   # Move the original input file into an archive folder.
   def archive(input_file, archive_path):
       s3.copy_basic(input_file,archive_path)
       print("Moved " + input_file + " to " + archive_path)
       s3.rm(input_file)
   ```

1. In the function editor, on the **File** menu, choose **Save As**\. Save the file as `lambda_function.py` in the root directory of the function\.

1. At the top of the page, choose **Save**\.

## Step 4\.2: Test the Function<a name="tutorials-importing-data-lambda-function-input-split-test"></a>

After you create the function, you should test it to make sure that it's set up correctly\.

**To test the Lambda function**

1. In a text editor, create a new file\. In the file, paste the following text:

   ```
   Salutation,First Name,Last Name,Title,Mailing Street,Mailing City,Mailing State/Province,Mailing Zip/Postal Code,Mailing Country,Phone,Email,Contact Record Type,Account Name,Account Owner,Lead Source
   Mr.,Alejandro,Rosales,Operations Manager,414 Main Street,Anytown,AL,95762,United States,+18705550156,arosales@example.com,Customer,Example Corp.,Richard Roe,Website
   Ms.,Ana Carolina,Silvia,Customer Service Representative,300 First Avenue,Any Town,AK,65141,United States,(727) 555-0128,asilvia@example.com,Qualified Lead,AnyCompany,Jane Doe,Seminar
   Mrs.,Juan,Li,Auditor,717 Kings Street,Anytown,AZ,18162,United States,768.555.0122,jli@example.com,Unqualified Lead,Example Corp.,Richard Roe,Phone
   Dr.,Arnav,Desai,Senior Analyst,782 Park Court,Anytown,AR,27084,United States,+17685550162,adesai@example.com,Customer,Example Corp.,Richard Roe,Website
   Mr.,Mateo,Jackson,Sales Representative,372 Front Street,Any Town,CA,83884,United States,(781) 555-0169,mjackson@example.com,Customer,AnyCompany,Jane Doe,Seminar
   Mr.,Nikhil,Jayashankar,Executive Assistant,468 Fifth Avenue,Anytown,CO,75376,United States,384.555.0178,njayashankar@example.com,Qualified Lead,Example Corp.,Jane Doe,Website
   Mrs.,Shirley,Rodriguez,Account Manager,287 Park Avenue,Any Town,CT,26715,United States,+12455550188,srodriguez@example.com,Qualified Lead,Example Corp.,Richard Roe,Seminar
   Ms.,Xiulan,Wang,Information Architect,107 Queens Place,Anytown,DE,70710,United States,(213) 555-0192,xwang@example.com,Unqualified Lead,Example Corp.,Richard Roe,Phone
   Miss,Saanvi,Sarkar,Director of Finance,273 Sample Boulevard,Any Town,FL,85431,United States,237.555.0121,ssarkar@example.com,Customer,AnyCompany,Richard Roe,Referral
   Mr.,Wei,Zhang,Legal Counsel,15 Third Avenue,Any Town,GA,82387,United States,+18065550179,wzhang@example.com,Customer,AnyCompany,Jane Doe,Website
   ```

   Save the file as `testfile.csv`\.
**Note**  
This file contains fictitious contact records\. You only use it to test the Lambda function that you create in this tutorial\. Later, you can delete the segments that contain this fictitious data\.  
For now, don't add or remove any columns to the file\. After you implement the solution that's shown in this tutorial, you can modify it to meet your needs\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, choose the bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md), and then choose the `input` folder\.

1. Choose **Upload**\. Upload the `testfile.csv` file that you just created\.

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the list of functions, choose the **CustomerImport\_ReadIncomingAndSplit** function that you created earlier\.

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
             "key": "input/testfile.csv"
           }
         }
       }
     ]
   }
   ```

   In the preceding example, replace *bucket\-name* with the name of the Amazon S3 bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md)\. When you finish, choose **Create**\.

1. Choose **Test** again\. The function executes with the test event that you provided\.

   If the function runs as expected, proceed to the next step\.

   If the function fails to complete, do the following: 
   + Make sure that you specified the correct bucket name in the IAM policy that you created in [Step 2: Create IAM Roles](tutorials-importing-data-create-iam-roles.md)\.
   + Make sure that the Lambda test event that you created in step 7 of this section refers to the correct bucket and file name\.
   + If you named the input file something other than `testfile.csv`, make sure that the file name doesn't contain any spaces\.

1. Return to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\. Choose the bucket that you created in [Step 1: Create an Amazon S3 Bucket](tutorials-importing-data-create-s3-bucket.md)\.

   Open the folders in the bucket and take note of the contents of each one\. If all of the following statements are true, then the Lambda function worked as expected:
   + The `input` folder doesn't contain any files\.
   + The `input_archive` folder contains the file that you uploaded in step 4 of this section\.
   + The `to_process` folder contains a file named `testfile__part1__of1.csv`\.

   Don't delete any of the newly generated files\. The Lambda function that you create in the next step uses the files in the `to_process` folder\.

**Next**: [Create a Lambda Function That Processes the Incoming Records](tutorials-importing-data-lambda-function-process-incoming.md)
# Step 3: Create Lambda functions<a name="tutorials-two-way-sms-part-3"></a>

This solution uses two Lambda functions\. This section shows you how to create and configure these functions\. Later, you set up API Gateway and Amazon Pinpoint to execute these functions when certain events occur\. Both of these functions create and update endpoints in the Amazon Pinpoint project that you specify\. The first function also uses the phone number validation feature\.

## Step 3\.1: Create the function that validates customer information and creates endpoints<a name="tutorials-two-way-sms-part-3-create-register-function"></a>

The first function takes input from your registration form, which it receives from Amazon API Gateway\. It uses this information to obtain information about the customer's phone number by using the [phone number validation](validate-phone-numbers.md) feature of Amazon Pinpoint\. The function then uses the validated data to create a new endpoint in the Amazon Pinpoint project that you specify\. By default, the endpoint that the function creates is opted out of future communications from you, but this status can be changed by the second function\. Finally, this function sends the customer a message asking them to verify that they want to receive SMS communications from you\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Under **Create a function**, choose **Blueprints**\.

1. In the search field, enter **hello**, and then press Enter\. In the list of results, choose the `hello-world` Node\.js function, as shown in the following image\. Choose **Configure**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_LAM_Step1.4.png)

1. Under **Basic information**, do the following:
   + For **Name**, enter a name for the function, such as **RegistrationForm**\.
   + For **Role**, select **Choose an existing role**\.
   + For **Existing role**, choose the **SMSRegistrationForm** role that you created in [Step 2\.2](tutorials-two-way-sms-part-2.md#tutorials-two-way-sms-part-2-create-role)\.

   When you finish, choose **Create function**\.

1. Delete the sample function in the code editor, and then paste the following code:

   ```
   var AWS = require('aws-sdk');
   var pinpoint = new AWS.Pinpoint({region: process.env.region}); 
   
   // Make sure the SMS channel is enabled for the projectId that you specify.
   // See: https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-setup.html
   var projectId = process.env.projectId;
   
   // You need a dedicated long code in order to use two-way SMS. 
   // See: https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-voice-manage.html#channels-voice-manage-request-phone-numbers
   var originationNumber = process.env.originationNumber;
   
   // This message is spread across multiple lines for improved readability.
   var message = "ExampleCorp: Reply YES to confirm your subscription. 2 msgs per "
               + "month. No purchase req'd. Msg&data rates may apply. Terms: "
               + "example.com/terms-sms";
               
   var messageType = "TRANSACTIONAL";
   
   exports.handler = (event, context, callback) => {
     console.log('Received event:', event);
     validateNumber(event);
   };
   
   function validateNumber (event) {
     var destinationNumber = event.destinationNumber;
     if (destinationNumber.length == 10) {
       destinationNumber = "+1" + destinationNumber;
     }
     var params = {
       NumberValidateRequest: {
         IsoCountryCode: 'US',
         PhoneNumber: destinationNumber
       }
     };
     pinpoint.phoneNumberValidate(params, function(err, data) {
       if (err) {
         console.log(err, err.stack);
       }
       else {
         console.log(data);
         //return data;
         if (data['NumberValidateResponse']['PhoneTypeCode'] == 0) {
           createEndpoint(data, event.firstName, event.lastName, event.source);
         } else {
           console.log("Received a phone number that isn't capable of receiving "
                      +"SMS messages. No endpoint created.");
         }
       }
     });
   }
   
   function createEndpoint(data, firstName, lastName, source) {
     var destinationNumber = data['NumberValidateResponse']['CleansedPhoneNumberE164'];
     var endpointId = data['NumberValidateResponse']['CleansedPhoneNumberE164'].substring(1);
     
     var params = {
       ApplicationId: projectId,
       // The Endpoint ID is equal to the cleansed phone number minus the leading
       // plus sign. This makes it easier to easily update the endpoint later.
       EndpointId: endpointId,
       EndpointRequest: {
         ChannelType: 'SMS',
         Address: destinationNumber,
         // OptOut is set to ALL (that is, endpoint is opted out of all messages)
         // because the recipient hasn't confirmed their subscription at this
         // point. When they confirm, a different Lambda function changes this 
         // value to NONE (not opted out).
         OptOut: 'ALL',
         Location: {
           PostalCode:data['NumberValidateResponse']['ZipCode'],
           City:data['NumberValidateResponse']['City'],
           Country:data['NumberValidateResponse']['CountryCodeIso2'],
         },
         Demographic: {
           Timezone:data['NumberValidateResponse']['Timezone']
         },
         Attributes: {
           Source: [
             source
           ]
         },
         User: {
           UserAttributes: {
             FirstName: [
               firstName
             ],
             LastName: [
               lastName
             ]
           }
         }
       }
     };
     pinpoint.updateEndpoint(params, function(err,data) {
       if (err) {
         console.log(err, err.stack);
       }
       else {
         console.log(data);
         //return data;
         sendConfirmation(destinationNumber);
       }
     });
   }
   
   function sendConfirmation(destinationNumber) {
     var params = {
       ApplicationId: projectId,
       MessageRequest: {
         Addresses: {
           [destinationNumber]: {
             ChannelType: 'SMS'
           }
         },
         MessageConfiguration: {
           SMSMessage: {
             Body: message,
             MessageType: messageType,
             OriginationNumber: originationNumber
           }
         }
       }
     };
   
     pinpoint.sendMessages(params, function(err, data) {
       // If something goes wrong, print an error message.
       if(err) {
         console.log(err.message);
       // Otherwise, show the unique ID for the message.
       } else {
         console.log("Message sent! " 
             + data['MessageResponse']['Result'][destinationNumber]['StatusMessage']);
       }
     });
   }
   ```

1. Under **Environment variables**, do the following:
   + In the first row, create a variable with a key of **originationNumber**\. Next, set the value to the phone number of the dedicated long code that you received in [Step 1\.2](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-set-up-channel)\.
**Note**  
Be sure to include the plus sign \(\+\) and the country code for the phone number\. Don't include any other special characters, such as dashes \(\-\), periods \(\.\), or parentheses\.
   + In the second row, create a variable with a key of **projectId**\. Next, set the value to the unique ID of the project that you created in [Step 1\.1](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-create-project)\.
   + In the third row, create a variable with a key of **region**\. Next, set the value to the Region that you use Amazon Pinpoint in, such as **us\-east\-1** or **us\-west\-2**\.

   When you finish, the **Environment Variables** section should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_LAM_Step1.7.png)

1. At the top of the page, choose **Save**\.

### Step 3\.1\.1: Test the function<a name="tutorials-two-way-sms-part-3-create-register-function-test"></a>

After you create the function, you should test it to make sure that it's configured properly\. Also, make sure that the IAM role you created has the appropriate permissions\.

**To test the function**

1. Choose **Test**\.

1. On the **Configure test event** window, do the following:
   + Choose **Create new test event**\.
   + For **Event name**, enter a name for the test event, such as **MyPhoneNumber**\.
   + Erase the example code in the code editor\. Paste the following code:

     ```
     {
       "destinationNumber": "2065550142",
       "firstName": "Carlos",
       "lastName": "Salazar",
       "source": "Registration form test"
     }
     ```
   + In the preceding code example, replace the values of the `destinationNumber`, `firstName`, and `lastName` attributes with the values that you want to use for testing, such as your personal contact details\. When you test this function, it sends an SMS message to the phone number that you specify in the `destinationNumber` attribute\. Make sure that the phone number that you specify is able to receive SMS messages\.
   + Choose **Create**\.

1. Choose **Test** again\.

1. Under **Execution result: succeeded**, choose **Details**\. In the **Log output** section, review the output of the function\. Make sure that the function ran without errors\.

   Check the device that's associated with the `destinationNumber` that you specified to make sure that it received the test message\.

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\. On the **Segments page**, choose **Create a segment**\.

1. In **Segment group 1**, under **Add filters to refine your segment**, choose **Filter by user**\.

1. For **Choose a user attribute**, choose **FirstName**\. Then, for **Choose values**, choose the first name that you specified in the test event\.

   The **Segment estimate** section should show that there are zero eligible endpoints, and one total endpoint, as shown in the following image\. This result is expected\. When the function creates a new endpoint, the endpoint is opted out\. Segments in Amazon Pinpoint automatically exclude opted\-out endpoints\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_LAM_Step8.9.png)

## Step 3\.2: Create the function that opts in customers to your communications<a name="tutorials-two-way-sms-part-3-create-optin-function"></a>

The second function is only executed when a customer replies to the message that's sent by the first function\. If the customer's reply includes the keyword that you specified in [Step 1\.3](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-set-up-channel), the function updates their endpoint record to opt them in to future communications\. Amazon Pinpoint also automatically responds with the message that you specified in Step 1\.3\.

If the customer doesn't respond, or responds with anything other than the designated keyword, then nothing happens\. The customer's endpoint remains in Amazon Pinpoint, but it can't be targeted by segments\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Under **Create function**, choose **Blueprints**\.

1. In the search field, enter **hello**, and then press Enter\. In the list of results, choose the `hello-world` Node\.js function, as shown in the following image\. Choose **Configure**\.

1. Under **Basic information**, do the following:
   + For **Name**, enter a name for the function, such as **RegistrationForm\_OptIn**\.
   + For **Role**, select **Choose an existing role**\.
   + For **Existing role**, choose the SMSRegistrationForm role that you created in [Step 2\.2](tutorials-two-way-sms-part-2.md#tutorials-two-way-sms-part-2-create-role)\.

   When you finish, choose **Create function**\.

1. Delete the sample function in the code editor, and then paste the following code:

   ```
   var AWS = require('aws-sdk');
   var projectId = process.env.projectId;
   var confirmKeyword = process.env.confirmKeyword.toLowerCase();
   var pinpoint = new AWS.Pinpoint({region: process.env.region});
   
   exports.handler = (event, context) => {
     console.log('Received event:', event);
     var timestamp = event.Records[0].Sns.Timestamp;
     var message = JSON.parse(event.Records[0].Sns.Message);
     var originationNumber = message.originationNumber;
     var response = message.messageBody.toLowerCase();
     
     if (response.includes(confirmKeyword)) {
       updateEndpointOptIn(originationNumber, timestamp);  
     }
   };
   
   function updateEndpointOptIn (originationNumber, timestamp) {
     var endpointId = originationNumber.substring(1);
     
     var params = {
       ApplicationId: projectId,
       EndpointId: endpointId,
       EndpointRequest: { 
         Address: originationNumber,
         ChannelType: 'SMS',
         OptOut: 'NONE',
         Attributes: {
           OptInTimestamp: [
             timestamp
           ]
         },
       }
     };
     pinpoint.updateEndpoint(params, function(err, data) {
       if (err) {
         console.log("An error occurred.\n");
         console.log(err, err.stack);
       }
       else {
         console.log("Successfully changed the opt status of endpoint ID " + endpointId);
       }
     });
   }
   ```

1. Under **Environment variables**, do the following:
   + In the first row, create a variable with a key of **projectId**\. Next, set the value to the unique ID of the project that you created in [Step 1\.1](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-create-project)\.
   + In the second row, create a variable with a key of **region**\. Next, set the value to the Region that you use Amazon Pinpoint in, such as **us\-east\-1** or **us\-west\-2**\.
   + In the third row, create a variable with a key of **confirmKeyword**\. Next, set the value to the confirmation keyword that you created in [Step 1\.3](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-set-up-channel)\.
**Note**  
The keyword isn't case sensitive\. This function converts the incoming message to lowercase letters\.

   When you finish, the **Environment Variables** section should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_LAM_Step2.7.png)

1. At the top of the page, choose **Save**\.

### Step 3\.2\.1: Test the function<a name="tutorials-two-way-sms-part-3-create-optin-function-test"></a>

After you create the function, you should test it to make sure that it's configured properly\. Also, make sure that the IAM role you created has the appropriate permissions\.

**To test the function**

1. Choose **Test**\.

1. On the **Configure test event** window, do the following:

   1. Choose **Create new test event**\.

   1. For **Event name**, enter a name for the test event, such as **MyResponse**\.

   1. Erase the example code in the code editor\. Paste the following code:

      ```
      {
        "Records":[
          {
            "Sns":{
              "Message":"{\"originationNumber\":\"+12065550142\",\"messageBody\":\"Yes\"}",
              "Timestamp":"2019-02-20T17:47:44.147Z"
            }
          }
        ]
      }
      ```

      In the preceding code example, replace the values of the `originationNumber` attribute with the phone number that you used when you tested the previous Lambda function\. Replace the value of `messageBody` with the two\-way SMS keyword that you specified in [Step 1\.3](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-enable-two-way)\. Optionally, you can replace the value of `Timestamp` with the current date and time\.

   1. Choose **Create**\.

1. Choose **Test** again\.

1. Under **Execution result: succeeded**, choose **Details**\. In the **Log output** section, review the output of the function\. Make sure that the function ran without errors\.

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-two-way-sms-part-1.md#tutorials-two-way-sms-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\. On the **Segments page**, choose **Create a segment**\.

1. In **Segment group 1**, under **Add filters to refine your segment**, choose **Filter by user**\.

1. For **Choose a user attribute**, choose **FirstName**\. Then, for **Choose values**, choose the first name that you specified in the test event\.

   The **Segment estimate** section should show that there is one eligible endpoint, and one total endpoint\.

**Next**: [Set up Amazon API Gateway](tutorials-two-way-sms-part-4.md)
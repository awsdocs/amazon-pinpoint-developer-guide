# Step 4: Create the Lambda Function<a name="tutorials-email-prefs-part-4"></a>

This solution uses a Lambda function to update customers' endpoint records based on the preferences that they provide on the web form\. This section shows you how to create, configure, and test this function\. Later, you set up API Gateway and Amazon Pinpoint to execute this function\.

## Step 4\.1: Create the Function That Updates Endpoints<a name="tutorials-email-prefs-part-4-create-register-function"></a>

The first function takes input from the preference management web form, which it receives from Amazon API Gateway\. It uses this information to update the customer's endpoint record according to the preferences that they provided on the form\.

**To create the Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Under **Create a function**, choose **Blueprints**\.

1. In the search field, enter **hello**, and then press Enter\. In the list of results, choose the `hello-world` Node\.js function, as shown in the following image\. Choose **Configure**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_LAM_Step1.4.png)

1. Under **Basic information**, do the following:
   + For **Name**, enter a name for the function, such as **EmailPreferences**\.
   + For **Role**, select **Choose an existing role**\.
   + For **Existing role**, choose the **EmailPreferencesForm** role that you created in [Step 3\.2](tutorials-email-prefs-part-3.md#tutorials-email-prefs-part-3-create-role)\.

   When you finish, choose **Create function**\.

1. Delete the example code in the code editor, and then paste the following code:

   ```
   var AWS = require('aws-sdk');
   var pinpoint = new AWS.Pinpoint({region: process.env.region}); 
   var projectId = process.env.projectId;
   
   exports.handler = (event, context, callback) => {
     console.log('Received event:', event);
     var optOutAll = event.optOutAll;
     
     if (optOutAll === false) { 
       updateOpt(event);
     } else if (optOutAll === true) {
       unsubAll(event);
     }
   };
   
   function unsubAll(event) {
     
     var params = {
       ApplicationId: projectId,
       EndpointId: event.endpointId,
       EndpointRequest: {
         ChannelType: 'EMAIL',
         OptOut: 'ALL',
         Attributes: {
           SpecialOffersOptStatus: [
             "OptOut"
           ],
           NewProductsOptStatus: [
             "OptOut"
           ],
           ComingSoonOptStatus: [
             "OptOut"
           ], 
           DealOfTheDayOptStatus: [
             "OptOut"
           ], 
           OptStatusLastChanged: [
             event.optTimestamp
           ], 
           OptSource: [
             event.source
           ]
         }
       }
     };
     pinpoint.updateEndpoint(params, function(err,data) {
       if (err) {
         console.log(err, err.stack);
       }
       else {
         console.log(data);
       }
     });
     
     
   }
   
   function updateOpt(event) {
     var endpointId = event.endpointId,
         firstName = event.firstName,
         lastName = event.lastName,
         source = event.source,
         specialOffersOptStatus = event.topic1,
         newProductsOptStatus = event.topic2,
         comingSoonOptStatus = event.topic3,
         dealOfTheDayOptStatus = event.topic4,
         optTimestamp = event.optTimestamp;
     
     var params = {
       ApplicationId: projectId,
       EndpointId: endpointId,
       EndpointRequest: {
         ChannelType: 'EMAIL',
         OptOut: 'NONE',
         Attributes: {
           SpecialOffersOptStatus: [
             specialOffersOptStatus
           ],
           NewProductsOptStatus: [
             newProductsOptStatus
           ],
           ComingSoonOptStatus: [
             comingSoonOptStatus
           ], 
           DealOfTheDayOptStatus: [
             dealOfTheDayOptStatus
           ], 
           OptStatusLastChanged: [
             optTimestamp
           ], 
           OptSource: [
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
       }
     });
   }
   ```

1. Under **Environment variables**, do the following:
   + In the first row, create a variable with a key of **projectId**\. Next, set the value to the unique ID of the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.
   + In the second row, create a variable with a key of **region**\. Next, set the value to the Region that you use Amazon Pinpoint in, such as **us\-east\-1** or **us\-west\-2**\.

   When you finish, the **Environment Variables** section should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_LAM_Step4.1_7.png)

1. At the top of the page, choose **Save**\.

### Step 4\.1\.1: Test the Function<a name="tutorials-email-prefs-part-4-create-register-function-test"></a>

After you create the function, you should test it to make sure that it's configured properly\. Also, make sure that the IAM role you created has the appropriate permissions\.

**To test the function**

1. Choose **Test**\.

1. On the **Configure test event** window, do the following:
   + Choose **Create new test event**\.
   + For **Event name**, enter a name for the test event, such as **MyTestEvent**\.
   + Erase the example code in the code editor\. Paste the following code:

     ```
     {
       "endpointId": "12345",
       "firstName": "Carlos",
       "lastName": "Salazar",
       "topic1": "OptOut",
       "topic2": "OptIn",
       "topic3": "OptOut",
       "topic4": "OptIn",
       "source": "Lambda test",
       "optTimestamp": "Tue Mar 05 2019 14:35:07 GMT-0800 (PST)",
       "optOutAll": false
     }
     ```
   + In the preceding code example, replace the values of the `endpointId`, `firstName`, and `lastName` attributes with the values for the endpoint that you imported in [Step 2](tutorials-email-prefs-part-2.md#tutorials-email-prefs-part-2-import)\.
   + Choose **Create**\.

1. Choose **Test** again\.

1. Under **Execution result: succeeded**, choose **Details**\. In the **Log output** section, review the output of the function\. Make sure that the function ran without errors\.

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\. On the **Segments page**, choose **Create a segment**\.

1. In **Segment group 1**, under **Add filters to refine your segment**, choose **Filter by endpoint**\.

1. For **Choose an endpoint attribute**, choose **NewProductsOptStatus**\. Then, for **Choose values**, choose **OptIn**\.

   The **Segment estimate** section should show that there is one eligible endpoint, and one total endpoint, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_LAM_Step4.1.1_9.png)

**Next**: [Set Up Amazon API Gateway](tutorials-email-prefs-part-5.md)
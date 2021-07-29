# Step 3: Send additional requests<a name="tutorials-using-postman-sample-requests"></a>

When you finish configuring and testing Postman, you can start sending additional requests to the Amazon Pinpoint API\. This section includes information that you need to know before you start sending requests\. It also includes two sample requests that help you understand how to use the Amazon Pinpoint collection\.

**Important**  
When you complete the procedures in this section, you submit requests to the Amazon Pinpoint API\. These requests are capable of creating new resources in your Amazon Pinpoint account, modifying existing resources, sending messages, changing the configuration of your Amazon Pinpoint projects, and using other Amazon Pinpoint features\. Use caution when you execute these requests\.

## About the examples in the Amazon Pinpoint Postman collection<a name="tutorials-using-postman-sample-requests-about"></a>

You have to configure most of the operations in the Amazon Pinpoint Postman collection before you can use them\. For `GET` and `DELETE` operations, you typically only need to modify the variables that are set on the **Pre\-request Script** tab\.

**Note**  
When you use the IAM policy that's shown in [Step 1\.1](tutorials-using-postman-iam-user.md#tutorials-using-postman-iam-user-create-policy), you can't execute any of the `DELETE` requests that are included in this collection\.

For example, the `GetCampaign` operation requires you to specify a `projectId` and a `campaignId`\. On the **Pre\-request Script** tab, both of these variables are present, and are populated with example values\. Delete the example values and replace them with the appropriate values for your Amazon Pinpoint project and campaign\.

Of these variables, the most commonly used is the `projectId` variable\. The value for this variable should be the unique identifier for the project that your request applies to\. To get a list of these identifiers for your projects, you can refer to the response to the `GetApps` request that you sent in the preceding step of this tutorial\. \(In Amazon Pinpoint, a "project" is the same thing as an "app" or "application\."\) In that response, the `Id` field provides the unique identifier for a project\. To learn more about the `GetApps` operation and the meaning of each field in the response, see [Apps](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps.html) in the *Amazon Pinpoint API Reference*\.

For `POST` and `PUT` operations, you also need to modify the request body to include the values that you want to send to the API\. For example, when you submit a `CreateApp` request \(which is a `POST` request\), you have to specify a name for the project that you create\. You can modify the request on the **Body** tab\. In this example, replace the value next to `"Name"` with the name of the project\. If you want to add tags to the project, you can specify them in the `tags` object\. Or, if you don't want to add tags, you can delete the entire `tags` object\.

**Note**  
The `UntagResource` operation also requires you to specify URL parameters\. You can specify these parameters on the **Params** tab\. Replace the values in the **VALUE** column with the tags that you want to delete for the specified resource\.

## Example request: Creating a project by using the `CreateApp` operation<a name="tutorials-using-postman-sample-requests-createapp"></a>

Before you create segments and campaigns in Amazon Pinpoint, you first have to create a project\. In Amazon Pinpoint, a *project* consists of segments, campaigns, configurations, and data that are united by a common purpose\. For example, you could use a project to contain all of the content that's related to a particular app, or to a specific brand or marketing initiative\. When you add customer information to Amazon Pinpoint, that information is associated with a project\.

**To create a project by sending a CreateApp API request**

1. On the **Environments** menu, choose the AWS Region that you want to create the project in, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_Environments.png)

1. In the **Apps** folder, choose the **CreateApp** operation, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_3.2_2.png)

1. On the **Body** tab, next to `"Name"`, replace the placeholder value \(`"string"`\) with a name for the campaign, such as **"MySampleProject"**\.

1. Delete the comma that after the campaign name, and then delete the entire `tags` object on lines 3 through 5\. When you finish, your request should resemble the example that's shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_3.2_4.png)

1. Choose **Send**\. If the campaign is created successfully, the response pane shows a status of `201 Created`\. You see a response that resembles the example in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_3.2_5.png)

## Example: Sending an email by using the `SendMessages` operation<a name="tutorials-using-postman-sample-requests-sendmessages"></a>

It's very common to use the Amazon Pinpoint `SendMessages` API to send transactional messages\. One advantage to sending messages by using the `SendMessages` API \(as opposed to creating campaigns\), is that you can use the `SendMessages` API to send messages to any address \(such as an email address, phone number, or device token\)\. The address that you send messages to doesn't have to exist in your Amazon Pinpoint account already\. Compare this to sending messages by creating campaigns\. Before you send a campaign in Amazon Pinpoint, you have to add endpoints to your Amazon Pinpoint account, create segments, create the campaign, and execute the campaign\.

The example in this section shows you how to send a transactional email message directly to a specific email address\. You can modify this request to send messages through other channels, such as SMS, mobile push, or voice\.

**To send an email message by submitting a SendMessages request**

1. Verify the email address or domain that you want to use to send the message\. For more information, see [Verifying email identities](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-email-manage-verify.html) in the *Amazon Pinpoint User Guide*\.
**Note**  
In Amazon Pinpoint, you can only send email from addresses or domains that you've verified\. You won't be able to complete the procedure in this section until you verify an email address\.

1. On the **Environments** menu, choose the AWS Region that you want to send the message from, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_Environments.png)

1. In the **Messages** folder, choose the **SendMessages** operation\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_3.3_3.png)

1. On the **Pre\-request Script** tab, replace the value of the `projectId` variable with the ID of a project that already exists in the Region that you selected in step 2 of this section\.

1. On the **Body** tab, delete the example request that's shown in the request editor\. Paste the following code:

   ```
   {
       "MessageConfiguration":{
           "EmailMessage":{
               "FromAddress":"sender@example.com",
               "SimpleEmail":{
                   "Subject":{
                       "Data":"Sample Amazon Pinpoint message"
                   },
                   "HtmlPart":{
                       "Data":"<h1>Test message</h1><p>This is a sample message sent from <a href=\"https://aws.amazon.com/pinpoint\">Amazon Pinpoint</a> using the SendMessages API.</p>"
                   },
                   "TextPart":{
                       "Data":"This is a sample message sent from Amazon Pinpoint using the SendMessages API."
                   }
               }
           }
       },
       "Addresses":{
           "recipient@example.com": {
               "ChannelType": "EMAIL"
           }
       }
   }
   ```

1. In the preceding code, replace *sender@example\.com* with your verified email address\. Replace *recipient@example\.com* with the address that you want to send the message to\.
**Note**  
If your account is still in the Amazon Pinpoint email sandbox, you can only send email to addresses or domains that are verified in your Amazon Pinpoint account\. For more information about having your account removed from the sandbox, see [ Requesting production access for email](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-email-setup-production-access.html) in the *Amazon Pinpoint User Guide*\.

1. Choose **Send**\. If the message is sent successfully, the response pane shows a status of `200 OK`\. You see a response that resembles the example in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_3.3_7.png)
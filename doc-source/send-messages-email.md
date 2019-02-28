# Send Transactional Email Messages<a name="send-messages-email"></a>

This section provides complete code samples that you can use to send transactional email messages through Amazon Pinpoint\. There are three methods that you can use to send transactional email messages:
+ [By using the SendMessages operation in the Amazon Pinpoint API](send-messages-sdk.md): You can use the `SendMessages` operation in the Amazon Pinpoint API to send messages in all of the channels that Amazon Pinpoint supports, including the push notification, SMS, voice, and email channels\.

  The advantage of using this operation is that the request syntax for sending messages is very similar across all channels\. This makes it easier to repurpose your existing code\. The `SendMessages` operation also lets you to substitute content in your email messages, and lets you send email to Amazon Pinpoint endpoint IDs rather than to specific email addresses\.
+ [By using the SendEmail operation in the Amazon Pinpoint Email API](send-messages-email-smtp.md): You can use the `SendEmail` operation in the Amazon Pinpoint Email API to send email messages only\.

  The advantage of using this operation is that the Amazon Pinpoint Email API includes additional options that are specific to the email channel\. For example, when you send an email by using the Amazon Pinpoint Email API, you can specify CC and BCC recipients\. You can also specify a configuration set\. You can use configuration sets to specify which of your dedicated IP addresses your emails should be sent from, or to specify how Amazon Pinpoint handles open and click events\.
+ [By using the Amazon Pinpoint SMTP interface](send-messages-email-smtp.md): You can use the Amazon Pinpoint SMTP interface to send email messages only\.

  The advantage to using the SMTP interface is that you can use email\-sending applications and libraries to send email\. For example, if you use a ticketing system that sends emails to your customers, you can use the SMTP interface to configure the system to send emails from your domain\. You can also use email\-sending libraries in several programming languages to send email through the SMTP interface\.

This section includes example code in several programming languages that you can use to start sending transactional emails\.

**Topics**
+ [Choosing an Email\-Sending Method](#send-messages-email-choose-method)
+ [Send Email by Using the Amazon Pinpoint API](send-messages-sdk.md)
+ [Send Email by Using the Amazon Pinpoint Email API](send-messages-email-sdk.md)
+ [Send Email by Using the Amazon Pinpoint SMTP Interface](send-messages-email-smtp.md)

## Choosing an Email\-Sending Method<a name="send-messages-email-choose-method"></a>

The best method to use for sending transactional email depends on your use case\. For example, if you need to send email by using a third\-party application, or if there isn't an AWS SDK available for your programming language, you might have to use the SMTP interface\. If you want to send messages in other channels that Amazon Pinpoint supports, and you want to use consistent code for making those requests, you should use the `SendMessages` operation in the Amazon Pinpoint API\. If you need to be able to send messages to CC and BCC recipients, or use dedicated IP pools, or specify message tags in your emails, you should use the `SendEmail` operation in the Amazon Pinpoint Email API\.

The following decision tree shows some of the factors that you should consider when you decide which method you use to send transactional email in Amazon Pinpoint\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/PinpointEmailDecisionTree.png)
# Send transactional email messages<a name="send-messages-email"></a>

This section provides complete code samples that you can use to send transactional email messages through Amazon Pinpoint:
+ [By using the SendMessages operation in the Amazon Pinpoint API](send-messages-sdk.md): You can use the `SendMessages` operation in the Amazon Pinpoint API to send messages in all of the channels that Amazon Pinpoint supports, including the push notification, SMS, voice, and email channels\.

  The advantage of using this operation is that the request syntax for sending messages is very similar across all channels\. This makes it easier to repurpose your existing code\. The `SendMessages` operation also lets you to substitute content in your email messages, and lets you send email to Amazon Pinpoint endpoint IDs rather than to specific email addresses\.

This section includes example code in several programming languages that you can use to start sending transactional emails\.

**Topics**
+ [Choosing an email\-sending method](#send-messages-email-choose-method)
+ [Choosing between Amazon Pinpoint and Amazon Simple Email Service \(SES\)](#send-email-ses)
+ [Send email by using the Amazon Pinpoint API](send-messages-sdk.md)

## Choosing an email\-sending method<a name="send-messages-email-choose-method"></a>

The best method to use for sending transactional email depends on your use case\. For example, if you need to send email by using a third\-party application, or if there isn't an AWS SDK available for your programming language, you might have to use the SMTP interface\. If you want to send messages in other channels that Amazon Pinpoint supports, and you want to use consistent code for making those requests, you should use the `SendMessages` operation in the Amazon Pinpoint API\.

## Choosing between Amazon Pinpoint and Amazon Simple Email Service \(SES\)<a name="send-email-ses"></a>

If you send a large number of transactional emails, such as purchase confirmations or password reset messages, consider using Amazon SES\. Amazon SES has an API and an SMTP interface, both of which are well suited to sending email from your applications or services\. It also offers additional email features, including email receiving capabilities, configuration sets, and sending authorization capabilities\.

Amazon SES also includes an SMTP interface that you can integrate with your existing third\-party applications, including customer relationship management \(CRM\) services such as Salesforce\. For more information about sending email using Amazon SES, [Amazon Simple Email Service Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html) for more information\.
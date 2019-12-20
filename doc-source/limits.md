# Limits in Amazon Pinpoint<a name="limits"></a>

The following sections describe limits in Amazon Pinpoint\.

**Topics**
+ [General Limits](#limits-general)
+ [Campaign Limits](#limits-campaign)
+ [Email Limits](#limits-email)
+ [Endpoint Limits](#limits-endpoint)
+ [Endpoint Import Limits](#limits-import)
+ [Event Ingestion Limits](#limits-events)
+ [Journey Limits](#limits-journeys)
+ [Message Template Limits](#limits-message-templates)
+ [Mobile Push Limits](#limits-mobile)
+ [Segment Limits](#limits-segment)
+ [SMS Limits](#limits-sms)
+ [Voice Limits](#limits-voice)
+ [Requesting a Limit Increase](#limits-increase)

## General Limits<a name="limits-general"></a>

The following limits affect general use of Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| API request payload size | 7 MB per request | No | 
| Apps | 100 per account | No | 

## Campaign Limits<a name="limits-campaign"></a>

The following limits apply to the [Campaigns](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html) resource of the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Active campaigns | 200 per account An *active campaign* is a campaign that hasn't completed or failed\. Active campaigns have a status of `SCHEDULED`, `EXECUTING`, or `PENDING_NEXT_RUN`\.  | [Yes](#limits-increase) | 
| Message sends | 100 million per campaign activity | [Yes](#limits-increase) | 
| Event\-based campaigns | Each project can include up to 10 campaigns that are sent when events occur\. Campaigns that use event\-based triggers have to use dynamic segments\. They can't use imported segments\.  If you integrate your app with Amazon Pinpoint by using an AWS Mobile SDK, messages from event\-based campaigns are sent only to customers whose apps are running AWS Mobile SDK for Android version 2\.7\.2 or later, or AWS Mobile SDK for iOS version 2\.6\.30 or later\. If Amazon Pinpoint can't deliver a message from an event\-based campaign within five minutes, it drops the message and doesn't attempt to redeliver it\. | No | 

## Email Limits<a name="limits-email"></a>

The limits in the following sections apply to the email channel\.

### Email Message Limits<a name="limits-email-message"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum message size, including attachments |  10 MB per message  | No | 
| Number of verified identities |  10,000 identities  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   | No | 

### Email Sender and Recipient Limits<a name="limits-email-message"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Sender address | All sending addresses or domains must be verified\. | No | 
| Recipient address | If your account is still in the sandbox, all recipient email addresses or domains must be verified\. If your account is out of the sandbox, you can send to any valid address\. | [Yes](#limits-increase) | 
| Number of recipients per message |  50 recipients per message  | No | 
| Number of identities that you can verify |  10,000 identities per AWS Region  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   | No | 

### Email Sending Limits<a name="limits-email-sending"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Number of emails that can be sent per 24\-hour period \(sending quota\) |  If your account is in the sandbox, 200 emails per 24\-hour period\. If your account is out of the sandbox, the quota varies based on your specific use case\.  This quota is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#limits-increase)  | 
| Number of emails that can be sent each second \(sending rate\) |  If your account is in the sandbox, 1 email per second\. If your account is out of the sandbox, the rate varies based on your specific use case\.   This rate is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#limits-increase)  | 

## Endpoint Limits<a name="limits-endpoint"></a>

The following limits apply to the [Endpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html) resource of the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Attributes assigned to the `Attributes`, `Metrics`, and `UserAttributes` parameters collectively | 40 per project | No | 
| Attributes assigned to the `Attributes` parameter | 40 per project | No | 
| Attributes assigned to the `Metrics` parameter | 40 per project | No | 
| Attributes assigned to the `UserAttributes` parameter | 40 per project | No | 
| Attribute name length | 50 characters | No | 
| Attribute value length | 100 characters | No | 
| `EndpointBatchItem` objects in an `EndpointBatchRequest` payload | 100 per payload\. The payload size can't exceed 7 MB\. | No | 
| Endpoints with the same user ID | 10 unique endpoints per user ID | No | 
| Values assigned to `Attributes` parameter attributes | 50 per attribute | No | 
| Values assigned to `UserAttributes` parameter attributes | 50 per attribute | No | 

## Endpoint Import Limits<a name="limits-import"></a>

The following limits apply to importing endpoints into Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Concurrent import jobs | 2 per account | [Yes](#limits-increase) | 
| Import size | 1 GB per import jobFor example, if each endpoint is 4 KB or less, you can import 250,000 endpoints\. | [Yes](#limits-increase) | 

## Event Ingestion Limits<a name="limits-events"></a>

The following limits apply to the ingestion of events using the AWS Mobile SDKs and the [Amazon Pinpoint Events API](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-events.html)\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of custom event types | 1,500 per app | No | 
| Maximum number of custom attribute keys | 500 per app | No | 
| Maximum number of custom attribute values per attribute key | 100,000 | No | 
| Maximum number of characters per attribute key | 50 | No | 
| Maximum number of characters per attribute value | 200 | No | 
| Maximum number of custom metric keys | 500 per app | No | 
| Maximum number of events in a request | 100 per request | No | 
| Maximum size of a request | 4 MB | No | 
| Maximum size of an individual event | 1,000 KB | No | 
| Maximum number of attribute keys and metric keys for each event | 40 per request | No | 

## Journey Limits<a name="limits-journeys"></a>

The following limits apply to journeys\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of active journeys | 50 per account | [Yes](#limits-increase) | 
|  Maximum number of journey activities  | 40 per journey | [Yes](#limits-increase) | 
| Maximum segment size | For imported segments: 5 GB per journeyFor dynamic segments: unlimited | No | 

## Message Template Limits<a name="limits-message-templates"></a>

The following limits apply to message templates\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of message templates | 10,000 per account | [Yes](#limits-increase) | 
| Maximum number of versions of a message template | 5,000 per template | No | 
| Maximum number of characters in an email template | 500,000 characters | No | 
|  Maximum number of characters in the default template parts of a push notification template   | 2,000 characters | No | 
| Maximum number of characters in ADM\-specific template parts of a push notification template | 4,000 characters | No | 
| Maximum number of characters in APNs\-specific template parts of a push notification template | 2,000 characters | No | 
| Maximum number of characters in Baidu\-specific template parts of a push notification template | 4,000 characters | No | 
| Maximum number of characters in FCM\-specific template parts of a push notification template | 4,000 characters | No | 
|  Maximum number of characters in an SMS template  | 1,600 characters | No | 
| Maximum number of characters in a voice template | 10,000 characters | No | 

## Mobile Push Limits<a name="limits-mobile"></a>

The following limits apply to messages that Amazon Pinpoint delivers through mobile push notification channels\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of mobile push notifications that can be sent per second | 25,000 notifications per second | No | 
| Amazon Device Messaging \(ADM\) message payload size | 6 KB per message | No | 
| Apple Push Notification service \(APNs\) message payload size | 4 KB per message | No | 
| APNs sandbox message payload size | 4 KB per message | No | 
| Baidu Cloud Push message payload size | 4 KB per message | No | 
| Firebase Cloud Messaging \(FCM\) message payload size | 4 KB per message | No | 

## Segment Limits<a name="limits-segment"></a>

The following limits apply to the [Segments](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html) resource of the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of dimensions that can be used to create a segment | 100 per segment | No | 

## SMS Limits<a name="limits-sms"></a>

The following limits apply to the SMS channel\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Account spend threshold | USD $1\.00 per account | [Yes](#limits-increase) | 
| Number of SMS messages that can be sent each second \(*sending rate* \) | 20 messages per second | No | 
| Number of SMS messages that can be sent to a single recipient each second | 1 message per second | No | 
| Number of Amazon SNS topics for two\-way SMS | 100,000 per account | [Yes](#limits-increase) | 

## Voice Limits<a name="limits-voice"></a>

The following limits apply to the voice channel\.


| Resource | Default Limit | 
| --- | --- | 
| Number of voice messages that can be sent in a 24\-hour period | If your account is in the sandbox: 20 messagesIf your account is out of the sandbox: unlimited | 
| Number of voice messages that can be sent to a single recipient in a 24\-hour period | 5 messages | 
| Number of voice messages that can be sent per minute | If your account is in the sandbox: 5 calls per minuteIf your account is out of the sandbox: 20 calls per minute | 
| Number of voice messages that can be sent from a single originating phone number per second | 1 message per second | 
| Voice message length | If your account is in the sandbox: 30 secondsIf your account is out of the sandbox: 5 minutes | 
| Ability to send voice messages to international phone numbers | If your account is in the sandbox, you can send messages to recipients in only the following countries: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/limits.html) If your account is out of the sandbox, you can send messages to recipients in any country\.  International calls are subject to additional fees, which vary by destination country or region\.   | 
| Number of characters in a voice message |  3,000 billable characters \(characters in words that are spoken\) 6,000 characters total \(including billable characters and SSML tags\) | 
| Number of configuration sets | 10,000 voice configuration sets per AWS Region | 

## Requesting a Limit Increase<a name="limits-increase"></a>

If the value in the **Eligible for Increase** column in any of the preceding tables is **Yes**, you can request a change to that limit\.

**To request a limit increase**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. Create a new AWS Support case at [https://console\.aws\.amazon\.com/support/home\#/case/create](https://console.aws.amazon.com/support/home#/case/create)\.

1. On the **Create case** page, choose **Service limit increase**\.

1. For **Limit type**, choose one of the following options:
   + To request a limit increase that's related to the email channel, choose **Pinpoint Email**\.
   + To request a limit increase that's related to the SMS channel, choose **Pinpoint SMS**\.
   + To request a limit increase that's related to any other Amazon Pinpoint feature, choose **Pinpoint**\.

1. Under **Requests**, for **Region**, choose the AWS Region that you want to request the limit increase for\. To request a limit increase for the same limit type in an additional Region, choose **Add another request**, and then choose the additional Region\.

1. Under **Case description**, for **Use case description**, explain why you're requesting the limit increase\.

1. Under **Contact options**, for **Preferred contact language**, choose the language that you prefer to use when communicating with the AWS Support team\.

1. For **Contact method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we’re able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn’t align with our policies\.
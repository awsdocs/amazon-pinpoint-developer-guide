# Limits in Amazon Pinpoint<a name="limits"></a>

The following sections describe limits within Amazon Pinpoint\.

**Topics**
+ [General Limits](#limits-general)
+ [Endpoint Limits](#limits-endpoint)
+ [Endpoint Import Limits](#limits-import)
+ [Segment Limits](#limits-segment)
+ [Campaign Limits](#limits-campaign)
+ [Mobile Push Limits](#limits-mobile)
+ [Email Limits](#limits-email)
+ [SMS Limits](#limits-sms)
+ [Voice Limits](#limits-voice)
+ [Event Ingestion Limits](#limits-events)
+ [Requesting a Limit Increase](#limits-increase)

## General Limits<a name="limits-general"></a>

The following limits affect general use of Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| API request payload size | 7 MB per request | No | 
| Apps | 100 per account | No | 

## Endpoint Limits<a name="limits-endpoint"></a>

The following limits apply to the [Endpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html) resource in the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Attributes assigned to the `Attributes`, `Metrics`, and `UserAttributes` parameters collectively | 40 per app | No | 
| Attributes assigned to the `Attributes` parameter | 40 per app | No | 
| Attributes assigned to the `Metrics` parameter | 40 per app | No | 
| Attributes assigned to the `UserAttributes` parameter | 40 per app | No | 
| Attribute name length | 50 characters | No | 
| Attribute value length | 100 characters | No | 
| `EndpointBatchItem` objects in an `EndpointBatchRequest` payload | 100 per payload\. The payload size can't exceed 7 MB\. | No | 
| Endpoints with the same user ID | 10 unique endpoints per user ID | No | 
| Values assigned to `Attributes` parameter attributes | 50 per attribute | No | 
| Values assigned to `UserAttributes` parameter attributes | 50 per attribute | No | 

## Endpoint Import Limits<a name="limits-import"></a>

The following limits apply when you import endpoints into Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Concurrent import jobs | 2 per account | [Yes](#limits-increase) | 
| Import size | 1 GB per import job\(For example, if each endpoint is 4 KB or less, you can import 250,000 endpoints\.\) | [Yes](#limits-increase) | 

## Segment Limits<a name="limits-segment"></a>

The following limits apply to the [Segments](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html) resource in the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of dimensions that can be used to create a segment | 100 per segment | No | 

## Campaign Limits<a name="limits-campaign"></a>

The following limits apply to the [Campaigns](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html) resource in the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Active campaigns | 200 per account An *active campaign* is a campaign that hasn't completed or failed\. Active campaigns have a status of `SCHEDULED`, `EXECUTING`, or `PENDING_NEXT_RUN`\.  | [Yes](#limits-increase) | 
| Message sends | 100 million per campaign activity | [Yes](#limits-increase) | 
| Event\-based campaigns | Each project can include up to 10 campaigns that are sent when events occur\. Campaigns that use event\-based triggers have to use dynamic segments \(that is, they can't use imported segments\)\.  Event\-based campaigns are only sent to customers who use apps that run version 2\.7\.2 or later of the AWS Mobile SDK for Android or version 2\.6\.30 or later of the AWS Mobile SDK for iOS\. If Amazon Pinpoint can't deliver a message from an event\-based campaign within five minutes, it drops the message and doesn't attempt to re\-deliver it\. | No | 

## Mobile Push Limits<a name="limits-mobile"></a>

The following limits apply to messages that Amazon Pinpoint delivers through mobile push channels\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of mobile push notifications that can be sent per second | 25,000 notifications per second | No | 
| Amazon Device Messaging \(ADM\) message payload size | 6 KB per message | No | 
| Apple Push Notification service \(APNs\) message payload size | 4 KB per message | No | 
| APNs sandbox message payload size | 4 KB per message | No | 
| Baidu Cloud Push message payload size | 4 KB per message | No | 
| Firebase Cloud Messaging \(FCM\) message payload size | 4 KB per message | No | 

## Email Limits<a name="limits-email"></a>

The limits in the following sections apply to the Email channel\.

### Email Sending Limits<a name="limits-email-sending"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Number of emails that can be sent per 24\-hour period \(sending quota\) |  If your account is in the sandbox: 200 emails per 24\-hour period\. If your account is out of the sandbox, the quota varies based on your specific use case\.  This quota is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#limits-increase)  | 
| Number of emails that can be sent each second \(sending rate\) |  If your account is in the sandbox: 1 email per second\. If your account is out of the sandbox, the rate varies based on your specific use case\.   This rate is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#limits-increase)  | 

### Email Message Limits<a name="limits-email-message"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum message size \(including attachments\) |  10 MB per message\.  | No | 
| Number of verified identities |  10,000 identities\.  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   | No | 

### Email Sender and Recipient Limits<a name="limits-email-message"></a>


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Sender address | All sending addresses or domains must be verified\. | No | 
| Recipient address | If your account is still in the sandbox, all recipient email addresses or domains must be verified\. If your account is out of the sandbox, you can send to any valid address\. | [Yes](#limits-increase) | 
| Number of recipients per message |  50 recipients per message\.  | No | 
| Number of identities that you can verify |  10,000 identities per AWS Region\.  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   | No | 

## SMS Limits<a name="limits-sms"></a>

The following limits apply to the SMS channel\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Account spend threshold | USD$1\.00 per account\. | [Yes](#limits-increase) | 
| Number of SMS messages that can be sent each second \(*sending rate* \) | 20 messages per second\. | No | 
| Number of Amazon SNS topics for two\-way SMS | 100,000 per account\. | [Yes](#limits-increase) | 

## Voice Limits<a name="limits-voice"></a>

The following limits apply to the Voice channel\.


| Resource | Default Limit | 
| --- | --- | 
| Number of voice messages that can be sent in a 24\-hour period | If your account is in the sandbox: 20 messages\.If your account is out of the sandbox: unlimited\. | 
| Number of voice messages that can be sent to a single recipient in a 24\-hour period | 5 messages\. | 
| Number of voice messages that can be sent per minute | If your account is in the sandbox: 5 calls per minute\.If your account is out of the sandbox: 20 calls per minute\. | 
| Number of voice messages that can be sent from a single originating phone number per second | 1 message per second\. | 
| Voice message length | If your account is in the sandbox: 30 seconds\.If your account is out of the sandbox: 5 minutes\. | 
| Ability to send voice messages to international phone numbers | If your account is in the sandbox, you can only send messages to recipients in the following countries: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/limits.html) If your account is out of the sandbox, you can send messages to recipients in any country\.  International calls are subject to additional fees, which vary by destination country or region\.   | 
| Number of characters in a voice message |  3,000 billable characters \(characters in words that are spoken\) 6,000 characters total \(including billable characters and SSML tags\) | 
| Number of configuration sets | 10,000 voice configuration sets per AWS Region\. | 

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
| Maximum number events in a request | 100 per request | No | 
| Maximum size of a request | 4 MB | No | 
| Maximum size of an individual event | 1,000 KB | No | 
| Maximum number of attribute keys and metric keys for each event | 40 per request | No | 

## Requesting a Limit Increase<a name="limits-increase"></a>

If the value in the **Eligible for Increase** column in any of the tables above is **Yes**, you can request a change to that limit\.

**To request a limit increase**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. Create a new Support case at [https://console\.aws\.amazon\.com/support/home\#/case/create](https://console.aws.amazon.com/support/home#/case/create)\.

1. On the **Create Case** page, make the following selections:
   + For **Regarding**, choose **Service Limit Increase**\.
   + For **Limit Type**, choose one of the following options:
     + Choose **Amazon Pinpoint** for limit increases related to Amazon Pinpoint campaigns and imports\.
     + Choose **Amazon Pinpoint Email** for limit increases related to the email channel\.
     + Choose **Amazon Pinpoint SMS** for limit increases related to the SMS channel\.

1. For **Use Case Description**, explain why you are requesting the limit increase\.

1. For **Support Language**, choose the language you prefer to use when communicating with the AWS Support team\.

1. For **Contact Method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we’re able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn’t align with our policies\.
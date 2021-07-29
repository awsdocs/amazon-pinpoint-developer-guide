# Amazon Pinpoint quotas<a name="quotas"></a>

The following sections list and describe the quotas, formerly referred to as *limits*, that apply to Amazon Pinpoint resources and operations\. Some quotas can be increased, while others cannot\. To determine whether you can request an increase for a quota, refer to the **Eligible for Increase** column or statement in each section\.

**Topics**
+ [General quotas](#quotas-general)
+ [API request quotas](#quotas-pinpoint-api)
+ [Campaign quotas](#quotas-campaign)
+ [Email quotas](#quotas-email)
+ [Endpoint quotas](#quotas-endpoint)
+ [Endpoint import quotas](#quotas-import)
+ [Event ingestion quotas](#quotas-events)
+ [Journey quotas](#quotas-journeys)
+ [Machine learning quotas](#quotas-ML-models)
+ [Message template quotas](#quotas-message-templates)
+ [Push notification quotas](#quotas-mobile)
+ [Segment quotas](#quotas-segment)
+ [SMS quotas](#quotas-sms)
+ [10DLC quotas](#quotas-sms)
+ [Voice quotas](#quotas-voice)
+ [Requesting a quota increase](#quotas-increase)

## General quotas<a name="quotas-general"></a>

Your account can have up to 100 Amazon Pinpoint projects\. This quota is not eligible for increase\.

## API request quotas<a name="quotas-pinpoint-api"></a>

Amazon Pinpoint implements quotas that restrict the size and number of requests that you can make to the Amazon Pinpoint API from your AWS account\. These quotas are not eligible for increase\.

The maximum size of an invocation \(request and response\) payload is 7 MB, unless otherwise specified for a particular type of resource\. To determine whether a resource has a different quota, see the appropriate section of this topic for that type of resource\.

The maximum number of requests varies by quota type and API operation\. Amazon Pinpoint implements two types of quotas for API requests:
+ **Rate quotas** – Also referred to as *rate limits*, this type of quota defines the maximum number of requests that you can make per second for a particular operation\. It controls the rate of requests that are sent or received\.
+ **Burst quotas** – Also referred to as *burst limits* or *burst capacity*, this type of quota defines the maximum number of requests that you can send at one time for a particular operation\. It prevents usage spikes by ensuring that the supported number of requests is distributed evenly across a certain time period\.

The following table lists the rate and burst quotas for the Amazon Pinpoint API\.


| Operation | Default rate quota \(requests per second\) | Default burst quota \(number of requests\) | 
| --- | --- | --- | 
|  CreateCampaign  |  25  |  25  | 
|  CreateSegment  |  25  |  25  | 
| DeleteCampaign | 25 | 25 | 
| DeleteEndpoint | 1,000 | 1,000 | 
| DeleteSegment | 25 | 25 | 
| GetEndpoint | 7,000 | 7,000 | 
| PhoneNumberValidate | 20 | 20 | 
| PutEvents | 7,000 | 7,000 | 
| SendMessages | 4,000 | 4,000 | 
| SendUsersMessages | 6,000 | 6,000 | 
| UpdateCampaign | 25 | 25 | 
| UpdateEndpoint | 5,000 | 5,000 | 
| UpdateEndpointsBatch | 5,000 | 5,000 | 
| UpdateSegment | 25 | 25 | 
| All other operations | 300 | 300 | 

If you exceed one of these quotas, Amazon Pinpoint throttles the request—that is, it rejects an otherwise valid request and returns a `TooManyRequests` error\. Throttling is based on the total number of requests that you make from your account for a specific operation in a specific AWS Region\. In addition, throttling decisions are calculated independently for each operation\. For example, if Amazon Pinpoint throttles a request for the `SendMessages` operation, a concurrent request for the `UpdateEndpoint` operation can complete successfully\.

## Campaign quotas<a name="quotas-campaign"></a>

The following quotas apply to the [Campaigns](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html) resource of the Amazon Pinpoint API\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Active campaigns  |  200 per account  An *active campaign* is a campaign that hasn't completed or failed\. Active campaigns have a status of `SCHEDULED`, `EXECUTING`, or `PENDING_NEXT_RUN`\.   |  [Yes](#quotas-increase)  | 
| Maximum segment size | For imported segments: 100,000,000 per campaignFor dynamic segments: unlimited | No | 
| Event\-based campaigns |  Each project can include up to 25 campaigns that are sent when events occur\. Campaigns that use event\-based triggers have to use dynamic segments\. They can't use imported segments\.  If you integrate your app with Amazon Pinpoint by using an AWS Mobile SDK, messages from event\-based campaigns are sent only to customers whose apps are running AWS Mobile SDK for Android version 2\.7\.2 or later, or AWS Mobile SDK for iOS version 2\.6\.30 or later\. If Amazon Pinpoint can't deliver a message from an event\-based campaign within five minutes, it drops the message and doesn't attempt to redeliver it\.  | [Yes](#quotas-increase) | 

## Email quotas<a name="quotas-email"></a>

The quotas in the following sections apply to the email channel\. 

### Email message quotas<a name="quotas-email-message"></a>


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
| Maximum message size, including attachments |  10 MB per message  |  No  | 
| Number of verified identities |  10,000 identities  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   |  No  | 

### Email sender and recipient quotas<a name="quotas-email-message"></a>


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Sender address  |  All sending addresses or domains must be verified\.  |  No  | 
| Recipient address | If your account is in the sandbox, all recipient email addresses or domains must be verified\. If your account is out of the sandbox, you can send to any valid address\. | [Yes](#quotas-increase) | 
| Number of recipients per message |  50 recipients per message  |  No  | 
| Number of identities that you can verify |  10,000 identities per AWS Region  *Identities* refers to email addresses or domains, or any combination of the two\. Every email you send using Amazon Pinpoint must be sent from a verified identity\.   |  No  | 

### Email sending quotas<a name="quotas-email-sending"></a>

The sending quota, sending rate, and sandbox limits are shared between the two services in the same Region\. If you use Amazon SES in us\-east\-1, and you’ve been removed from the sandbox and had your sending quota/rate increased, then those changes all apply to your Pinpoint account in us\-east\-1\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
| Number of emails that can be sent per 24\-hour period \(sending quota\) | If your account is in the sandbox, 200 emails per 24\-hour period\. If your account is out of the sandbox, the quota varies based on your specific use case\.  This quota is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#quotas-increase)  | 
| Number of emails that can be sent each second \(sending rate\) | If your account is in the sandbox, 1 email per second\. If your account is out of the sandbox, the rate varies based on your specific use case\.   This rate is based on the number of recipients, as opposed to the number of unique messages sent\. A *recipient* is any email address on the To: line\.   |  [Yes](#quotas-increase)  | 

## Endpoint quotas<a name="quotas-endpoint"></a>

The following quotas apply to the [Endpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html) resource of the Amazon Pinpoint API\.

The maximum number of attributes supported per endpoint is 250, and the maximum endpoint size is 15 KB\. This number of attributes might be limited, however, by the total size of an endpoint, which includes all attributes\. If you run into any errors when adding attributes to your template, consider decreasing the amount of data in each attribute or decreasing the number of attributes\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Attributes assigned to the `Attributes`, `Metrics`, and `UserAttributes` parameters collectively  |  250 for all attribute parameters per application and a maximum size of 15 KB for the endpoint\.   |  Yes  | 
|  Attributes assigned to the `Attributes` parameter  | 250 for all attribute parameters per application and a maximum size of 15 KB for the endpoint\.  |  Yes  | 
|  Attributes assigned to the `Metrics` parameter  |  250 for all attribute parameters per application and a maximum size of 15 KB for the endpoint\.   |  Yes  | 
|  Attributes assigned to the `UserAttributes` parameter  |  250 for all attribute parameters per application and a maximum size of 15 KB for the endpoint\.   |  Yes  | 
|  Attribute name length  |  50 characters  |  No  | 
|  Attribute value length  |  100 characters  |  No  | 
|  `EndpointBatchItem` objects in an `EndpointBatchRequest` payload  |  100 per payload\. The payload size can't exceed 7 MB\.  |  No  | 
|  Endpoints with the same user ID  |  15 unique endpoints per user ID  |  No  | 
|  Values assigned to `Attributes` parameter attributes  |  50 per attribute  |  No  | 
|  Values assigned to `UserAttributes` parameter attributes  |  50 per attribute  |  No  | 

## Endpoint import quotas<a name="quotas-import"></a>

The following quotas apply to importing endpoints into Amazon Pinpoint\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Concurrent import jobs  |  10 per account  |  [Yes](#quotas-increase)  | 
| Import size | 1 GB per import jobFor example, if each endpoint is 4 KB or less, you can import 250,000 endpoints\. |  [Yes](#quotas-increase)  | 

## Event ingestion quotas<a name="quotas-events"></a>

The following quotas apply to the ingestion of events using the AWS Mobile SDKs and the [Events](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-events.html) resource of the Amazon Pinpoint API\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Maximum number of custom event types  |  1,500 per app  |  No  | 
|  Maximum number of custom attribute keys  |  500 per app  |  No  | 
|  Maximum number of custom attribute values per attribute key  |  100,000\. Any number that exceeds 100,000 is still registered, but won't be available in the Amazon Pinpoint analytics console\.  |  No  | 
|  Maximum number of characters per attribute key  |  50  |  No  | 
|  Maximum number of characters per attribute value  |  200\. If the number of characters exceeds 200 the event is dropped\.  |  No  | 
|  Maximum number of custom metric keys  |  500 per app  |  No  | 
|  Maximum number of events in a request  |  100 per request  |  No  | 
|  Maximum size of a request  |  4 MB  |  No  | 
|  Maximum size of an individual event  |  1,000 KB  |  No  | 
|  Maximum number of attribute keys and metric keys for each event  |  40 per request  |  No  | 

## Journey quotas<a name="quotas-journeys"></a>

The following quotas apply to journeys\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Maximum number of active journeys  |  50 per account  |  [Yes](#quotas-increase)  | 
| Maximum number of active EventTriggeredJourneys | 20 per account | [Yes](#quotas-increase) | 
|  Maximum number of journey activities  |  40 per journey  |  [Yes](#quotas-increase)  | 
| Maximum segment size | For imported segments: 100,000,000 per journey\.For dynamic segments: unlimited | No | 

## Machine learning quotas<a name="quotas-ML-models"></a>

The following quotas apply to Amazon Pinpoint configurations for retrieving and processing data from machine learning \(ML\) models\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
| Maximum number of model configurations | 1 per message template100 per account | No | 
| Maximum number of recommendations | 5 per endpoint or user | No | 
| Maximum number of recommended attributes per endpoint or user | 1, if the attribute values aren't processed by an AWS Lambda function10, if the attribute values are processed by an AWS Lambda function |  No  | 
| Maximum length of a recommended attribute name | 50 characters for an attribute name25 characters for an attribute display name \(the name that appears in the **Attribute finder** on the console\) | No | 
| Maximum length of a recommended attribute value that's retrieved from Amazon Personalize | 100 characters | No | 
| Maximum size of an invocation payload \(request and response\) for a Lambda function | 6 MB | No | 
| Maximum amount of time to wait for a Lambda function to process data | 15 seconds | No | 
| Maximum number of attempts to invoke a Lambda function | 3 attempts | No | 

Depending on how you configure Amazon Pinpoint to use an ML model, additional quotas may apply\. To learn about Amazon Personalize quotas, see [Quotas](https://docs.aws.amazon.com/personalize/latest/dg/limits.html) in the *Amazon Personalize Developer Guide*\. To learn about AWS Lambda quotas, see [Quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html) in the *AWS Lambda Developer Guide*\.

## Message template quotas<a name="quotas-message-templates"></a>

The following quotas apply to message templates for your Amazon Pinpoint account\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Maximum number of message templates  |  10,000 per account  |  [Yes](#quotas-increase)  | 
|  Maximum number of versions  |  5,000 per template  |  No  | 
|  Maximum number of characters in an email template  | 500,000 characters |  No  | 
|  Maximum number of characters in the default template parts of a push notification template   | 2,000 characters | No | 
| Maximum number of characters in ADM\-specific template parts of a push notification template | 4,000 characters | No | 
| Maximum number of characters in APNs\-specific template parts of a push notification template | 2,000 characters | No | 
| Maximum number of characters in Baidu\-specific template parts of a push notification template | 4,000 characters | No | 
| Maximum number of characters in FCM\-specific template parts of a push notification template | 4,000 characters | No | 
|  Maximum number of characters in an SMS template  | 1,600 characters | No | 
| Maximum number of characters in a voice template | 10,000 characters | No | 

## Push notification quotas<a name="quotas-mobile"></a>

The following quotas apply to messages that Amazon Pinpoint sends through push notification channels\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
| Maximum number of push notifications that can be sent per second in a campaign | 25,000 notifications per second | [Yes](#quotas-increase) | 
|  Amazon Device Messaging \(ADM\) message payload size  |  6 KB per message  |  No  | 
|  Apple Push Notification service \(APNs\) message payload size  |  4 KB per message  |  No  | 
|  APNs sandbox message payload size  |  4 KB per message  |  No  | 
|  Baidu Cloud Push message payload size  |  4 KB per message  |  No  | 
|  Firebase Cloud Messaging \(FCM\) message payload size  |  4 KB per message  |  No  | 

## Segment quotas<a name="quotas-segment"></a>

The following quotas apply to the [Segments](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html) resource of the Amazon Pinpoint API\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Maximum number of dimensions that can be used to create a segment  |  100 per segment  |  No  | 

## SMS quotas<a name="quotas-sms"></a>

The following quotas apply to the SMS channel\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Spending threshold  |  USD $1\.00 per account  |  [Yes](#quotas-increase), but spending limits vary by region\. You must specify the region\(s\) in which you require an increase\.   | 
|  Number of SMS messages that can be sent each second \(*sending rate*\)  |  20 messages per second  You can request a quota increase if your use case supports it\.   |  [Yes](#quotas-increase) if your use case supports an increase; otherwise, No\.  | 
| Number of SMS messages that can be sent to a single recipient each second | 1 message per second | No | 
| Number of Amazon SNS topics for two\-way SMS | 100,000 per account | [Yes](#quotas-increase) | 

## 10DLC quotas<a name="quotas-sms"></a>

The following quotas apply to 10DLC\. 10DLC only applies to the U\.S\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
|  Max 10DLC companies per AWS account  | 25 |  [Yes](#quotas-increase)  | 
|  Max 10DLC campaigns per 10DLC company  | 10 |  [Yes](#quotas-increase)  | 
| Max 10DLC numbers per 10DLC campaign | 49 | [Yes](#quotas-increase), but may require carrier approval\. | 

## Voice quotas<a name="quotas-voice"></a>

The following quotas apply to the voice channel\.

**Note**  
When your account is removed from the sandbox, you automatically qualify for the maximum quotas shown in the following table\.


| Resource | Default quota | Eligible for increase | 
| --- | --- | --- | 
| Number of voice messages that can be sent during a 24\-hour period | If your account is in the sandbox: 20 messages | No | 
| Number of voice messages that can be sent to a single recipient during a 24\-hour period | 5 messages | No | 
| Number of voice messages that can be sent per minute | If your account is in the sandbox: 5 calls per minuteIf your account is out of the sandbox: 20 calls per minute | No | 
| Number of voice messages that can be sent from a single originating phone number per second | 1 message per second | No | 
| Voice message length | If your account is in the sandbox: 30 secondsIf your account is out of the sandbox: 5 minutes | No | 
| Ability to send voice messages to international phone numbers |  If your account is in the sandbox, you can send messages to recipients in only the following countries: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/quotas.html) If your account is out of the sandbox, you can send messages to recipients in any country\.  International calls are subject to additional fees, which vary by destination country or region\.   | No | 
| Number of characters in a voice message | 3,000 billable characters, in words that are spoken 6,000 characters total, including billable characters and SSML tags | No | 
| Number of configuration sets |  10,000 voice configuration sets per AWS Region  | No | 

## Requesting a quota increase<a name="quotas-increase"></a>

If the value in the **Eligible for Increase** column in any of the preceding tables is **Yes**, you can request an increase for that quota\.

**To request a quota increase**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. Create a new AWS Support case at [https://console\.aws\.amazon\.com/support/home\#/case/create](https://console.aws.amazon.com/support/home#/case/create)\.

1. On the **Create case** page, choose **Service quota increase**\.

1. Under **Case classification**, for **Quota type**, choose one of the following options:
   + To request a quota increase that's related to the email channel, choose **Pinpoint Email**\.
   + To request a quota increase that's related to the SMS channel, choose **Pinpoint SMS**\.
   + To request a quota increase that's related to the voice channel, choose **Pinpoint Voice**\.
   + To request an increase that's related to 10DLC, choose **Pinpoint**\.
   + To request a quota increase that's related to any other Amazon Pinpoint feature, choose **Pinpoint**\.

1. Depending on the option that you chose in the preceding step, enter the requested information in the optional and required fields to explain your use case\.

1. Under **Requests**, do one of the following:
   + If your request is related to the SMS channel, for **Resource Type**, choose **General Quotas**\.
   + If your request is related to another channel or any other Amazon Pinpoint feature, for **Region**, choose the AWS Region that you want to request the quota increase for\. To request an increase to the same quota in an additional Region, choose **Add another request**, and then choose the additional Region\.

1. Choose the quota that you want to increase, and then enter the new value that you want for the quota\.

1. Under **Case description**, for **Use case description**, explain why you're requesting the quota increase\.

1. Under **Contact options**, for **Preferred contact language**, choose the language that you prefer to use when communicating with the AWS Support team\.

1. For **Contact method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we’re able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn’t align with our policies\.
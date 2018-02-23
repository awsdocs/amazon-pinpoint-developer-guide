# Limits in Amazon Pinpoint<a name="limits"></a>

The following sections describe limits within Amazon Pinpoint\.


+ [General limits](#limits-general)
+ [Endpoint limits](#limits-endpoint)
+ [Endpoint import limits](#limits-import)
+ [Segment limits](#limits-segment)
+ [Campaign limits](#limits-campaign)
+ [Mobile push limits](#limits-mobile)
+ [Email limits](#limits-email)
+ [SMS limits](#limits-sms)
+ [Requesting a limit increase](#limits-increase)

## General limits<a name="limits-general"></a>

The following limits affect general use of Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| API request payload size | 7 MB per request | No | 
| Apps | 100 per account | No | 

## Endpoint limits<a name="limits-endpoint"></a>

The following limits apply to the [Endpoints](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html) resource in the Amazon Pinpoint API\.


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

## Endpoint import limits<a name="limits-import"></a>

The following limits apply when you import endpoints into Amazon Pinpoint\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Concurrent import jobs | 2 per account | [Yes](#limits-increase) | 
| Import size | 1 GB per import job\(For example, if each endpoint is 4 KB or less, you can import 250,000 endpoints\.\) | [Yes](#limits-increase) | 

## Segment limits<a name="limits-segment"></a>

The following limits apply to the [Segments](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html) resource in the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Maximum number of segments | 100 per app | No | 
| Maximum number of dimensions that can be used to create a segment | 100 per segment | No | 

## Campaign limits<a name="limits-campaign"></a>

The following limits apply to the [Campaigns](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html) resource in the Amazon Pinpoint API\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Active campaigns | 200 per account An *active campaign* is a campaign that hasn't completed or failed\. Active campaigns have a status of `SCHEDULED`, `EXECUTING`, or `PENDING_NEXT_RUN`\.  | [Yes](#limits-increase) | 
| Message sends | 100 million per campaign activity | [Yes](#limits-increase) | 

## Mobile push limits<a name="limits-mobile"></a>

The following limits apply to messages that Amazon Pinpoint delivers through mobile push channels\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Amazon Device Messaging \(ADM\) message payload size | 6 KB per message | No | 
| Apple Push Notification service \(APNs\) message payload size | 4 KB per message | No | 
| APNs sandbox message payload size | 4 KB per message | No | 
| Baidu Cloud Push message payload size | 4 KB per message | No | 
| Firebase Cloud Messaging \(FCM\) or Google Cloud Messaging \(GCM\) message payload size | 4 KB per message | No | 

## Email limits<a name="limits-email"></a>

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

## SMS limits<a name="limits-sms"></a>

The following limits apply to the SMS channel\.


| Resource | Default Limit | Eligible for Increase | 
| --- | --- | --- | 
| Account spend threshold | USD$1\.00 per account\. | [Yes](#limits-increase) | 
| Number of SMS messages that can be sent each second \(*sending rate* \) | 20 messages per second\. | No | 
| Number of Amazon SNS topics for two\-way SMS | 100,000 per account\. | [Yes](#limits-increase) | 

## Requesting a limit increase<a name="limits-increase"></a>

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
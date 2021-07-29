# Document history for Amazon Pinpoint<a name="doc-history"></a>

The following table describes important changes in each release of the *Amazon Pinpoint Developer Guide* after December 2018\. For notification about updates to this documentation, you can subscribe to an RSS feed\.
+ **Latest documentation update:** November 16, 2020

| Change | Description | Date | 
| --- |--- |--- |
| [UpdateEndpoint](#doc-history) | The Amazon Pinpoint [UpdateEndpoint API](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#UpdateEndpoint.html) is now logged by CloudTrail\. | November 16, 2020 | 
| [Custom attributes](#doc-history) | Amazon Pinpoint now supports 250 attributes in email messaging templates\. See [Quotas](https://docs.aws.amazon.com/pinpoint/latest/developerguide/quotas)\. | September 18, 2020 | 
| [Regional availability](#doc-history) | Amazon Pinpoint is now available in these Regions: Asia Pacific \(Tokyo\) Region, Europe \(London\) Region, and Canada \(Central\) Region\. Note that the Amazon Pinpoint SMS and Voice API is not available in these Regions\. | September 10, 2020 | 
| [Regional availability](#doc-history) | Amazon Pinpoint is now available in the Asia Pacific \(Tokyo\) Region\. Note that the Amazon Pinpoint SMS and Voice API does not support Voice in this Region\. | September 2, 2020 | 
| [Campaign events](#doc-history) | Added information about a new campaign event `delivery_type` parameter to [Campaign events](https://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-campaign.html)\. | August 2, 2020 | 
| [Regional availability](#doc-history) | Amazon Pinpoint is now available in the Asia Pacific \(Seoul\) Region\. Note that the Amazon Pinpoint API does not support Voice or SMS in this Region\. | July 31, 2020 | 
| [Regional availability](#doc-history) | Amazon Pinpoint is now available in the AWS GovCloud \(US\) Region\. | April 30, 2020 | 
| [Custom channels](#doc-history) | Updated information about [creating custom channels by using Lambda functions or webhooks](https://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-custom.html)\. | April 23, 2020 | 
| [Machine learning](#doc-history) | Added information about retrieving personalized recommendations from recommender models, and optionally [enhancing those recommendations](https://docs.aws.amazon.com/pinpoint/latest/developerguide/ml-models-rm-lambda.html) by using AWS Lambda functions\. | March 4, 2020 | 
| [Security](#doc-history) | Added a [Security chapter](https://docs.aws.amazon.com/pinpoint/latest/developerguide/security.html), which provides information about various security controls and features of Amazon Pinpoint\. | February 4, 2020 | 
| [Journeys](#doc-history) | Added information about using Amazon Pinpoint journeys to develop automated workflows that perform messaging activities for projects\. Also added information about [querying analytics data](https://docs.aws.amazon.com/pinpoint/latest/developerguide/analytics.html) for a subset of metrics that apply to journeys\. | October 31, 2019 | 
| [Analytics](#doc-history) | Added procedures that explain how to [query analytics data](https://docs.aws.amazon.com/pinpoint/latest/developerguide/analytics.html) for campaigns and transactional messages, and added information about [using query results](https://docs.aws.amazon.com/pinpoint/latest/developerguide/analytics-query-results.html)\. | October 17, 2019 | 
| [Analytics](#doc-history) | Added information about [querying analytics data](https://docs.aws.amazon.com/pinpoint/latest/developerguide/analytics.html) for a subset of metrics that apply to transactional email and SMS messages\. | September 6, 2019 | 
| [Code examples](#doc-history) | Added [code examples](https://docs.aws.amazon.com/pinpoint/latest/developerguide/send-messages-push.html) that you can use to send transactional push notifications using all of the services that Amazon Pinpoint supports\. | July 30, 2019 | 
| [Analytics](#doc-history) | Added information about [querying analytics data](https://docs.aws.amazon.com/pinpoint/latest/developerguide/analytics.html) for a subset of metrics that apply to projects \(applications\) and campaigns\. | July 24, 2019 | 
| [Segments](#doc-history) | Added a [tutorial](https://docs.aws.amazon.com/pinpoint/latest/developerguide/tutorials-importing-data.html) that describes a solution for importing customer data into Amazon Pinpoint from external systems, such as Salesforce or Marketo\. | May 14, 2019 | 
| [Regional availability](#doc-history) | Amazon Pinpoint is now available in the AWS Asia Pacific \(Mumbai\) and Asia Pacific \(Sydney\) Regions\. | April 25, 2019 | 
| [Using postman with Amazon Pinpoint](#doc-history) | Added a [tutorial](https://docs.aws.amazon.com/pinpoint/latest/developerguide/tutorials-using-postman.html) that describes how to use Postman to interact with the Amazon Pinpoint API\. | April 8, 2019 | 
| [Tagging](#doc-history) | Added information about [tagging Amazon Pinpoint resources](https://docs.aws.amazon.com/pinpoint/latest/developerguide/tagging-resources.html)\. | February 27, 2019 | 
| [SMS registration](#doc-history) | Added a [Tutorials chapter](https://docs.aws.amazon.com/pinpoint/latest/developerguide/tutorials.html), and added a tutorial that describes how to create [a solution that handles SMS user registration](https://docs.aws.amazon.com/pinpoint/latest/developerguide/tutorials-two-way-sms.html)\. | February 27, 2019 | 
| [Code examples](#doc-history) | Added [code examples](https://docs.aws.amazon.com/pinpoint/latest/developerguide/send-messages.html) in several programming languages that show you how to send [email](https://docs.aws.amazon.com/pinpoint/latest/developerguide/send-messages-email.html), [SMS](https://docs.aws.amazon.com/pinpoint/latest/developerguide/send-messages-sms.html), and [voice](https://docs.aws.amazon.com/pinpoint/latest/developerguide/send-messages-voice.html) messages programmatically\. | February 6, 2019 | 

## Earlier updates<a name="doc-releases-archive"></a>

The following table describes important changes in each release of the *Amazon Pinpoint Developer Guide* through December 2018\.


| Change | Description | Date | 
| --- | --- | --- | 
| Regional availability | Amazon Pinpoint is now available in the AWS US West \(Oregon\) and Europe \(Frankfurt\) Regions\. | December 21, 2018 | 
| Voice channel | You can use the new Amazon Pinpoint voice channel to create voice messages and deliver them to your customers over the phone\. Currently, you can only send voice messages by using the Amazon Pinpoint SMS and Voice API\. | November 15, 2018 | 
| Europe \(Ireland\) Availability | Amazon Pinpoint is now available in the AWS Europe \(Ireland\) Region\. | October 25, 2018 | 
| Events API | Use the Amazon Pinpoint API to [record events](integrate-events.md#integrate-events-api) and associate them with endpoints\. | August 7, 2018 | 
| Code examples for defining and looking up endpoints | Code examples are added that show you how to define, update, delete, and look up endpoints\. Examples are provided for the AWS CLI, AWS SDK for Java, and the Amazon Pinpoint API\. For more information, see [Defining your audience to Amazon Pinpoint](audience-define.md) and [Accessing audience data in Amazon Pinpoint](audience-data.md)\. | August 7, 2018 | 
| Endpoint export permissions | [Configure an IAM policy](permissions-export-endpoints.md) that allows you to export Amazon Pinpoint endpoints to an Amazon S3 bucket\. | May 1, 2018 | 
| Phone number verification for SMS | Use the Amazon Pinpoint API to [verify a phone number](validate-phone-numbers.md) to determine whether it is a valid destination for SMS messages\. | April 23, 2018 | 
| Updated topics for Amazon Pinpoint integration | [Integrate Amazon Pinpoint](integrate.md) with your Android, iOS, or JavaScript application by using AWS SDKs or libraries\. | March 23, 2018 | 
| AWS CloudTrail logging | Added information about [logging Amazon Pinpoint API calls with CloudTrail](logging-using-cloudtrail.md)\. | February 6, 2018 | 
| Updated service quotas | Updated [Amazon Pinpoint quotas](quotas.md) with additional information about email quotas\. | January 19, 2018 | 
| Public beta for Amazon Pinpoint extensions | Use AWS Lambda functions to [customize segments](segments-dynamic.md) or [create custom messaging channels](channels-custom.md)\. | November 28, 2017 | 
| External ID removed from IAM trust policies | The external ID key is removed from the example [trust policy](permissions-import-segment.md#permissions-import-segment-trustpolicy) and example [Java code](segments-importing.md) for importing segments\. | October 26, 2017 | 
| Push notification payload quotas | The quotas include [payload sizes for mobile push messages](quotas.md#quotas-mobile)\. | October 25, 2017 | 
| Updated service quotas | Added SMS and email channel information to [Amazon Pinpoint quotas](quotas.md)\. | October 9, 2017 | 
| ADM and Baidu mobile push | Update your app code to handle push notifications from the Baidu and ADM mobile push channels\. | September 27, 2017 | 
| User IDs and authentication events with Amazon Cognito user pools\. | If you use Amazon Cognito user pools to manage user sign\-in in your mobile apps, Amazon Cognito assigns user IDs to endpoints, and it reports authentication events to Amazon Pinpoint\. | September 26, 2017 | 
| User IDs | Assign user IDs to endpoints to monitor app usage from individual users\. Examples are provided for the [AWS Mobile SDKs](integrate-endpoints.md) and [SDK for Java](audience-define-user.md#audience-define-user-example)\. | August 31, 2017 | 
| Authentication events | Report authentication events to learn how frequently users authenticate with your app\. Examples are provided in [Reporting events in your application](integrate-events.md)\. | August 31, 2017 | 
| Updated sample events | The [example events](event-streams.md) include events that Amazon Pinpoint streams for email and SMS activity\. | June 08, 2017 | 
| Android session management | Manage sessions in Android apps by using a class provided by the AWS Mobile Hub sample app\. | April 20, 2017 | 
| Updated monetization event samples | The sample code is updated for reporting monetization events\.  \. | March 31, 2017 | 
| Event streams | You can configure Amazon Pinpoint to [send your app and campaign events to an Kinesis stream](event-streams.md)\. | March 24, 2017 | 
| Permissions | See [How Amazon Pinpoint works with IAM](security_iam_service-with-iam.md) for information about granting access to Amazon Pinpoint for AWS users in your account and users of your mobile app\. | January 12, 2017 | 
| Amazon Pinpoint general availability | This release introduces Amazon Pinpoint\. | December 1, 2016 | 
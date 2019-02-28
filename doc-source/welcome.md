# What Is Amazon Pinpoint?<a name="welcome"></a>

Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels\. You can use Amazon Pinpoint to send push notifications, emails, SMS text messages, or voice messages\.

The information in this developer guide is intended for application developers\. This guide contains information about using the features of Amazon Pinpoint programmatically\. It also contains information of particular interest to mobile app developers, such as procedures for [integrating analytics and messaging features with your application](integrate.md)\.

There are several other documents that are companions to this document\. The following documents provide reference information related to the Amazon Pinpoint APIs:
+ [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)
+ [Amazon Pinpoint Email API](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/)
+ [Amazon Pinpoint SMS and Voice API](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)

If you're new to Amazon Pinpoint, you might find it helpful to review the [Amazon Pinpoint User Guide](https://docs.aws.amazon.com/pinpoint/latest/userguide/) before proceeding with this document\.

## Amazon Pinpoint Features<a name="welcome-features"></a>

This section describes the major features of Amazon Pinpoint and the tasks that you can perform by using them\.

### Define Audience Segments<a name="welcome-segments"></a>

Reach the right audience for your messages by [defining audience segments](segments.md)\. A segment designates which users receive the messages that are sent from a campaign\. You can define dynamic segments based on data that's reported by your application, such as operating system or mobile device type\. You can also import static segments that you define outside Amazon Pinpoint\.

### Engage Your Audience with Messaging Campaigns<a name="welcome-campaigns"></a>

Engage your audience by [creating a messaging campaign](campaigns.md)\. A campaign sends tailored messages on a schedule that you define\. You can create campaigns that send mobile push, email, or SMS messages\.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics\.

### Send Transactional Messages<a name="welcome-transactional"></a>

Keep your customers informed by sending transactional mobile push and SMS messages—such as new account activation messages, order confirmations, and password reset notifications—to specific users\. You can send transactional messages by using the Amazon Pinpoint REST API\.

### Analyze User Behavior<a name="welcome-analyze"></a>

Gain insights about your audience and the effectiveness of your campaigns by using the analytics that Amazon Pinpoint provides\. You can view trends about your users' level of engagement, purchase activity, and demographics\. You can also monitor your message traffic with metrics for messages that are sent and opened\. Through the Amazon Pinpoint API, your application can report custom data, which Amazon Pinpoint makes available for analysis\.

To analyze or store the analytics data outside of Amazon Pinpoint, you can configure Amazon Pinpoint to [stream the data](analytics-streaming.md) to Amazon Kinesis\.

## Regional Availability<a name="welcome-regions"></a>

Amazon Pinpoint is available in several AWS Regions and it provides an endpoint for each of these Regions\. For a list of supported Regions and endpoints, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#pinpoint_region) in the *Amazon Web Services General Reference*\.
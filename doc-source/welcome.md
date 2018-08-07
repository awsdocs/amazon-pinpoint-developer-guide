# What Is Amazon Pinpoint?<a name="welcome"></a>

Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels\. You can send push notifications, emails, or text messages \(SMS\), depending on the purpose of your campaign\.

The information in this developer guide is intended for application developers\. This guide contains information about using the features of Amazon Pinpoint programmatically\. It also contains information of particular interest to mobile app developers, such as procedures for [integrating analytics and messaging features with your application](integrate.md)\.

The [Amazon Pinpoint API Reference](http://docs.aws.amazon.com/pinpoint/latest/apireference/) is a companion to this document\. The API Reference provides information about the resources and methods that are available in the Amazon Pinpoint API\.

If you're new to Amazon Pinpoint, you might find it helpful to review the [Amazon Pinpoint User Guide](http://docs.aws.amazon.com/pinpoint/latest/userguide/) before proceeding with this document\.

## Amazon Pinpoint Features<a name="welcome-features"></a>

This section describes the major features of Amazon Pinpoint\.

### Define Audience Segments<a name="welcome-segments"></a>

Reach the right audience for your messages by [defining audience segments](segments.md)\. A segment designates which users receive the messages that are sent from a campaign\. You can define dynamic segments based on data that's reported by your application, such as operating system or mobile device type\. You can also import static segments that you define outside of Amazon Pinpoint\.

### Engage Your Audience with Messaging Campaigns<a name="welcome-campaigns"></a>

Engage your audience by [creating a messaging campaign](campaigns.md)\. A campaign sends tailored messages on a schedule that you define\. You can create campaigns that send mobile push, email, or SMS messages\.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics\.

### Send Direct Messages<a name="welcome-transactional"></a>

Keep your customers informed by sending direct mobile push and SMS messages—such as new account activation messages, order confirmations, and password reset notifications—to specific users\. You can send direct messages from the Amazon Pinpoint console, or by using the Amazon Pinpoint REST API\.

### Analyze User Behavior<a name="welcome-analyze"></a>

Gain insights about your audience and the effectiveness of your campaigns by using the analytics that Amazon Pinpoint provides\. You can view trends about your users' level of engagement, purchase activity, and demographics\. You can monitor your message traffic with metrics for messages sent and opened\. Through the Amazon Pinpoint API, your application can report custom data, which Amazon Pinpoint makes available for analysis\.

To analyze or store the analytics data outside of Amazon Pinpoint, you can configure Amazon Pinpoint to [stream the data](analytics-streaming.md) to Amazon Kinesis\.
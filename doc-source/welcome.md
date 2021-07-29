# What is Amazon Pinpoint?<a name="welcome"></a>

Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels\. You can use Amazon Pinpoint to send push notifications, emails, SMS text messages, or voice messages\.

The information in this developer guide is intended for application developers\. This guide contains information about using the features of Amazon Pinpoint programmatically\. It also contains information of particular interest to mobile app developers, such as procedures for [integrating analytics and messaging features with your application](integrate.md)\.

There are several other documents that are companions to this document\. The following documents provide reference information related to the Amazon Pinpoint APIs:
+ [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)
+ [Amazon Pinpoint SMS and voice API](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)

If you're new to Amazon Pinpoint, you might find it helpful to review the [Amazon Pinpoint User Guide](https://docs.aws.amazon.com/pinpoint/latest/userguide/) before proceeding with this document\.

## Amazon Pinpoint features<a name="welcome-features"></a>

This section describes the major features of Amazon Pinpoint and the tasks that you can perform by using them\.

### Define audience segments<a name="welcome-segments"></a>

Reach the right audience for your messages by [defining audience segments](segments.md)\. A segment designates which users receive the messages that are sent from a campaign\. You can define dynamic segments based on data that's reported by your application, such as operating system or mobile device type\. You can also import static segments that you define by using another service or application\.

### Engage your audience with messaging campaigns<a name="welcome-campaigns"></a>

Engage your audience by [creating a messaging campaign](campaigns.md)\. A campaign sends tailored messages on a schedule that you define\. You can create campaigns that send mobile push, email, or SMS messages\.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics\.

### Send transactional messages<a name="welcome-transactional"></a>

Keep your customers informed by sending transactional mobile push and SMS messages—such as new account activation messages, order confirmations, and password reset notifications— directly to specific users\. You can send transactional messages by using the Amazon Pinpoint REST API\.

### Analyze user behavior<a name="welcome-analyze"></a>

Gain insights about your audience and the effectiveness of your campaigns by using the analytics that Amazon Pinpoint provides\. You can view trends about your users' level of engagement, purchase activity, demographics, and more\. You can also monitor your message traffic by viewing metrics such as the total number of messages that were sent or opened for a campaign or application\. Through the Amazon Pinpoint API, your application can report custom data, which Amazon Pinpoint makes available for analysis, and you can query analytics data for certain standard metrics\.

To analyze or store analytics data outside Amazon Pinpoint, you can configure Amazon Pinpoint to [stream the data](event-streams.md) to Amazon Kinesis\.

## Regional availability<a name="welcome-regions"></a>

Amazon Pinpoint is available in several AWS Regions in North America, Europe, Asia, and Oceania\. In each Region, AWS maintains multiple Availability Zones\. These Availability Zones are physically isolated from each other, but are united by private, low\-latency, high\-throughput, and highly redundant network connections\. These Availability Zones enable us to provide very high levels of availability and redundancy, while also minimizing latency\.

To learn more about AWS Regions, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *Amazon Web Services General Reference*\. For a list of all the Regions where Amazon Pinpoint is currently available, see [AWS service endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#pinpoint_region) in the *Amazon Web Services General Reference*\. To learn more about the number of Availability Zones that are available in each Region, see [AWS global infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.
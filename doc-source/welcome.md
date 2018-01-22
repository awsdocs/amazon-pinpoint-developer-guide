# What Is Amazon Pinpoint?<a name="welcome"></a>

Amazon Pinpoint is an AWS service that you can use to improve user engagement\. Use Amazon Pinpoint to create campaigns that reach audience segments with tailored messages\. 

Amazon Pinpoint supports multiple messaging channels\. You can choose to send push notifications, emails, or text messages \(SMS\) depending on the purpose of your campaign and the type of message\.

With Amazon Pinpoint, you can do the following:

## Define Audience Segments<a name="welcome-segments"></a>

To reach the right audience with your messages, define audience segments\. You can define dynamic segments based on data reported by your application, such as operating system or mobile device\.

You can also import segments that you define outside of Amazon Pinpoint\.

A segment designates which users receive the messages delivered by a campaign\.

## Engage Your Audience with Messaging Campaigns<a name="welcome-campaigns"></a>

Engage your audience segment by creating a messaging campaign\. A campaign sends tailored messages according to a schedule that you define\. You can create a campaign that sends messages through any channel that is supported by Amazon Pinpoint: mobile push, email, or SMS\.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics\.

## Analyze User Behavior<a name="welcome-analyze"></a>

Using the analytics provided by Amazon Pinpoint, you can gain insights about your audience and the effectiveness of your campaigns\. You can view trends about your users' level of engagement, purchase activity, and demographics\. You can monitor your message traffic with metrics for messages sent and opened\. Through the Amazon Pinpoint API, your application can report custom data, which Amazon Pinpoint makes available for analysis\.

To analyze or store the analytics data outside of Amazon Pinpoint, you can configure Amazon Pinpoint to stream the data to Kinesis or Amazon S3\.

## Integrating Amazon Pinpoint with Mobile Apps<a name="incorporating-dartboard"></a>

Amazon Pinpoint can capture and present information about user engagement in mobile apps\. To do this, you integrate Amazon Pinpoint into your app code so the app can connect to the service and then report events about app usage\. Amazon Pinpoint compiles the app usage events into user engagement analytics that you use to identify and engage with specific segments of users\.
# What Is Amazon Pinpoint?<a name="welcome"></a>

Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels\. You can send push notifications, emails, or text messages \(SMS\), depending on the purpose of your campaign\.

This section describes the major features of Amazon Pinpoint\.

## Define Audience Segments<a name="welcome-segments"></a>

Reach the right audience for your messages by defining audience segments\. A segment designates which users receive the messages that are sent from a campaign\. You can define dynamic segments based on data that's reported by your application, such as operating system or mobile device type\. You can also import static segments that you define outside of Amazon Pinpoint\.

## Engage Your Audience with Messaging Campaigns<a name="welcome-campaigns"></a>

Engage your audience by creating a messaging campaign\. A campaign sends tailored messages on a schedule that you define\. You can create campaigns that send mobile push, email, or SMS messages\.

To experiment with alternative campaign strategies, set up your campaign as an A/B test, and analyze the results with Amazon Pinpoint analytics\.

## Send Direct Messages<a name="welcome-transactional"></a>

Keep your customers informed by sending direct mobile push and SMS messages—such as new account activation messages, order confirmations, and password reset notifications—to specific users\. You can send direct messages from the Amazon Pinpoint console, or by using the Amazon Pinpoint REST API\.

## Analyze User Behavior<a name="welcome-analyze"></a>

Gain insights about your audience and the effectiveness of your campaigns by using the analytics that Amazon Pinpoint provides\. You can view trends about your users' level of engagement, purchase activity, and demographics\. You can monitor your message traffic with metrics for messages sent and opened\. Through the Amazon Pinpoint API, your application can report custom data, which Amazon Pinpoint makes available for analysis\.

To analyze or store the analytics data outside of Amazon Pinpoint, you can configure Amazon Pinpoint to stream the data to Amazon Kinesis\.

## Integrate Amazon Pinpoint with Your Applications<a name="incorporating-dartboard"></a>

Amazon Pinpoint can capture information about the ways your customers interact with your applications\. If you're a mobile app developer, you can integrate Amazon Pinpoint into your app code so the app can connect to the service and then report events about app usage\. Amazon Pinpoint compiles the app usage events into user engagement analytics that you can use to identify and engage with specific segments of users\.

You can use the Amazon Pinpoint API to export application event data, define customer segments, and create and execute multichannel campaigns\. You can also use the API to send transactional SMS and mobile push messages directly to specific recipients\.

For more information about the Amazon Pinpoint API, see the [Amazon Pinpoint API Reference](http://docs.aws.amazon.com/pinpoint/latest/apireference/)\.

### AWS SDK Support<a name="welcome-incorporating-sdks"></a>

One of the easiest ways to interact with the Amazon Pinpoint API is to use an AWS SDK\. The following AWS SDKs include support for Amazon Pinpoint API operations:

+ [AWS Mobile SDK for Android](https://aws.amazon.com/sdkforandroid/) version 2\.3\.5 or later

+ [AWS Mobile SDK for iOS](https://aws.amazon.com/sdkforios/) version 2\.4\.14 or later

+ [AWS SDK for JavaScript](https://aws.amazon.com/sdk-for-node-js/) version 2\.7\.10 or later

+ [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) version 1\.11\.63 or later

+ [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/) version 3\.3\.27\.0 or later

+ [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) version 3\.20\.1

+ [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/) version 1\.4\.2 or later

+ [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) version 1\.0\.0\.rc2 or later

+ [AWS SDK for Go](https://aws.amazon.com/sdk-for-go/) version 1\.5\.13 or later

+ [AWS SDK for C\+\+](https://aws.amazon.com/sdk-for-cpp/) version 1\.0\.20151208\.143 or later

You can also interact with the Amazon Pinpoint API by using version 1\.11\.24 or later of the [AWS Command Line Interface](https://aws.amazon.com/cli/) \(AWS CLI\)\. The AWS CLI requires Python 2 version 2\.6\.5 or later, or Python 3 version 3\.3 or later\. For more information about installing and configuring the AWS CLI, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

**Note**  
The version numbers shown in this section are the first versions of each SDK or CLI that included support for the Amazon Pinpoint API\. New resources or operations are occasionally added to the Amazon Pinpoint API\. In order to use all of the features of Amazon Pinpoint through the API, ensure that you're using the latest version of the SDK or CLI\.
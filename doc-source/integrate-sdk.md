# Integrating the AWS Mobile SDKs or JavaScript Library with Your Application<a name="integrate-sdk"></a>

To connect to the Amazon Pinpoint service from your web or mobile application, integrate the AWS Mobile SDKs or JavaScript library with your code\. With these resources, you can use the native programming language for your platform to issue requests to the Amazon Pinpoint API\. For example, you can add endpoints, apply custom endpoint attributes, report usage data, and more\.

For Android or iOS apps, use the AWS Mobile SDKs\. For web or mobile JavaScript applications, use the AWS Amplify JavaScript library for web and React Native\.

## Integrating the AWS Mobile SDKs for Android or iOS<a name="integrate-sdk-mobile"></a>

To integrate the AWS Mobile SDK for Android or iOS with your app, see [Get Started \(Android and iOS\)](https://docs.aws.amazon.com/aws-mobile/latest/developerguide/getting-started.html) in the *AWS Mobile Developer Guide*\. This topic helps you:
+ Create a project with AWS Mobile Hub\. The **Messaging & Analytics** feature is enabled by default\.
+ Connect your app to the backend AWS resources that Mobile Hub provisions\.
+ Integrate the AWS Mobile SDK for iOS or Android with your app\.

After you integrate the SDK, return to this topic in the *Amazon Pinpoint Developer Guide* for the next step\.

## Integrating the AWS Amplify JavaScript Library<a name="integrate-sdk-amplify"></a>

To integrate AWS Amplify with your JavaScript application, see [Get Started \(Web\)](https://docs.aws.amazon.com/aws-mobile/latest/developerguide/web-getting-started.html) or [Get Started \(React Native\)](https://docs.aws.amazon.com/aws-mobile/latest/developerguide/react-native-getting-started.html) in the *AWS Mobile Developer Guide*\. These topics help you:
+ Use the AWS Mobile CLI to create a project\. Analytics and web hosting are enabled by default\.
+ Create backend AWS resources for your application\.
+ Connect your app to the backend resources\.
+ Integrate the AWS Amplify library with your application\.

After you integrate AWS Amplify, return to this topic in the *Amazon Pinpoint Developer Guide* for the next step\.

## Migrating from Amazon Mobile Analytics in the AWS Mobile SDKs<a name="integrate-sdk-migrate-ama"></a>

Amazon Mobile Analytics preceded Amazon Pinpoint as an AWS service that you could use to report and monitor analytics from your mobile app\. Mobile Analytics features are now provided exclusively by Amazon Pinpoint\. If you're currently using Mobile Analytics with the AWS Mobile SDKs, your apps will continue to report events, and this event data is shown in the Amazon Pinpoint console\. However, we recommend that you switch to the Amazon Pinpoint analytics features in the latest AWS Mobile SDK or JavaScript library, which offer matching functionality\. By migrating, you help ensure that you're able to use the latest features and that you receive support for any issues\.

To migrate, see [Migrating to Amazon Pinpoint in the AWS Mobile SDKs or JavaScript Library](https://docs.aws.amazon.com/mobileanalytics/latest/ug/migrate-sdk.html) in the *Amazon Mobile Analytics User Guide*\.

## Next Step<a name="integrate-sdk-next"></a>

You've integrated an AWS Mobile SDK or AWS Amplify with your application\. Now you can easily add Amazon Pinpoint API requests to your code\. Next, update your code to register your users' devices as endpoints\. See [Registering Endpoints in Your Application](integrate-endpoints.md)\.
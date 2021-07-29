# Registering endpoints in your application<a name="integrate-endpoints"></a>

When a user starts a session \(for example, by launching your mobile app\), your mobile or web application can automatically register \(or update\) an *endpoint* with Amazon Pinpoint\. The endpoint represents the device that the user starts the session with\. It includes attributes that describe the device, and it can also include custom attributes that you define\. Endpoints can also represent other methods of communicating with customers, such as email addresses or mobile phone numbers\. 

After your application registers endpoints, you can segment your audience based on endpoint attributes\. You can then engage these segments with tailored messaging campaigns\. You can also use the **Analytics** page in the Amazon Pinpoint console to view charts about endpoint registration and activity, such as **New endpoints** and **Daily active endpoints**\.

You can assign a single user ID to multiple endpoints\. A user ID represents a single user, while each endpoint that is assigned the user ID represents one of the user's devices\. After you assign user IDs to your endpoints, you can view charts about user activity in the console, such as **Daily active users** and **Monthly active users**\. 

## Before you begin<a name="integrate-endpoints-before"></a>

If you haven't already, integrate the AWS Mobile SDK for Android or iOS, or integrate the AWS Amplify JavaScript library with your application\. See [Integrating the AWS mobile SDKs or JavaScript library with your application](integrate-sdk.md)\.

## Registering endpoints with the AWS mobile SDKs for Android or iOS<a name="integrate-endpoints-mobile"></a>

You can use the AWS Mobile SDKs for Android or iOS to register and customize endpoints\. For more information, and to view code examples, see the following documents:
+ [Registering endpoints in your application](https://aws-amplify.github.io/docs/android/analytics#registering-endpoints-in-your-application) in the Android SDK documentation\.
+ [Registering endpoints in your application](https://docs.amplify.aws/lib/analytics/getting-started/q/platform/ios) in the iOS SDK documentation\.

## Registering endpoints with the AWS Amplify JavaScript library<a name="integrate-events-amplify"></a>

You can use the AWS Amplify JavaScript library to register and update endpoints in your apps\. For more information, and to view code examples, see [Update endpoint](https://aws-amplify.github.io/docs/js/analytics#update-endpoint) in the AWS Amplify JavaScript documentation\.

## Next steps<a name="integrate-endpoints-next"></a>



You've updated your app to register endpoints\. Now, as users launch your app, device information and custom attributes are provided to Amazon Pinpoint\. You can use this information to define audience segments\. In the console, you can see metrics about endpoints and, if applicable, users who are assigned user IDs\.

Next, complete the steps at [Reporting events in your application](integrate-events.md) to update your app to report usage data\.
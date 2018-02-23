# Adding an Android App to Amazon Pinpoint<a name="getting-started-android-mobilehub"></a>

Add an Android app to Amazon Pinpoint by creating a project in AWS Mobile Hub\.

**Prerequisite**  
To complete this task, you need:  
A project that has push messaging enabled with Firebase or Google Cloud Platform\. For more information, see [Step 1: Create a Firebase Project](mobile-push-android-cloud-messaging-project.md)\.
A Firebase Cloud Messaging \(FCM\) or Google Cloud Messaging \(GCM\) API key and a sender ID \(also called project number\)\. For more information, see [Step 2: Get Push Messaging Credentials for Android](mobile-push-android-creds.md)\.

**To add an Android app**

1. Sign in to the AWS Management Console and open the Mobile Hub console at [https://console.aws.amazon.com/mobilehub](https://console.aws.amazon.com/mobilehub)\.

1. If you have other Mobile Hub projects, choose **Create new mobile project**\. If this is your first project, skip this step because you are taken directly to the page for creating a new project\.

1. Enter a project name\. The name you enter will be the name of your project in the Amazon Pinpoint console\. 

1. For the region, keep **US East \(Virginia\)**\. 

1. Choose **Create project**\. Mobile Hub creates the project and shows the **Pick and configure features for your project** page\.

1. Choose **Messaging & Analytics**\.

1. On the **Messaging & Analytics** page, for **What engagement features do you want to enable?**, choose **Messaging**\.

1. For **What Messaging Channels do you want to enable?**, choose **Mobile push**\.

1. For **Choose the platforms for which you want to enable push notifications**, choose **Android**\.

1. Enter your **API key** and **Sender ID**, which are available from your project that has push messaging enabled in Firebase or Google Cloud Platform\. In Google Cloud Platform, the sender ID is called project number\.

1. Choose **Enable**\.

Mobile Hub helps you integrate Amazon Pinpoint with your app in two ways:

1. You can download a working sample app that demonstrates Amazon Pinpoint features\. Then, you can [create a virtual device](getting-started-android-virtual-device.md) and [build the sample app](getting-started-android-sampleapp.md) so that you can send it a push notification with Amazon Pinpoint\.

1. You can download a package that includes the required SDKs for your project\. Then, you can follow the instructions in the AWS Mobile Hub console to integrate Amazon Pinpoint with your app\.
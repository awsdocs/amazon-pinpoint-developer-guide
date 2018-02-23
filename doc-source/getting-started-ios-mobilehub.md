# Adding an iOS App to Amazon Pinpoint<a name="getting-started-ios-mobilehub"></a>

Add an iOS app to Amazon Pinpoint by creating a project in AWS Mobile Hub\.

**Prerequisite**  
To complete this task, you need an SSL certificate as a `.p12` file that authorizes Amazon Pinpoint to send push notifications to your app though Apple Push Notification service \(APNs\)\. For more information, see [Setting Up iOS Push Notifications](apns-setup.md)\.

**To add an iOS app**

1. Sign in to the AWS Management Console and open the Mobile Hub console at [https://console.aws.amazon.com/mobilehub](https://console.aws.amazon.com/mobilehub)\.

1. If you have other Mobile Hub projects, choose **Create new mobile project**\. If this is your first project, skip this step because you are taken directly to the page for creating a new project\.

1. Enter a project name\. The name you enter will be the name of your project in the Amazon Pinpoint console\. 

1. For the region, keep **US East \(Virginia\)**\. 

1. Choose **Create project**\. Mobile Hub creates the project and shows the **Pick and configure features for your project** page\.

1. Choose **Messaging & Analytics**\.

1. On the **Messaging & Analytics** page, for **What engagement features do you want to enable?**, choose **Messaging**\.

1. For **What Messaging Channels do you want to enable?**, choose **Mobile push**\.

1. For **Choose the platforms for which you want to enable push notifications**, choose **iOS**\.

1. For **P12 Certificate**, provide the P12 certificate that authorizes Amazon Pinpoint to send push notifications to your app through Apple Push Notification service \(APNs\)\. Then, choose **Upload**\.

1. Choose **Enable**\. 

Mobile Hub helps you integrate Amazon Pinpoint with your app in two ways:

1. You can download a working sample app that demonstrates Amazon Pinpoint features\. Then, you can [build the sample app](getting-started-ios-sampleapp.md) and send it a push notification with Amazon Pinpoint\.

1. You can download a package that includes the required SDKs for your project\. Then, you can follow the instructions in the AWS Mobile Hub console to integrate Amazon Pinpoint with your app\.

Now that you have created a Mobile Hub project for your app, you can see your app in the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.
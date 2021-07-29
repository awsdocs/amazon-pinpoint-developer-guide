# Create a project in Amazon Pinpoint<a name="mobile-push-create-project"></a>

In Amazon Pinpoint, a *project* is a collection of settings, data, campaigns, and segments that all share a common purpose\. In the Amazon Pinpoint API, projects are also referred to as *applications*\. This section uses the word "project" exclusively when referring to this concept\.

To start sending push notifications in Amazon Pinpoint, you have to create a project\. Next, you have to enable the push notification channels that you want to use by providing the appropriate credentials\.

You can create new projects and set up push notification channels by using the Amazon Pinpoint console\. For more information, see [Setting up Amazon Pinpoint push notification channels](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-mobile-setup.html) in the *Amazon Pinpoint User Guide*\.

You can also create and set up projects by using the [Amazon Pinpoint API](https://docs.aws.amazon.com/pinpoint/latest/apireference/), an [AWS SDK](https://aws.amazon.com/tools/#sdk), or the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/) \(AWS CLI\)\. To create a project, use the `Apps` resource\. To configure push notification channels, use the following resources:
+ [APNs channel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html) to send messages to users of iOS devices by using the Apple Push Notification service\.
+ [ADM channel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html) to send messages to users of Amazon Kindle Fire devices\.
+ [Baidu channel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html) to send messages to Baidu users\.
+ [GCM channel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html) to send messages to Android devices by using Firebase Cloud Messaging \(FCM\), which replaces Google Cloud Messaging \(GCM\)\.
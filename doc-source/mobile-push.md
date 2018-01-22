# Setting Up Push Notifications for Amazon Pinpoint<a name="mobile-push"></a>

Before using Amazon Pinpoint, you must add your app as a project in AWS Mobile Hub\. When you add your project, you provide the credentials that authorize Amazon Pinpoint to send messages to your app through the push notification services for iOS and Android:

+ For iOS apps, you provide an SSL certificate, which you obtain from the Apple Developer portal\. The certificate authorizes Amazon Pinpoint to send messages to your app through Apple Push Notification service \(APNs\)\.

+ For Android apps, you provide an API Key and a sender ID, which you obtain from the Firebase console or the Google API console\. These credentials authorize Amazon Pinpoint to send messages to your app through Firebase Cloud Messaging or its predecessor, Google Cloud Messaging\.

After you have the credentials, you can complete the steps in [Getting Started: Creating an Mobile App With Amazon Pinpoint Support](getting-started.md)\.

If you already have the credentials, you can skip this section\.


+ [Setting Up iOS Push Notifications](apns-setup.md)
+ [Setting Up Android Push Notifications](mobile-push-android.md)
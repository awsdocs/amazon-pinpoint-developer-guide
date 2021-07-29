# Setting up push notifications for Amazon Pinpoint<a name="mobile-push"></a>

In order to set up Amazon Pinpoint so that it can send push notifications to your apps, you first have to provide the credentials that authorize Amazon Pinpoint to send messages to your app\. The credentials that you provide depend on which push notification system you use:
+ For iOS apps, you provide an SSL certificate, which you obtain from the Apple Developer portal\. The certificate authorizes Amazon Pinpoint to send messages to your app through the Apple Push Notification service\.
+ For Android apps, you provide a Web API Key, which you obtain from the Firebase console\. These credentials authorize Amazon Pinpoint to send messages to your app through Firebase Cloud Messaging\.

After you obtain the credentials for a push notification channel, you have to create a project in Amazon Pinpoint and provide it with the credentials for the push notification service\.

**Topics**
+ [Setting up iOS push notifications](apns-setup.md)
+ [Setting up Android push notifications](mobile-push-android.md)
+ [Create a project in Amazon Pinpoint](mobile-push-create-project.md)
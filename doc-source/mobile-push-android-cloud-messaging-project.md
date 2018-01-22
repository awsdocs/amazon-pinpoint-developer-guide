# Step 1: Create a Firebase Project<a name="mobile-push-android-cloud-messaging-project"></a>

To send push notifications to Android apps, you must have a project that is enabled with an Android push notification service\. The push notification services for Android are Firebase Cloud Messaging \(FCM\) and its predecessor, Google Cloud Messaging \(GCM\)\.

If you are new to push messaging on Android, you must create a Firebase project, as this topic describes\. However, if you have an existing Google Cloud Messaging project that has push messaging enabled, you can skip this step and use that project instead\.

**To create a Firebase project**

1. Go to the Firebase console at [https://console\.firebase\.google\.com/](https://console.firebase.google.com/)\. If you are not signed in to Google, the link takes you to a sign\-in page\. After you sign in, you see the Firebase console\.

1. Choose **Create New Project**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-fcm-new-project.png)

1. Type a project name, and then choose **Create Project**\.

   Firebase projects support push messaging by default\.
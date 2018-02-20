# Step 2: Get Push Messaging Credentials for Android<a name="mobile-push-android-creds"></a>

To send push notifications to Android apps, you must have credentials from either Firebase Cloud Messaging \(FCM\) or its predecessor, Google Cloud Messaging \(GCM\)\. The credentials are an API key and a sender ID \(also called project number\)\. You get these credentials from a project that has push messaging enabled\. This project could either be in the Firebase console or the Google Cloud Platform console, depending on where you created it\.

This topic describes how to retrieve your credentials from FCM or GCM\. Use FCM for new Android apps\. Use GCM only if you have a preexisting GCM project that you have not yet updated for FCM support\.

**To obtain your credentials from FCM**

1. Go to the Firebase console at [https://console\.firebase\.google\.com/](https://console.firebase.google.com/) and open your project\.

1. In the left pane, to the right of your project name, choose the gear icon, and then choose **Project Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-fcm-project-settings.png)

1. In the top menu, choose **Cloud Messaging**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-fcm-project-messaging.png)

1. Under **Project credentials**, you find the API key and sender ID\. Save these values somewhere you can access later\.

**To obtain your credentials from GCM**

1. Go to the Google API Console at [https://console\.developers\.google\.com](https://console.developers.google.com)\.

1. In the left pane, choose **Credentials**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-gcm-credentials-in-menu.png)

1. If you already have credentials for your app, your server key is shown in the **API keys** section\. Save this key somewhere you can access later\.

1. If you don't have credentials for your app, the console displays the **Credentials** dialog box\. Create a server key by completing the following steps:

   1. Choose **Create credentials**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-gcm-credentials-create.png)

   1. Save the API key somewhere you can access later\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-gcm-credentials-api-key-display.png)

1. To retrieve your sender ID \(also called project number\), go to the Google Cloud Platform console at [https://console.cloud.google.com/](https://console.cloud.google.com/)\. Select your project from the **Project** menu\. Then, choose the arrow next to the project name\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-gcm-credentials-expand-project.png)

1. Save the displayed project number somewhere you can access later\.
**Note**  
Project number is another name for sender ID\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-gcm-credentials-sender-id.png)
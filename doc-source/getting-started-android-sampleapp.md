# Building the Sample Android App From AWS Mobile Hub<a name="getting-started-android-sampleapp"></a>

This topic describes how to import and build the AWS Mobile Hub sample app code in Android Studio\. We assume that you have generated the quickstart app code using the AWS Mobile Hub console, and downloaded the code in the \.zip file to your computer\.

**Prerequisites**  
To complete this task, you need:  
A Mobile Hub sample Android app with Amazon Pinpoint support\. If you need to create the sample app, see [Adding an Android App to Amazon Pinpoint](getting-started-android-mobilehub.md)\.
Android Studio with the following SDK support, which you can download from [https://developer\.android\.com/studio/index\.html](https://developer.android.com/studio/index.html):  
Android Studio 1\.4 or newer
Android SDK 4\.4 \(KitKat\) API Level 19 or newer
Android SDK Build\-tools 23\.0\.1
A physical or virtual Android device with Android OS 4\.0\.3 \(IceCreamSandwich\) API Level 15 or newer and Google Play Services\.

**To build the AWS Mobile Hub sample app**

1. Unzip the \.zip file that you downloaded from AWS Mobile Hub\.

1. Open Android Studio and choose **Import project \(Eclipse ADT, Gradle, etc\.\)**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-import.png)

1. Open the folder where you extracted the \.zip file contents and choose the **MySampleApp** folder\. 

   Android Studio imports the project and builds it using Gradle\.

1. If a dialog box appears recommending that you update your Gradle Plug\-in, choose **Don't remind me again for this project**\.

After the build completes, you can install and run the app on a physical or virtual Android device\.

**To run the app on a virtual Android device**

1. In Android Studio, choose the run icon in the toolbar\.

1. In the **Select a Deployment Target** window, select your virtual device, and choose **OK**\. The Android emulator opens, and the sample app launches in the virtual device\. You can choose **User Sign\-in** or **User Engagement** to learn more about these features\.  
![\[An Android emulator running the sample app.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/app_screen_android.png)![\[An Android emulator running the sample app.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[An Android emulator running the sample app.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the Android emulator, sign in to your Google account so that the sample app can receive push notifications from Amazon Pinpoint:

   1. Open **Settings**\.

   1. Choose **Accounts**\.

   1. Choose **Add account**, **Google**, and provide your Google email address and password\.
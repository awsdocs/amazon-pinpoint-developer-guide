# Amazon Pinpoint Developer Guide

-----
*****Copyright &copy; 2018 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is Amazon Pinpoint?](welcome.md)
+ [Setting Up Push Notifications for Amazon Pinpoint](mobile-push.md)
   + [Setting Up iOS Push Notifications](apns-setup.md)
      + [Step 1: Create an App ID](apns-setup-appid.md)
      + [Step 2: Create an APNs SSL Certificate](apns-setup-apnscert.md)
      + [Step 3: Register a Test Device](apns-setup-device.md)
      + [Step 4: Create an iOS Distribution Certificate](apns-setup-ioscert.md)
      + [Step 5: Create a Provisioning Profile](apns-setup-provisioning-profile.md)
   + [Setting Up Android Push Notifications](mobile-push-android.md)
      + [Step 1: Create a Firebase Project](mobile-push-android-cloud-messaging-project.md)
      + [Step 2: Get Push Messaging Credentials for Android](mobile-push-android-creds.md)
+ [Getting Started: Creating an Mobile App With Amazon Pinpoint Support](getting-started.md)
   + [Getting Started With iOS Apps](getting-started-ios.md)
      + [Adding an iOS App to Amazon Pinpoint](getting-started-ios-mobilehub.md)
      + [Building the Sample iOS App From AWS Mobile Hub](getting-started-ios-sampleapp.md)
   + [Getting Started With Android Apps](getting-started-android.md)
      + [Adding an Android App to Amazon Pinpoint](getting-started-android-mobilehub.md)
      + [Creating an Android Virtual Device](getting-started-android-virtual-device.md)
      + [Building the Sample Android App From AWS Mobile Hub](getting-started-android-sampleapp.md)
   + [Testing the Sample App With Amazon Pinpoint](getting-started-sampletest.md)
+ [Integrating Amazon Pinpoint with a Mobile App](mobile-sdk.md)
   + [Integrating Amazon Pinpoint With iOS Apps](mobile-sdk-ios.md)
      + [Setting Up the AWS Mobile SDK for iOS](mobile-sdk-ios-setup.md)
      + [Initializing the Amazon Pinpoint Client](mobile-sdk-ios-modify.md)
      + [Registering Endpoints](mobile-sdk-ios-register.md)
      + [Reporting Events](mobile-sdk-ios-events.md)
      + [Handling Push Notifications](mobile-sdk-ios-push.md)
         + [Handling Push Notifications from Apple Push Notification Service](mobile-sdk-ios-push-apns.md)
         + [Setting Up Deep Linking](mobile-sdk-ios-deep-linking.md)
   + [Integrating Amazon Pinpoint with Android Apps](mobile-sdk-android.md)
      + [Setting up the AWS Mobile SDK for Android](mobile-sdk-android-setup.md)
      + [Initializing the Amazon Pinpoint Client](mobile-sdk-android-modify.md)
      + [Managing Sessions](mobile-sdk-android-sessions.md)
      + [Registering Endpoints](mobile-sdk-android-register.md)
      + [Reporting Events](mobile-sdk-android-events.md)
      + [Handling Push Notifications](mobile-sdk-android-push.md)
         + [Handling Push Notifications from Firebase Cloud Messaging or Google Cloud Messaging](mobile-sdk-android-push-fcm.md)
         + [Handling Push Notifications from Amazon Device Messaging](mobile-sdk-android-push-adm.md)
         + [Handling Push Notifications from Baidu Cloud Push](mobile-sdk-android-push-baidu.md)
         + [Setting up Deep Linking](mobile-sdk-android-deep-linking.md)
+ [Adding Endpoints](endpoints.md)
+ [Creating Segments](segments.md)
   + [Building Segments](segments-dimensional.md)
   + [Importing Segments](segments-importing.md)
   + [Customizing Segments with AWS Lambda](segments-dynamic.md)
+ [Creating Campaigns](campaigns.md)
+ [Streaming Amazon Pinpoint Events to Kinesis](analytics-streaming.md)
+ [Permissions](permissions.md)
   + [IAM Policies for Amazon Pinpoint Users](permissions-actions.md)
   + [User Authentication in Amazon Pinpoint Apps](permissions-authentication.md)
   + [AWS Mobile Hub Service Role](permissions-mobilehub.md)
   + [IAM Role for Importing Segments](permissions-import.md)
   + [IAM Role for Streaming Events to Kinesis](permissions-streams.md)
+ [Limits in Amazon Pinpoint](limits.md)
+ [Creating Custom Channels with AWS Lambda](channels-custom.md)
+ [Document History for Amazon Pinpoint](doc-history.md)
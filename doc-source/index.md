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
+ [Integrating Amazon Pinpoint with Your Application](integrate.md)
   + [AWS SDK Support for Amazon Pinpoint](integrate-supported-sdks.md)
   + [Integrating the AWS Mobile SDKs or JavaScript Library with Your Application](integrate-sdk.md)
   + [Registering Endpoints in Your Application](integrate-endpoints.md)
   + [Managing Sessions in Your Application (Android Only)](integrate-sessions-android.md)
   + [Reporting Events in Your Application](integrate-events.md)
   + [Handling Push Notifications](integrate-push.md)
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
      + [Handling Push Notifications](integrate-push-services.md)
         + [Handling Push Notifications from Apple Push Notification Service](mobile-sdk-ios-push-apns.md)
         + [Handling Push Notifications from Firebase Cloud Messaging or Google Cloud Messaging](mobile-sdk-android-push-fcm.md)
         + [Handling Push Notifications from Amazon Device Messaging](mobile-sdk-android-push-adm.md)
         + [Handling Push Notifications from Baidu Cloud Push](mobile-sdk-android-push-baidu.md)
+ [Defining Your Audience to Amazon Pinpoint](audience-define.md)
   + [Adding Endpoints to Amazon Pinpoint](audience-define-endpoints.md)
   + [Associating Users with Amazon Pinpoint Endpoints](audience-define-user.md)
   + [Adding a Batch of Endpoints to Amazon Pinpoint](audience-define-endpoints-batch.md)
   + [Importing Endpoints into Amazon Pinpoint](audience-define-import.md)
   + [Deleting Endpoints from Amazon Pinpoint](audience-define-remove.md)
+ [Accessing Audience Data in Amazon Pinpoint](audience-data.md)
   + [Looking Up Endpoints with Amazon Pinpoint](audience-data-endpoints.md)
   + [Exporting Endpoints from Amazon Pinpoint](audience-data-export.md)
   + [Listing Endpoint IDs with Amazon Pinpoint](audience-data-list-ids.md)
+ [Creating Segments](segments.md)
   + [Building Segments](segments-dimensional.md)
   + [Importing Segments](segments-importing.md)
   + [Customizing Segments with AWS Lambda](segments-dynamic.md)
+ [Creating Campaigns](campaigns.md)
+ [Streaming Amazon Pinpoint Events to Kinesis](analytics-streaming.md)
+ [Logging Amazon Pinpoint API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Deleting Data from Amazon Pinpoint](deleting-data.md)
+ [Permissions](permissions.md)
   + [IAM Policies for Amazon Pinpoint Users](permissions-actions.md)
   + [User Authentication in Amazon Pinpoint Apps](permissions-authentication.md)
   + [AWS Mobile Hub Service Role](permissions-mobilehub.md)
   + [IAM Role for Importing Endpoints or Segments](permissions-import-segment.md)
   + [IAM Role for Exporting Endpoints or Segments](permissions-export-endpoints.md)
   + [IAM Role for Streaming Events to Kinesis](permissions-streams.md)
+ [Limits in Amazon Pinpoint](limits.md)
+ [Creating Custom Channels with AWS Lambda](channels-custom.md)
+ [Document History for Amazon Pinpoint](doc-history.md)
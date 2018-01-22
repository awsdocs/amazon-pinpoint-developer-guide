# Setting Up iOS Push Notifications<a name="apns-setup"></a>

Push notifications for iOS apps are sent using Apple Push Notification service \(APNs\)\. Before you can send push notifications to iOS devices, you must create an app ID on the Apple Developer portal, and you must create the required certificates\.

This section describes how to use the Apple Developer portal to obtain iOS and APNs credentials\. These credentials enable you to create an iOS project in AWS Mobile Hub and launch a sample app that can receive push notifications\.

You do not need an existing iOS app to complete the steps in this section\. After you create an iOS project in Mobile Hub, you can download and launch a working sample app\. Mobile Hub automatically provisions the AWS resources that your app requires\.

After completing the steps in this section, you will have the following items in your Apple Developer account:

+ An app ID\.

+ An SSL certificate, which authorizes you to send push notifications to your app through APNs\.

+ A registration for your test device, such as an iPhone, with your Apple Developer account\.

+ An iOS distribution certificate, which enables you to install your app on your test device\.

+ A provisioning profile, which allows your app to run on your test device\.

If you already have these items, you can skip this section and complete the steps in [Getting Started With iOS Apps](getting-started-ios.md)\.

Before you begin, you must have an account with the Apple Developer Program as an individual or as part of an organization, and you must have agent or admin privileges in that account\.

The steps in this tutorial are based on the following versions of Mac OS software:

+ OS X El Capitan version 10\.11\.6

+ Xcode version 8\.1


+ [Step 1: Create an App ID](apns-setup-appid.md)
+ [Step 2: Create an APNs SSL Certificate](apns-setup-apnscert.md)
+ [Step 3: Register a Test Device](apns-setup-device.md)
+ [Step 4: Create an iOS Distribution Certificate](apns-setup-ioscert.md)
+ [Step 5: Create a Provisioning Profile](apns-setup-provisioning-profile.md)
# Building the Sample iOS App From AWS Mobile Hub<a name="getting-started-ios-sampleapp"></a>

To test the Amazon Pinpoint features that you enabled in AWS Mobile Hub, build the sample iOS app and launch it on a test device\. Then, create a simple push notification campaign in Amazon Pinpoint and send a message to the sample app\.

**Prerequisites**  
To complete this task, you need:  
A Mobile Hub sample iOS app with Amazon Pinpoint support\. If you need to create the sample app, see [Adding an iOS App to Amazon Pinpoint](getting-started-ios-mobilehub.md)\.
A Mac with Xcode installed\. The following procedure is based on OS X El Capitan version 10\.11\.6 and Xcode version 8\.1\.
The following items from the Apple Developer portal:  
An app ID\.
A registration for a test device \(such as an iPhone\)\.
An iOS distribution certificate installed in Keychain on your Mac\.
An Ad Hoc distribution provisioning profile installed in Xcode on your Mac\.
If you need to obtain these items, see [Setting Up iOS Push Notifications](apns-setup.md)\.

**To build the sample app**

1. In the AWS Mobile Hub console, on the **Integrate** page for your app, choose **Download a sample app**, and save the sample app to your local drive\.

1. Browse to the package you downloaded and open the `MySampleApp.xcodeproj` file to open the sample app in Xcode\.

1. Open the target for the sample app project, and choose **General**\.

1. In the **Identity** section, for **Display Name**, type a custom display name to make the sample app easy to recognize after you install it on your device, such as **Pnpt Sample**\.

1. For **Bundle Identifier**, type the bundle ID associated with the provisioning profile for your app\.  
![\[The identity section in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_identity.png)![\[The identity section in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The identity section in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Signing \(Debug\)** and **Signing \(Release\)** sections, for **Provisioning Profile**, select your provisioning profile\. Xcode shows only those provisioning profiles that are associated with the bundle ID that you provided\.  
![\[The signing sections in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_signing.png)![\[The signing sections in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The signing sections in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Product** menu, choose **Archive**\.   
![\[The archive menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_archive.png)![\[The archive menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The archive menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Archives** window, choose **Export**\.  
![\[The export button in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_export.png)![\[The export button in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The export button in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Select a method for export** window, select **Save for Ad Hoc Deployment**, and choose **Next**\.

1. When prompted, choose your development team\.

1. In the **Device Support** window, select **Export one app for all compatible devices**, and choose **Next**\.

1. In the **Summary** window, choose **Next**\.

1. Browse to the location on your local drive where you want to save the exported `.ipa` file, and choose **Export**\.

**To launch the sample app on your test device**

1. Connect your device to your Mac with a USB cable\.

1. In the Xcode menu bar, choose **Window**, and then choose **Devices**\.  
![\[The Devices menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_devices.png)![\[The Devices menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The Devices menu option in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Devices** section, select your device\.

1. In the **Installed Apps** section, choose the plus icon\.  
![\[The Devices window in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/xcode_deviceswindow.png)![\[The Devices window in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The Devices window in Xcode.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Browse to the location of your exported `.ipa` file, select it, and choose **Open**\. The sample app is installed, and you can see the app icon on your device\.  
![\[The sample app icon on a device.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/app_image.png)![\[The sample app icon on a device.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The sample app icon on a device.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Launch the app on your device\. If this your first time launching an app signed with your distribution certificate, you receive an **Untrusted Developer** message, and the app won't launch\.   
![\[The Untrusted Developer message.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/app_untrusted.png)![\[The Untrusted Developer message.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The Untrusted Developer message.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

   Before you can open your app, you must trust your distribution certificate on your device\. Tap **Settings**, **General**, and finally **Device Management**\. Then, tap the distribution certificate, tap **Trust**, and when prompted, tap **Trust**\.

   Launch the app again\.

1. When the app requests permission to send you notifications, tap **Allow**\.

1. In the sample app, you can tap **User Sign\-in** or **User Engagement** to learn more about these features\.  
![\[The sample app screen after launching.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/app_screen.png)![\[The sample app screen after launching.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The sample app screen after launching.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

Now that you have built the sample iOS app, you can [test the app](getting-started-sampletest.md) by sending it a push notification from Amazon Pinpoint\.
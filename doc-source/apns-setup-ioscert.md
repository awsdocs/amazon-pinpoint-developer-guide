# Step 4: Create an iOS Distribution Certificate<a name="apns-setup-ioscert"></a>

An iOS distribution certificate enables you to install your app on a test device and deliver push notifications to that device\. You specify your iOS distribution certificate later when you create a provisioning profile for your app\.

If you already have an iOS distribution certificate, you can skip this step\.

**To create an iOS distribution certificate**

1. On the **Certificates, Identifiers & Profiles** page of your Apple Developer account, in the **Certificates section**, choose **Production**\.

1. In the **iOS Certificates \(Production\)** pane, choose the Add button \(\+\)\. The **Add iOS Certificate** pane opens\.

1. In the **Production** section, select **App Store and Ad Hoc**, and then choose **Continue**\.

1. On the **About Creating a Certificate Signing Request \(CSR\)** page, choose **Continue**\.

1. In the **About Creating a Certificate Signing Request \(CSR\)** pane, follow the instructions for creating a Certificate Signing Request \(CSR\) file\. You use the Keychain Access application on your Mac to create the request and save it on your local disk\. When you are done, choose **Continue**\.

1. In the **Generate your certificate** pane, choose **Choose File\.\.\.**, and then select the CSR file you created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert13.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Choose **Continue**\.

1. When the certificate is ready, choose **Download** to save the certificate to your computer\.

1. Double\-click the downloaded certificate to install it in Keychain on your Mac\.
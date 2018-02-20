# Step 2: Create an APNs SSL Certificate<a name="apns-setup-apnscert"></a>

Create an APNs SSL certificate so that you can deliver push notifications to your app through APNs\. You will create and download the certificate from your Apple Developer account\. Then, you will install the certificate in Keychain Access and export it as a \.p12 file\.

If you already have an SSL certificate for your app, you can skip this step\.

**To create an SSL certificate for push notifications**

1. Sign in to your Apple Developer account at [https://developer\.apple\.com/membercenter/index\.action](https://developer.apple.com/membercenter/index.action)\.

1. Choose **Certificates, Identifiers & Profiles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert04.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. From the **Identifiers** section, choose **App IDs**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert05.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. From the list of iOS app IDs, select the app ID that you created in [Step 1: Create an App ID](apns-setup-appid.md)\.

1. Choose **Edit**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert11.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Under **Push Notifications**, in the **Production SSL Certificate** section, choose **Create Certificate\.\.\.**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert12.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **About Creating a Certificate Signing Request \(CSR\)** pane, follow the instructions for creating a Certificate Signing Request \(CSR\) file\. You use the Keychain Access application on your Mac to create the request and save it on your local disk\. When you are done, choose **Continue**\.

1. In the **Generate your certificate** pane, choose **Choose File\.\.\.**, and then select the CSR file you created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert13.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Choose **Continue**\.

1. When the certificate is ready, choose **Download** to save the certificate to your computer\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert15.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Double\-click the downloaded certificate to install it to the Keychain on your Mac\.

1. On your Mac, start the Keychain Access application\.

1. In **My Certificates**, find the certificate you just added\. The certificate is named "Apple Push Services:*com\.my\.app\.id*", where com\.my\.app\.id is the app ID for which the certificate was created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert17.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Context\-select the push certificate and then select **Export\.\.\.** from the context menu to export a file containing the certificate\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert18.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Type a name for the certificate that is easy to recognize and save it to your computer\. Do not provide an export password when prompted\. You need to upload this certificate when creating your app in AWS Mobile Hub\.
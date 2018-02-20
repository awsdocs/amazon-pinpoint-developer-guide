# Step 5: Create a Provisioning Profile<a name="apns-setup-provisioning-profile"></a>

A provisioning profile allows your app to run on your test device\. You create and download a provisioning profile from your Apple Developer account and then install the provisioning profile in Xcode\.

**To create a provisioning profile**

1. Sign in to your Apple Developer account at [https://developer\.apple\.com/membercenter/index\.action](https://developer.apple.com/membercenter/index.action)\.

1. Select **Certificates, Identifiers & Profiles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert04.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Provisioning Profiles** section, choose **Distribution**\.

1. In the **iOS Provisioning Profiles \(Distribution\)** pane, choose the add button \(\+\)\. The **Add iOS Provisioning Profiles \(Distribution\)** pane is shown\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert20.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Distribution** section, select **Ad Hoc**, and then choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert21.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. For **App ID**, select the app ID you created for your app, and then choose **Continue**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert22.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Select your iOS Development certificate and then choose **Continue**\. 

1. For **Select devices**, select the device that you registered for testing, and choose **Continue**\.

1. Type a name for this provisioning profile, such as **ApnsDistributionProfile**, and choose **Continue**\.

1. Select **Download** to download the generated provisioning profile\.

1. Install the provisioning profile by double\-clicking the downloaded file\. The Xcode app opens in response\.

1. To verify that the provisioning profile is installed, check the list of installed provisioning profiles in Xcode by doing the following: 

   1. In the Xcode menu bar, choose **Xcode** and then choose **Preferences**\.

   1. In the preferences window, choose **Accounts**\.

   1. In the **Accounts** tab, select your Apple ID, and then choose **View Details**\.

   1. Check that your provisioning profile is listed in the **Provisioning Profiles** section\.
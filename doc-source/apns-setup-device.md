# Step 3: Register a Test Device<a name="apns-setup-device"></a>

Register a test device with your Apple Developer account so that you can test your app on that device\. Later, you associate this test device with your provisioning profile, which allows your app to launch on your device\.

If you already have a registered device, you can skip this step\.

**To add a device**

1. Sign in to your Apple Developer account at [https://developer\.apple\.com/membercenter/index\.action](https://developer.apple.com/membercenter/index.action)\.

1. Choose **Certificates, Identifiers & Profiles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert04.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Devices** section, choose the type of device that you want to add, such **iPhone**\.

1. Choose the **Add** button \(\+\)\.

1. In the **Register Device** section, for **Name**, type a name that is easy to recognize later\.

1. For **UDID**, type the unique device ID\. For an iPhone, you can find the UDID by completing the following steps:

   1. Connect your iPhone to your Mac with a USB cable\.

   1. Open the iTunes app\.

   1. In the top left corner of the iTunes window, a button with an iPhone icon is shown\. Choose this button\. iTunes displays the summary page for your iPhone\.

   1. In the top box, the summary page provides your iPhone's **Capacity**, **Phone Number**, and **Serial Number**\. Click **Serial Number**, and the value changes to **UDID**\.

   1. Context\-select your UDID, and choose **Copy**\.

   1. Paste your UDID into the **UDID** field in the Apple Developer website\.

1. Choose **Continue**\.

1. On the **Review and register** pane, verify the details for your device, and choose **Register**\. Your device name and identifier are added to the list of devices\.
# Step 1: Create an App ID<a name="apns-setup-appid"></a>

Create an app ID to identify your app in the Apple Developer portal\. You need this ID when you create an SSL certificate for sending push notifications, when you create an iOS distribution certificate, and when you create a provisioning profile\.

If you already have an ID assigned to your app, you can skip this step\. You can use an existing app ID provided it doesn't contain a wildcard character \("\*"\)\. 

**To assign an App ID to your app**

1. Sign in to your Apple Developer account at [https://developer\.apple\.com/membercenter/index\.action](https://developer.apple.com/membercenter/index.action)\.

1. Choose **Certificates, Identifiers & Profiles**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert04.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Identifiers** section, choose **App IDs**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert05.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **iOS App IDs** pane, choose the **Add** button \(\+\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert06.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **Registering an App ID** pane, for **Name**, type a custom name for your app ID that makes it easy to recognize later\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert07.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Choose the default selection for an App ID Prefix\.

1. For **App ID Suffix**, select **Explicit App ID**, and type a bundle ID for your app\. If you already have an app, use the bundle ID assigned to it\. You can find this ID in the app project in Xcode on your Mac\. Otherwise, take note of the bundle ID because you will assign it to your app in Xcode later\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert08.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Under **App Services** select **Push Notifications**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_appcert09.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Choose **Continue**\. In the **Confirm your App ID** pane, check that all values were entered correctly\. The identifier should match your app ID and bundle ID\.

1. Choose **Register** to register the new app ID\.
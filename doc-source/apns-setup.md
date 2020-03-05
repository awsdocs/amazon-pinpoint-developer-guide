# Setting Up iOS Push Notifications<a name="apns-setup"></a>

Push notifications for iOS apps are sent using the Apple Push Notification service \(APNs\)\. Before you can send push notifications to iOS devices, you must create an app ID on the Apple Developer portal, and you must create the required certificates\. You can find more information about completing these steps in [Setting Up APNs for Push Notifications](https://aws-amplify.github.io/docs/sdk/ios/push-notifications-setup-apns) in the iOS SDK documentation\.

## Working with APNs Tokens<a name="apns-setup-best-practices"></a>

As a best practice, you should develop your app so that your customers' device tokens are regenerated when the app is reinstalled\.

If a recipient upgrades their device to a new major version of iOS \(for example, from iOS 12 to iOS 13\), and later reinstalls your app, the app generates a new token\. If your app doesn't refresh the token, the older token is used to send the notification\. As a result, Apple Push Notification service \(APNs\) rejects the notification, because the token is now invalid\. When you attempt to send the notification, you receive a message failure notification from APNs\.
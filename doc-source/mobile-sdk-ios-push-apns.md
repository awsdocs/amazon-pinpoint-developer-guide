# Handling Push Notifications from Apple Push Notification Service<a name="mobile-sdk-ios-push-apns"></a>

Modify your app code to handle push notifications from Apple Push Notification service \(APNs\)\.

## Registering for Push Notifications<a name="mobile-sdk-ios-modify-register"></a>

Prompt the user to accept receiving push notifications during app launch or when the app requests receipt of push notifications\. After the user grants permission, the app should re\-register when launching the app because the device token can change\.

**Objective\-C**

```
UIUserNotificationType userNotificationTypes =
(UIUserNotificationTypeAlert | UIUserNotificationTypeBadge |
UIUserNotificationTypeSound);
UIUserNotificationSettings *notificationSettings =
[UIUserNotificationSettings settingsForTypes:userNotificationTypes
categories:nil];
[[UIApplication sharedApplication]
registerUserNotificationSettings:notificationSettings];
```

**Swift**

```
let settings = UIUserNotificationSettings(forTypes: [.Sound, .Alert, .Badge], categories: nil)     
UIApplication.sharedApplication().registerUserNotificationSettings(settings)
UIApplication.shared().registerForRemoteNotifications()
```

If you are using iOS 10 or greater with the `UserNotification` framework, add the following:

**Objective\-C**

```
[UNUserNotificationCenter currentNotificationCenter]
 requestAuthorizationWithOptions:(UNAuthorizationOptionAlert + UNAuthorizationOptionSound)
   completionHandler:^(BOOL granted, NSError * _Nullable error) {
      // Enable or disable features based on authorization.
}];
[[UIApplication sharedApplication] registerForRemoteNotifications];
```

**Swift**

```
UNUserNotificationCenter.current().requestAuthorization(options:[.badge, .alert, .sound]) { (granted, error) in
            // Enable or disable features based on authorization.
        }
UIApplication.shared().registerForRemoteNotifications()
```

## Handle Notification Callbacks<a name="mobile-sdk-ios-modify-callbacks"></a>

To enable Amazon Pinpoint to report the notifications that are opened for a campaign, you must intercept some notification callbacks\.

In the `application:didRegisterForRemoteNotificationsWithDeviceToken:` method in the `ApplicationDelegate` for your app, call the interceptor:

**Objective\-C**

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
	[_pinpoint.notificationManager interceptDidRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
```

**Swift**

```
func application(application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
	pinpoint!.notificationManager.interceptDidRegisterForRemoteNotificationsWithDeviceToken(deviceToken)
```

In the `application:didReceiveNotification:fetchCompletionHandler:` method in the `ApplicationDelegate` for your app, call the interceptor:

**Objective\-C**

```
- (void) application:(UIApplication *)application didReceiveRemoteNotification:(nonnull NSDictionary *)userInfo fetchCompletionHandler:(nonnull void (^)(UIBackgroundFetchResult))completionHandler {
	[_pinpoint.notificationManager interceptDidReceiveRemoteNotification:userInfofetchCompletionHandler:completionHandler];
	completionHandler(UIBackgroundFetchResultNewData);
```

**Swift**

```
func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject], fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void) {
	pinpoint!.notificationManager.interceptDidReceiveRemoteNotification(userInfo, fetchCompletionHandler: completionHandler)
```

If you are using iOS 10 or greater with the `UserNotification` framework, add the following:

**Objective\-C**

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void (^)(void))completionHandler {
	[[[AWSMobileClient sharedInstance] pinpoint].notificationManager interceptDidReceiveRemoteNotification:response.notification.request.content.userInfo fetchCompletionHandler:^(UIBackgroundFetchResult result) {}];
```

**Swift**

```
@available(iOS 10.0, *)
func userNotificationCenter(center: UNUserNotificationCenter, didReceiveNotificationResponse response: UNNotificationResponse, withCompletionHandler completionHandler: () -> Void) {
	pinpoint!.notificationManager.interceptDidReceiveRemoteNotification(response.notification.request.content.userInfo) { (UIBackgroundFetchResult) in}
```
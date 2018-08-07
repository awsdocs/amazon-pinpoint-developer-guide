# Handling Push Notifications from Baidu Cloud Push<a name="mobile-sdk-android-push-baidu"></a>

Baidu Cloud Push is the push notification service provided by Baidu, a Chinese cloud service\. By integrating Baidu Cloud Push with your mobile app, you can use Amazon Pinpoint to send notifications to your app through the Baidu mobile push channel\.

**Prerequisites**  
To send push notifications to mobile devices using Amazon Pinpoint and Baidu, you need the following:  
Baidu account\.
Registration as a Baidu developer\.
Baidu Cloud Push project\.
API key and secret key from a Baidu Cloud Push project\.
Baidu user ID and channel ID\.
You must also integrate the AWS Mobile SDK for Android before you begin\. For more information, see [Integrating the AWS Mobile SDKs or JavaScript Library with Your Application](integrate-sdk.md)\.

## Integrating Baidu Cloud Push with Your App<a name="mobile-sdk-android-push-baidu-integrate"></a>

The following procedure is based on version 5\.7\.1\.65 of the Baidu push service jar\.

**To integrate Baidu with your app**

1. Download the latest Baidu Cloud Push SDK Android client from [http://push\.baidu\.com/](http://push.baidu.com/)\.

1. Extract the zip file and import the `pushservice-x.x.xx.jar` file from the Baidu\-Push\-SDK\-Android `libs` folder into your Android app’s `lib` folder\.

1. The Baidu\-Push\-SDK\-Android `libs` folder should also include the following folders:
   + `arm64-v8a/`
   + `armeabi/`
   + `armeabi-v7a/`
   + `mips/`
   + `mips64/`
   + `x86/`
   + `x86_64/`

   Add each complete folder to your Android app’s `src/main/jniLibs` folder\.

1. In the Android app’s `AndroidManifest.xml` file, declare the following permissions:

   ```
   <uses-permission android:name="android.permission.INTERNET" />
   <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
   
   <!-- Baidu permissions -->
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   
   <uses-permission android:name="android.permission.READ_PHONE_STATE" />
   <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
   <uses-permission android:name="android.permission.WRITE_SETTINGS" />
   <uses-permission android:name="android.permission.VIBRATE" />
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
   <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
   <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
   ```

1. Under `<application>`, specify the following receivers and intent filters: 

   ```
   <!-- Baidu settings -->
   <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
             android:process=":bdservice_v1">
     <intent-filter>
       <action android:name="android.intent.action.BOOT_COMPLETED" />
       <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
       <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
       <action android:name="com.baidu.android.pushservice.action.media.CLICK" />
       <!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
       <action android:name="android.intent.action.MEDIA_MOUNTED" />
       <action android:name="android.intent.action.USER_PRESENT" />
       <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
       <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
     </intent-filter>
   </receiver>
   <receiver
             android:name="com.baidu.android.pushservice.RegistrationReceiver"
             android:process=":bdservice_v1">
     <intent-filter>
       <action android:name="com.baidu.android.pushservice.action.METHOD" />
       <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
     </intent-filter>
     <intent-filter>
       <action android:name="android.intent.action.PACKAGE_REMOVED" />
   
       <data android:scheme="package" />
     </intent-filter>
   </receiver>
   
   <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1">
     <intent-filter>
       <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
     </intent-filter>
   </service>
   <service
            android:name="com.baidu.android.pushservice.CommandService"
            android:exported="true" />
   <!-- Amazon Pinpoint Notification Receiver -->
   <receiver android:name="com.amazonaws.mobileconnectors.pinpoint.targeting.notification.PinpointNotificationReceiver">
     <intent-filter>
     <action android:name="com.amazonaws.intent.baidu.NOTIFICATION_OPEN" />
     </intent-filter>
   </receiver>
   ```

1. Update the `AndroidManifest.xml` file with the following permissions, which are specific to your application\. Remember to replace *YourPackageName* with the name of your package\.

   ```
   <!-- 适配Android N系统必需的ContentProvider写权限声明，写权限包含应用包名-->
   <uses-permission android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName" />
   <permission
               android:name="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName"
               android:protectionLevel="normal"></permission>
    <!-- 适配Android N系统必需的ContentProvider声明，写权限包含应用包名-->
   <provider
             android:name="com.baidu.android.pushservice.PushInfoProvider"
             android:authorities="YourPackageName.bdpush"
             android:exported="true"
             android:protectionLevel="signature"
             android:writePermission="baidu.push.permission.WRITE_PUSHINFOPROVIDER.YourPackageName" />
   ```

1. Inside your Android application, create a `MessageReceiver` class that subclasses `com.baidu.android.pushservice.PushMessageReceiver`\. The subclass should implement the following methods and perform the corresponding calls:  
`onBind`  
Called when the device is registered with Baidu Cloud Push\. Provides the Baidu user ID and channel ID that are needed to register the device with Amazon Pinpoint\. Include the following call as part of this method:  

   ```
   pinpointManager.getNotificationClient().registerDeviceToken(registrationId)
   ```  
`onUnbind`  
Called when the device is no longer registered with Baidu Cloud Push\.  
`onMessage`  
Called when the device receives a raw message from Baidu Cloud Push\. Amazon Pinpoint transmits campaign push notifications with the Baidu Cloud Push raw message format\. Include the following call as part of this method:  

   ```
   NotificaitonDetails details = NotificationDetailsBuilder.builder()
                                 .message(message);
                                 .intentAction(NotificationClient.BAIDU_INTENT_ACTION)
                                 .build();
   
   pinpointManager.getNotificationClient().handleCampaignPush(details)
   ```

   Only the message parameter contains data\. The `customContentString` is not used with raw messages\.

1. After creating the subclass, modify the `AndoriodManifest.xml` file to register it as a receiver\. In the following example, the `PushMessageReceiver` subclass is named `com.baidu.push.example.MyPushMessageReceiver`\.

   ```
   <!-- push应用定义消息receiver声明 -->
   <receiver android:name="com.baidu.push.example.MyPushMessageReceiver">
     <intent-filter>
       <!-- 接收push消息 -->
       <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
       <!-- 接收bind,unbind,fetch,delete等反馈消息 -->
       <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
       <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
     </intent-filter>
   </receiver>
   ```

1. To start the Baidu listener service, in your Android app’s main activity, add the following code to the `onCreate` method:

   ```
   // Push: 以apikey的方式登录，一般放在主Activity的onCreate中。
   // ！！ ATTENTION：You need to modify the value of api_key to your own !!
   // 启动百度push
   PushManager.startWork(getApplicationContext(), PushConstants.LOGIN_TYPE_API_KEY, api_key);
   
   // Push: 设置自定义的通知样式，具体API介绍见用户手册，如果想使用系统默认的可以不加这段代码
   // 请在通知推送界面中，高级设置->通知栏样式->自定义样式，选中并且填写值：1，
   // 与下方代码中 PushManager.setNotificationBuilder(this, 1, cBuilder)中的第二个参数对应
   CustomPushNotificationBuilder cBuilder = new CustomPushNotificationBuilder(
     getResources().getIdentifier("notification_custom_builder", "layout", getPackageName()),
     getResources().getIdentifier("notification_icon", "id", getPackageName()),
     getResources().getIdentifier("notification_title", "id", getPackageName()),
     getResources().getIdentifier("notification_text", "id", getPackageName()));
   cBuilder.setNotificationFlags(Notification.FLAG_AUTO_CANCEL);
   cBuilder.setNotificationDefaults(Notification.DEFAULT_VIBRATE);
   
   cBuilder.setStatusbarIcon(this.getApplicationInfo().icon);
   cBuilder.setLayoutDrawable(getResources().getIdentifier(
     "simple_notification_icon", "drawable", getPackageName()));
   cBuilder.setNotificationSound(Uri.withAppendedPath(
     Audio.Media.INTERNAL_CONTENT_URI, "6").toString());
   // 推送高级设置，通知栏样式设置为下面的ID
   PushManager.setNotificationBuilder(this, 1, cBuilder);
   ```

1. Remember to properly initialize your `PinpointManager` reference\. Use a `PinpointConfiguration` with a `ChannelType` value of `ChannelType.BAIDU`\. You can do this programmatically, as in the following example:

   ```
   final PinpointConfiguration config =
     new PinpointConfiguration(this,
                               IdentityManager.getDefaultIdentityManager()
                               .getCredentialsProvider(),
                               awsConfiguration)
     .withChannelType(ChannelType.BAIDU);
   Application.pinpointManager = new PinpointManager(config);
   ```

   Or, you can define a configuration file to be consumed by `AWSConfiguration`:

   ```
   "PinpointAnalytics": {
       "Default": {
         "AppId": "[YourPinpointAppId]",
         "Region": "us-east-1",
         "ChannelType": "BAIDU"
       }
     }
   ```

## Testing Baidu Push Notifications<a name="mobile-sdk-android-push-baidu-test"></a>

To test, you need an Amazon Pinpoint project, a Baidu API key, and a Baidu Secret key\.

Before you begin, augment your app to display the device token after registration\. The device token can be retrieved by calling:

```
PinpointManager::getNotificationClient().getDeviceToken()
```

Complete the following steps using the AWS CLI\.

**To test Baidu push notifications**

1. Register Baidu as a channel with your Amazon Pinpoint project\. Provide the Baidu API key and Secret key\.

   ```
   aws pinpoint update-baidu-channel --application-id YourPinpointAppId --baidu-channel-request "{                                                                                                 
       \"ApiKey\": \"BAIDU_API_KEY\",                                                                
       \"Enabled\": true,                                                                             
       \"SecretKey\": \"BAIDU_SECRET_KEY\"                                                           
   }"
   ```

1. Install your app on to a Baidu\-enabled device and capture the generated device token\.

1. Send a direct message to the device specifying the device token as the address\.

   ```
   aws pinpoint send-messages --application-id YourPinpointAppId --message-request "{
     \"Addresses\": {
         \"DeviceToken\": {
             \"ChannelType\": \"BAIDU\"
         } 
     },
     \"MessageConfiguration\": {
         \"BaiduMessage\": {
             \"RawContent\":\"{'pinpoint.campaign.campaign_id':'_DIRECT','pinpoint.notification.silentPush':0,'pinpoint.openApp':true,'pinpoint.notification.title':'Hello','pinpoint.notification.body':'Hello World.'}\"
         }
     }
   }"
   ```
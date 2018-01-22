# Handling Push Notifications from Firebase Cloud Messaging or Google Cloud Messaging<a name="mobile-sdk-android-push-fcm"></a>

Modify your app code to handle push notifications from Firebase Cloud Messaging \(FCM\) or its predecessor, Google Cloud Messaging \(GCM\)\.

## Set up the Manifest File<a name="mobile-sdk-android-modify-manifest"></a>

If you are using FCM, add the following entries to your `AndroidManifest.xml` file before the `<application>` tag\.

```
<receiver
    android:name="com.amazonaws.mobileconnectors.pinpoint.targeting.notification.PinpointNotificationReceiver"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.amazonaws.intent.fcm.NOTIFICATION_OPEN" />
    </intent-filter>
</receiver>
```

If you are using GCM, you must provide permissions for your app to register, receive, and respond to GCM notifications\. Add the following entries to your `AndroidManifest.xml` file before the `<application>` tag\.

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
<permission android:name="PACKAGE_NAME.permission.C2D_MESSAGE"
    android:protectionLevel="signature" />
<uses-permission android:name="PACKAGE_NAME.permission.C2D_MESSAGE" />
```

Install a listener for push notification messages from Google servers\. Add the following entries to your `AndroidManifest.xml` file inside the `<application>` tag: 

```
<receiver
    android:name="com.google.android.gms.gcm.GcmReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <category android:name="@string/google_cloud_messaging_package" />
    </intent-filter>
</receiver>
```

Register a service that extends the Google `GcmListenerService` to listen for push notifications\. Here is an example of such a service called `PushListenerService`\. 

```
<service
    android:name="PACKAGE_NAME.PushListenerService"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    </intent-filter>
</service>
```

## Register the Token<a name="mobile-sdk-android-modify-token"></a>

If you are using GCM, register the GCM token with the Amazon Pinpoint client\.

In the following example, the `AWSMobileClient` class is provided in the AWS Mobile Hub sample code to reference the Amazon Pinpoint object\.

```
InstanceID instanceID = InstanceID.getInstance(this);
String gcmToken =
    instanceID.getToken(
        getString(R.string.gcm_defaultSenderId),
        GoogleCloudMessaging.INSTANCE_ID_SCOPE,
        null);
AWSMobileClient.defaultMobileClient().getPinpointManager().getNotificationClient().registerGCMDeviceToken(refreshedToken) ;
```

If you are using FCM, call this object inside your class that extends `FirebaseInstanceIdService`:

```
/**
 * Called if InstanceID token is updated. This may occur if the security of
 * the previous token has been compromised. Note that this is called when the InstanceID token
 * is initially generated so this is where you would retrieve the token.
 */
// [START refresh_token]
@Override
public void onTokenRefresh() {
    // Get updated InstanceID token.
    String refreshedToken = FirebaseInstanceId.getInstance().getToken();
    Log.d(TAG, "Refreshed token: " + refreshedToken);

    // If you want to send messages to this application instance or
    // manage this apps subscriptions on the server side, send the
    // Instance ID token to your app server.

    AWSMobileClient.defaultMobileClient().getPinpointManager().getNotificationClient().registerGCMDeviceToken(refreshedToken);
}
// [END refresh_token]
```

## Handling the Message<a name="mobile-sdk-android-modify-handling"></a>

You must add a hook to handle the message\. 

If you are using GCM, add a hook in the class in your app that extends `GcmListenerService` in the `onMessageReceived` method:

```
@Override
public void onMessageReceived(final String from, final Bundle data) {
    AWSMobileClient.initializeMobileClientIfNecessary(this.getApplicationContext());
    final NotificationClient notificationClient = AWSMobileClient.defaultMobileClient()
        .getPinpointManager().getNotificationClient();

    NotificationClient.CampaignPushResult pushResult =
        notificationClient.handleGCMCampaignPush(from, data, this.getClass());
```

If you are using FCM, add a hook in the class in your app that extends `FirebaseMessagingService` in the `onMessageReceived` method:

```
/**
     * Called when message is received.
     *
     * @param remoteMessage Object representing the message received from Firebase Cloud Messaging.
     */
    // [START receive_message]
    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {

AWSMobileClient.defaultMobileClient().getPinpointManager().getNotificationClient().handleFCMCampaignPush(remoteMessage.getFrom(), remoteMessage.getData());
    } 
    // [END receive_message]
```
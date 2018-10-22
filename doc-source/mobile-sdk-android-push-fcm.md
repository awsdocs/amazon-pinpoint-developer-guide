# Handling Push Notifications from Firebase Cloud Messaging or Google Cloud Messaging<a name="mobile-sdk-android-push-fcm"></a>

You can enable your Android or iOS app to receive push notifications that you send through by using Amazon Pinpoint\. With Amazon Pinpoint, you can send push notifications through Firebase Cloud Messaging \(FCM\) or its predecessor, Google Cloud Messaging \(GCM\)\.

**Prerequisite**  
Before you update your app to receive push notifications, integrate the AWS Mobile SDK for Android\. For more information, see [Integrating the AWS Mobile SDKs for Android or iOS](integrate-sdk.md#integrate-sdk-mobile)\.

## Enabling Push Notifications<a name="mobile-sdk-android-push-fcm-enable"></a>

To enable push notifications in your app, update your app to include push listening code\. For more information, see [Add Push Notifications to Your Mobile App with Amazon Pinpoint](https://docs.aws.amazon.com/aws-mobile/latest/developerguide/add-aws-mobile-push-notifications.html) in the *AWS Mobile Developer Guide*\.

## Setting up Deep Linking<a name="mobile-sdk-android-deep-linking"></a>

Amazon Pinpoint campaigns can take one of three actions when a user taps a notification\. One of those possible actions is a deep link, which opens the app to a specified activity\.

To specify a destination activity for deep links, the app must have set up deep linking\. This setup requires an intent filter that registers a URL scheme the deep links will use\. After the app creates an intent filter, the data provided by the intent determines the activity to render\. 

### Creating an Intent Filter<a name="mobile-sdk-android-deep-linking-filter"></a>

Begin to set up deep linking by creating an intent filter in your `AndroidManifest.xml` file\. For example:

```
<!-- This activity allows your application to receive a deep link that navigates directly to the 
"Deeplink Page"-->
<activity
    android:name=".DeepLinkActivity"
    android:label="A deeplink!" >
    <intent-filter android:label="inAppReceiver">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- Accepts URIs of type "pinpoint://deeplink" -->
        <data android:scheme="pinpoint"
            android:host="deeplink" />
    </intent-filter>
</activity>
```

The data element in the previous example registers a URL scheme, `pinpoint://`, as well as the host, `deeplink`\. As a result, when given a URL in the form of `pinpoint://deeplink`, the manifest is prepared to execute the action\. 

### Handling the Intent<a name="mobile-sdk-android-deep-linking-intent"></a>

Next, set up an intent handler to present the screen associated with the registered URL scheme and host\. Intent data is retrieved in the `onCreate()` method, which then can use `Uri data` to create an activity\. The following example shows an alert and tracks an event\. 

```
public class DeeplinkActivity extends Activity {
 
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        if (getIntent().getAction() == Intent.ACTION_VIEW) {
            Uri data = getIntent().getData();
 
            if (data != null) {
 
                // show an alert with the "custom" param
                new AlertDialog.Builder(this)
                        .setTitle("An example of a Deeplink")
                        .setMessage("Found custom param: " +data.getQueryParameter("custom"))
                        .setPositiveButton(android.R.string.yes, new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog, int which) {
                                dialog.dismiss();
                            }
                        })
                        .setIcon(android.R.drawable.ic_dialog_alert)
                        .show();
            }
        }
    }
}
```
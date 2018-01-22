# Managing Sessions<a name="mobile-sdk-android-sessions"></a>

As users engage with your app, it reports information about app sessions to Amazon Pinpoint, such as session start times, session end times, and events that occur during sessions\. To report this information, your app must include methods that handle events as your app enters the foreground and the background on the user's Android device\.

When you use AWS Mobile Hub to create a project for your Amazon Pinpoint app, Mobile Hub provides a sample app that demonstrates how to integrate with Amazon Pinpoint\. The Android version of the sample app includes the `AbstractApplicationLifeCycleHelper` class to help you manage app sessions\. Include this class in your Android app package\.

For more information about creating the Mobile Hub sample app, see [Getting Started With Android Apps](getting-started-android.md)\.

After you include the `AbstractApplicationLifeCycleHelper` class, implement the abstract methods, `applicationEnteredForeground` and `applicationEnteredBackground`, in the `Application` file in your app package\. These methods enable your app to report the following information to Amazon Pinpoint:

+ Session start times \(when the app enters the foreground\)\.

+ Session end times \(when the app enters the background\)\.

+ The events that occur during the app session, such as monetization events\. This information is reported when the app enters the background\.

The following example shows how to implement the `applicationEnteredForeground` and `applicationEnteredBackground` abstract methods:

```
applicationLifeCycleHelper = new AbstractApplicationLifeCycleHelper(this) {
    @Override
    protected void applicationEnteredForeground() {
        final PinpointManager pinpointManager = AWSMobileClient.defaultMobileClient()
            .getPinpointManager();
        pinpointManager.getSessionClient().startSession();
        // Handles events that occur when your app enters the foreground.
    }

    @Override
    protected void applicationEnteredBackground() {
        Log.d(LOG_TAG, "Detected application has entered the background.");
        final PinpointManager pinpointManager = AWSMobileClient.defaultMobileClient()
            .getPinpointManager();
        pinpointManager.getSessionClient().stopSession();
        pinpointManager.getAnalyticsClient().submitEvents();
        // Handles events that occur when your app enters the background.
    }
};
```
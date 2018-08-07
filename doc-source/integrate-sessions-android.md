# Managing Sessions in Your Application \(Android Only\)<a name="integrate-sessions-android"></a>

As users engage with your app, it reports information about app sessions to Amazon Pinpoint, such as session start times, session end times, and events that occur during sessions\. To report this information from an Android application, your app must include methods that handle events as your app enters the foreground and the background on the user's Android device\. \(For iOS applications, the AWS Mobile SDK for iOS automatically reports session information\.\)

## Before You Begin<a name="integrate-sessions-android-before"></a>

If you haven't already, do the following:
+ Integrate the AWS Mobile SDK for Android with your app\. See [Integrating the AWS Mobile SDKs for Android or iOS](integrate-sdk.md#integrate-sdk-mobile)\.
+ Update your application to register endpoints\. See [Registering Endpoints in Your Application](integrate-endpoints.md)\.

## Example Lifecycle Manager<a name="integrate-sessions-android-example-lifecycle-manager"></a>

The following example class, `AbstractApplicationLifeCycleHelper`, implements the `Application.ActivityLifecycleCallbacks` interface to track when the application enters the foreground or background, among other states\. Add this class to your app, or use it as an example for how to update your code:

```
package com.amazonaws.mobile.util;

import android.app.Activity;
import android.app.Application;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.Bundle;
import android.util.Log;

import java.util.WeakHashMap;

/**
 * Aids in determining when your application has entered or left the foreground.
 * The constructor registers to receive Activity lifecycle events and also registers a
 * broadcast receiver to handle the screen being turned off.  Abstract methods are
 * provided to handle when the application enters the background or foreground.
 * Any activity lifecycle callbacks can easily be overriden if additional handling
 * is needed. Just be sure to call through to the super method so that this class
 * will still behave as intended.
 **/
public abstract class AbstractApplicationLifeCycleHelper implements Application.ActivityLifecycleCallbacks {
    private static final String LOG_TAG = AbstractApplicationLifeCycleHelper.class.getSimpleName();
    private static final String ACTION_SCREEN_OFF = "android.intent.action.SCREEN_OFF";
    private boolean inForeground = false;
    /** Tracks the lifecycle of activities that have not stopped (including those restarted). */
    private WeakHashMap<Activity, String> activityLifecycleStateMap = new WeakHashMap<>();

    /**
     * Constructor. Registers to receive activity lifecycle events.
     * @param application The Android Application class.
     */
    public AbstractApplicationLifeCycleHelper(final Application application) {
        application.registerActivityLifecycleCallbacks(this);
        final ScreenOffReceiver screenOffReceiver = new ScreenOffReceiver();
        application.registerReceiver(screenOffReceiver, new IntentFilter(ACTION_SCREEN_OFF));
    }

    @Override
    public void onActivityCreated(final Activity activity, final Bundle bundle) {
        Log.d(LOG_TAG, "onActivityCreated " + activity.getLocalClassName());
        handleOnCreateOrOnStartToHandleApplicationEnteredForeground();
        activityLifecycleStateMap.put(activity, "created");
    }

    @Override
    public void onActivityStarted(final Activity activity) {
        Log.d(LOG_TAG, "onActivityStarted " + activity.getLocalClassName());
        handleOnCreateOrOnStartToHandleApplicationEnteredForeground();
        activityLifecycleStateMap.put(activity, "started");
    }

    @Override
    public void onActivityResumed(final Activity activity) {
        Log.d(LOG_TAG, "onActivityResumed " + activity.getLocalClassName());
        activityLifecycleStateMap.put(activity, "resumed");
    }

    @Override
    public void onActivityPaused(final Activity activity) {
        Log.d(LOG_TAG, "onActivityPaused " + activity.getLocalClassName());
        activityLifecycleStateMap.put(activity, "paused");
    }

    @Override
    public void onActivityStopped(final Activity activity) {
        Log.d(LOG_TAG, "onActivityStopped " + activity.getLocalClassName());
        // When the activity is stopped, we remove it from the lifecycle state map since we
        // no longer consider it keeping a session alive.
        activityLifecycleStateMap.remove(activity);
    }

    @Override
    public void onActivitySaveInstanceState(final Activity activity, final Bundle outState) {
        Log.d(LOG_TAG, "onActivitySaveInstanceState " + activity.getLocalClassName());
    }

    @Override
    public void onActivityDestroyed(final Activity activity) {
        Log.d(LOG_TAG, "onActivityDestroyed " + activity.getLocalClassName());
        // Activity should not be in the activityLifecycleStateMap any longer.
        if (activityLifecycleStateMap.containsKey(activity)) {
            Log.wtf(LOG_TAG, "Destroyed activity present in activityLifecycleMap!?");
            activityLifecycleStateMap.remove(activity);
        }
    }

    /**
     * Call this method when your Application trims memory.
     * @param level the level passed through from Application.onTrimMemory().
     */
    public void handleOnTrimMemory(final int level) {
        Log.d(LOG_TAG, "onTrimMemory " + level);
        // If no activities are running and the app has gone into the background.
        if (level >= Application.TRIM_MEMORY_UI_HIDDEN) {
            checkForApplicationEnteredBackground();
        }
    }

    class ScreenOffReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            checkForApplicationEnteredBackground();
        }
    }

    /**
     * Called back when your application enters the Foreground.
     */
    protected abstract void applicationEnteredForeground();

    /**
     * Called back when your application enters the Background.
     */
    protected abstract void applicationEnteredBackground();

    /**
     * Called from onActivityCreated and onActivityStarted to handle when the application enters
     * the foreground.
     */
    private void handleOnCreateOrOnStartToHandleApplicationEnteredForeground() {
        // if nothing is in the activity lifecycle map indicating that we are likely in the background, and the flag
        // indicates we are indeed in the background.
        if (activityLifecycleStateMap.size() == 0 && !inForeground) {
            inForeground = true;
            // Since this is called when an activity has started, we now know the app has entered the foreground.
            applicationEnteredForeground();
        }
    }

    private void checkForApplicationEnteredBackground() {
        ThreadUtils.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                // If the App is in the foreground and there are no longer any activities that have not been stopped.
                if ((activityLifecycleStateMap.size() == 0) && inForeground) {
                    inForeground = false;
                    applicationEnteredBackground();
                }
            }
        });
    }
}
```

## Reporting Session Events<a name="integrate-sessions-android-report"></a>

After you include the `AbstractApplicationLifeCycleHelper` class, implement the two abstract methods, `applicationEnteredForeground` and `applicationEnteredBackground`, in the `Application` class\. These methods enable your app to report the following information to Amazon Pinpoint:
+ Session start times \(when the app enters the foreground\)\.
+ Session end times \(when the app enters the background\)\.
+ The events that occur during the app session, such as monetization events\. This information is reported when the app enters the background\.

The following example shows how to implement `applicationEnteredForeground` and `applicationEnteredBackground`\. It also shows how to call `handleOnTrimMemory` from inside the `onTrimMemory` function of the `Application` class:

```
import com.amazonaws.mobileconnectors.pinpoint.PinpointConfiguration;
import com.amazonaws.mobileconnectors.pinpoint.PinpointManager;

public class Application extends MultiDexApplication {
    public static PinpointManager pinpointManager;
    private AbstractApplicationLifeCycleHelper applicationLifeCycleHelper;

    @Override
    public void onCreate() {
        super.onCreate();

        // . . .
           
        // The Helper registers itself to receive application lifecycle events when it is constructed.
        // A reference is kept here in order to pass through the onTrimMemory() call from
        // the Application class to properly track when the application enters the background.
        applicationLifeCycleHelper = new AbstractApplicationLifeCycleHelper(this) {
            @Override
            protected void applicationEnteredForeground() {
                Application.pinpointManager.getSessionClient().startSession();
                // handle any events that should occur when your app has come to the foreground...
            }

            @Override
            protected void applicationEnteredBackground() {
                Log.d(LOG_TAG, "Detected application has entered the background.");
                Application.pinpointManager.getSessionClient().stopSession();
                Application.pinpointManager.getAnalyticsClient().submitEvents();
                // handle any events that should occur when your app has gone into the background...
            }
        };
    }

    private void updateGCMToken() {
        try {
            final String gcmToken = InstanceID.getInstance(this).getToken(
                    "YOUR_SENDER_ID",
                    GoogleCloudMessaging.INSTANCE_ID_SCOPE
            );
            Application.pinpointManager.getNotificationClient().registerGCMDeviceToken(gcmToken);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void onTrimMemory(final int level) {
        Log.d(LOG_TAG, "onTrimMemory " + level);
        applicationLifeCycleHelper.handleOnTrimMemory(level);
        super.onTrimMemory(level);
    }

}
```

## Next Step<a name="integrate-sessions-android-next"></a>

You've updated your Android app to report session information\. Now, when users open and close your app, you can see session metrics in the Amazon Pinpoint console, including those shown by the **Sessions** and **Session heat map** charts\.

Next, update your app to report usage data\. See [Reporting Events in Your Application](integrate-events.md)\.
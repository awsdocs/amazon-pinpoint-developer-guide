# Initializing the Amazon Pinpoint Client<a name="mobile-sdk-android-modify"></a>

After you have included AWS Mobile SDK for Android support in your app, which enables the app to call AWS services, modify the app code to initialize the Amazon Pinpoint client\.

Add the following import to your main activity:

```
import com.amazonaws.mobileconnectors.pinpoint.*;
```

Depending on how your app authenticates calls, you may also need to add the following import:

```
import com.amazonaws.regions.Regions;
```

**Important**  
For simplicity, the code in these examples initializes the Amazon Pinpoint client in the `onCreate` method\. This approach can take several seconds to execute\. In production code, this initialization is best done in a background thread\. For more information about how to do this, see the Google Developer website\.

**To initialize the Amazon Pinpoint client using the default initializer**

+ Initialize the Amazon Pinpoint client as follows:

```
CognitoCachingCredentialsProvider cognitoCachingCredentialsProvider = new CognitoCachingCredentialsProvider(context,"IDENTITY_POOL_ID",Regions.US_EAST_1);

PinpointConfiguration config = new PinpointConfiguration(context, "APP_ID", Regions.US_EAST_1, cognitoCachingCredentialsProvider);

this.pinpointManager = new PinpointManager(config);
```
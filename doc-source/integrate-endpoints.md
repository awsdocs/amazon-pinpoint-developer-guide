# Registering Endpoints in Your Application<a name="integrate-endpoints"></a>

When a user starts a session \(for example, by launching your mobile app\), your mobile or web application can automatically register \(or update\) an *endpoint* with Amazon Pinpoint\. The endpoint represents the device that the user starts the session with\. It includes attributes that describe the device, and it can also include custom attributes that you define\. Endpoints can also represent other methods of communicating with customers, such as email addresses or mobile phone numbers\. 

After your application registers endpoints, you can segment your audience based on endpoint attributes\. You can then engage these segments with tailored messaging campaigns\. You can also use the **Analytics** page in the Amazon Pinpoint console to view charts about endpoint registration and activity, such as **New endpoints** and **Daily active endpoints**\.

You can assign a single user ID to multiple endpoints\. A user ID represents a single user, while each endpoint that is assigned the user ID represents one of the user's devices\. After you assign user IDs to your endpoints, you can view charts about user activity in the console, such as **Daily active users** and **Monthly active users**\. 

## Before You Begin<a name="integrate-endpoints-before"></a>

If you haven't already, integrate the AWS Mobile SDK for Android or iOS, or integrate the AWS Amplify JavaScript library with your application\. See [Integrating the AWS Mobile SDKs or JavaScript Library with Your Application](integrate-sdk.md)\.

## Registering Endpoints with the AWS Mobile SDKs for Android or iOS<a name="integrate-endpoints-mobile"></a>

Use the AWS Mobile SDKs for Android or iOS to register and customize endpoints\.

### Initializing the Amazon Pinpoint Client<a name="integrate-endpoints-mobile-initialize"></a>

To update your application to register endpoints, initialize the Amazon Pinpoint client that's provided by the AWS Mobile SDKs\.

------
#### [ Android \- Java ]

1. Add the Amazon Pinpoint library to the `app/build.gradle` file:

   ```
   dependencies{
      compile 'com.amazonaws:aws-android-sdk-pinpoint:2.6.+'
   }
   ```

1. Add the following import statements to classes that use the Amazon Pinpoint client:

   ```
   import com.amazonaws.mobileconnectors.pinpoint.PinpointConfiguration;
   import com.amazonaws.mobileconnectors.pinpoint.PinpointManager;
   ```

1. Create an instance of the `PinpointManager` class\. The following example defines a global variable with a type of `PinpointManager`\. The variable is initialized in the `onCreate()` method of the `Application` class:

   ```
   public class Application extends MultiDexApplication {
       public static PinpointManager pinpointManager;
   
       @Override
       public void onCreate() {
           super.onCreate();
           awsConfiguration = new AWSConfiguration(this);
   
           if (IdentityManager.getDefaultIdentityManager() == null) {
               final IdentityManager identityManager = new IdentityManager(getApplicationContext(), awsConfiguration);
               IdentityManager.setDefaultIdentityManager(identityManager);
           }
   
           try {
               final PinpointConfiguration config =
                       new PinpointConfiguration(this,
                               IdentityManager.getDefaultIdentityManager().getCredentialsProvider(),
                               awsConfiguration);
               Application.pinpointManager = new PinpointManager(config);
           } catch (final AmazonClientException ex) {
               Log.e(LOG_TAG, "Unable to initialize PinpointManager. " + ex.getMessage(), ex);
           }
   
   }
   ```

------
#### [ iOS \- Swift ]

1. Include the `AWSPinpoint` pod in the `Podfile` that you configure to install the AWS Mobile SDK:

   ```
   platform :ios, '9.0'
   target :'YourAppName' do
   	use_frameworks!
   
   	pod 'AWSPinpoint', '~> 2.6.6'
   
   	# other pods
   
   end
   ```

1. Add the following import statements to classes that use the Amazon Pinpoint client:

   ```
   import AWSCore
   import AWSPinpoint
   ```

1. Create an instance of the `AWSPinpoint` class\. The following example defines a global variable with a type of `AWSPinpoint?`\. The variable is initialized in the `application(_:didFinishLaunchingWithOptions:)` method of the `AppDelegate` class:

   ```
   class AppDelegate: UIResponder, UIApplicationDelegate {
   
       var pinpoint: AWSPinpoint?
   
       func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions:
       [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
   
       // . . .
   
       // Initialize the Amazon Pinpoint client
       pinpoint = AWSPinpoint(configuration:
               AWSPinpointConfiguration.defaultPinpointConfiguration(launchOptions: launchOptions))
   
       // . . .
       }
   }
   ```

------
#### [ iOS \- Objective\-C ]

1. Include the `AWSPinpoint` pod in the `Podfile` that you configure to install the AWS Mobile SDK:

   ```
   platform :ios, '9.0'
   target :'YourAppName' do
   	use_frameworks!
   
   	pod 'AWSPinpoint', '~> 2.6.6'
   
   	# other pods
   
   end
   ```

1. Add the following import statements to classes that use the Amazon Pinpoint client:

   ```
   #import <AWSCore/AWSCore.h>
   #import <AWSPinpoint/AWSPinpoint.h>
   ```

1. Create an instance of the `AWSPinpoint` class\. The following example defines a global variable with a type of `AWSPinpoint`\. The variable is initialized in the `application:didFinishLaunchingWithOptions:` method of the `AppDelegate` class:

   ```
   @interface AppDelegate : UIResponder <UIApplicationDelegate>
   
   @property(atomic) AWSPinpoint *pinpoint;
   
   @end
   
   @implementation AppDelegate
   
   - (BOOL)application:(UIApplication *)application 
   didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
   
       // . . .
   
       // Initialize the Amazon Pinpoint client
       AWSPinpointConfiguration *config = 
       	[AWSPinpointConfiguration defaultPinpointConfigurationWithLaunchOptions:launchOptions];
       _pinpoint = [AWSPinpoint pinpointWithConfiguration:config];
       
       // . . .
       
       return YES;
   }
   
   @end
   ```

------

### Adding Custom Endpoint Attributes<a name="integrate-endpoints-mobile-custom-attributes"></a>

After you initialize the Amazon Pinpoint client in your application, you can add custom attributes to endpoints\.

------
#### [ Android \- Java ]

To add custom endpoint attributes in an Android application, use the `TargetingClient` class to store your custom attributes\. Then, call the `updateEndpointProfile()` method\. This method generates an endpoint that has the attributes that are stored in the client, and it updates the endpoint with Amazon Pinpoint:

```
final PinpointManager pinpoint = Application.pinpointManager;
final List<String> interestsList = Arrays.asList("science", "politics", "travel");
pinpoint.getTargetingClient().addAttribute("Interests", interestsList);
pinpoint.getTargetingClient().updateEndpointProfile();
```

Alternatively, you can create and modify an instance of the `EndpointProfile` class, and you can pass the endpoint object to the `updateEndpointProfile()` call:

```
final PinpointManager pinpoint = Application.pinpointManager;
final List<String> interestsLists = Arrays.asList("science", "politics", "travel");
EndpointProfile endpointProfile = pinpoint.getTargetingClient().currentEndpoint();
endpointProfile.addAttribute("Interests", interestsLists);
pinpoint.getTargetingClient().updateEndpointProfile(endpointProfile);
```

------
#### [ iOS \- Swift ]

To add custom endpoint attributes in Swift, use the `AWSPinpointTargetingClient` class\. The following example updates the endpoint in the `applicationDidEnterBackground(_:)` method\. The example adds a custom attribute called `interests`, and it assigns the values `science`, `politics`, and `travel`:

```
func applicationDidEnterBackground(_ application: UIApplication) {
    
    // . . .
    
    // Add a custom attribute to the endpoint
    if let targetingClient = pinpoint?.targetingClient {
        targetingClient.addAttribute(["science", "politics", "travel"], forKey: "interests")
        targetingClient.updateEndpointProfile()
        let endpointId = targetingClient.currentEndpointProfile().endpointId
        print("Updated custom attributes for endpoint: \(endpointId)")
    }
    
    // . . .
    
}
```

------
#### [ iOS \- Objective\-C ]

To add custom endpoint attributes in Objective\-C, use the `AWSPinpointTargetingClient` class\. The following example updates the targeting client in the `application:didFinishLaunchingWithOptions:` method\. The example adds a custom attribute called `interests`, and it assigns the values `science`, `politics`, and `travel`\. The example also adds a custom metric called `scienceInterestLevel` with a value of `100`:

```
- (BOOL)application:(UIApplication *)application 
didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    // . . .
    
    // Add a custom attribute to the endpoint
    AWSPinpointEndpointProfile *profile = [_pinpoint.targetingClient currentEndpointProfile];
    [profile addAttribute:@[@"science", @"politics", @"travel"] 
                   forKey:@"interests"];
    [profile addMetric:@100 forKey:@"scienceInterestLevel"];
    [[_pinpoint targetingClient] updateEndpointProfile: profile];

    // . . .
    
    return YES;
}
```

------

### Assigning User IDs to Endpoints<a name="integrate-endpoints-mobile-user-ids"></a>

Assign user IDs to endpoints by doing either of the following:
+ Manage user sign\-up and sign\-in with Amazon Cognito user pools\.
+ Use the Amazon Pinpoint client to assign user IDs without using Amazon Cognito user pools\.

Amazon Cognito user pools are user directories that make it easier to add sign\-up and sign\-in to your app\. When the AWS Mobile SDKs for iOS and Android register an endpoint with Amazon Pinpoint, Amazon Cognito automatically assigns a user ID from the user pool\. For more information, see [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-pinpoint-integration.html) in the *Amazon Cognito Developer Guide*\.

If you don't want to use Amazon Cognito user pools, you can use the Amazon Pinpoint client in your application to assign user IDs to endpoints\.

------
#### [ Android \- Java ]

To assign user IDs to endpoints in an Android application, create an instance of the `EndpointProfileUser` class, and call its `setUserId` method\. Then, assign this user object to the endpoint\. Finally, pass the updated endpoint to the `TargetingClient` of the `PinpointManager` class:

```
final PinpointManager pinpoint = Application.pinpointManager;
EndpointProfile endpointProfile = pinpoint.getTargetingClient().currentEndpoint();
EndpointProfileUser user = new EndpointProfileUser();
user.setUserId("UserIdValue");
endpointProfile.setUser(user);
pinpoint.getTargetingClient().updateEndpointProfile(endpointProfile);
```

------
#### [ iOS \- Swift ]

To assign user IDs to endpoints with Swift, use the `AWSPinpointTargetingClient` class\. First, create an `AWSPinpointEndpointProfileUser` object, and set its `userId` property\. Then, assign this user object to the endpoint\. Finally, pass the updated endpoint to the targeting client with the `update(_endpointProfile:)` method\. The following example assigns a user ID in the `application(_:didFinishLaunchingWithOptions:)` method:

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions
    launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    
    // . . .

    if let targetingClient = pinpoint?.targetingClient {
        let endpoint = targetingClient.currentEndpointProfile()
        // Create a user and set its userId property
        let user = AWSPinpointEndpointProfileUser()
        user.userId = "UserIdValue"
        // Assign the user to the endpoint
        endpoint.user = user
        // Update the endpoint with the targeting client
        targetingClient.update(endpoint)
        print("Assigned user ID \(user.userId ?? "nil") to endpoint \(endpoint.endpointId)")
    }

    // . . .
    
    return didFinishLaunching
}
```

------
#### [ iOS \- Objective\-C ]

To assign user IDs with Objective\-C, use the `AWSPinpointTargetingClient` class\. First, create an `AWSPinpointEndpointProfileUser` object, and set its `userId` property\. Then, assign this user object to the endpoint\. Finally, pass the updated endpoint to the targeting client with the `updateEndpointProfile:` method\. The following example assigns a user ID in the `application:didFinishLaunchingWithOptions:` method:

```
- (BOOL)application:(UIApplication *)application
didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    // . . .

    AWSPinpointEndpointProfile *profile = [_pinpoint.targetingClient currentEndpointProfile];
    // Create a user and set its userId property
    AWSPinpointEndpointProfileUser *user = [AWSPinpointEndpointProfileUser new];
    [user setUserId:@"UserIdValue"]; 
    // Assign the user to the endpoint
    [profile setUser:user]; 
    // Update the endpoint with the targeting client
    [[_pinpoint targetingClient] updateEndpointProfile: profile]; 

    // . . .

    return YES;
}
```

------

## Registering Endpoints with the AWS Amplify JavaScript Library<a name="integrate-events-amplify"></a>

You can enable a web or mobile JavaScript application to register endpoints by using the AWS Amplify JavaScript library for web and React Native\.

### Adding Custom Endpoint Attributes<a name="integrate-events-amplify-custom-attributes"></a>

After you integrate AWS Amplify with your application, you can use the `Analytics` module to add custom attributes to endpoints\.

```
import { Analytics } from 'aws-amplify';

Analytics.updateEndpoint({
    Attributes: {
        interests: ['science', 'politics', 'travel']
        // ...
    },
})
```

### Assigning User IDs to Endpoints<a name="integrate-endpoints-amplify-user-ids"></a>

You can use the `Analytics` module to assign user IDs:

```
import { Analytics } from 'aws-amplify';

Analytics.updateEndpoint({
    // Customized userId
    UserId: 'UserIdValue,
})
```

## Next Steps<a name="integrate-endpoints-next"></a>

You've updated your app to register endpoints\. Now, as users launch your app, device information and custom attributes are provided to Amazon Pinpoint\. You can use this information to define audience segments\. In the console, you can see metrics about endpoints and, if applicable, users who are assigned user IDs\.

Next, do one of the following:
+ If you're working with an Android app, update your code to track the application lifecycle and report session events to Amazon Pinpoint\. See [Managing Sessions in Your Application \(Android Only\)](integrate-sessions-android.md)\.
+ For other types of applications, update your app to report usage data\. See [Reporting Events in Your Application](integrate-events.md)\.
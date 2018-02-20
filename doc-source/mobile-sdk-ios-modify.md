# Initializing the Amazon Pinpoint Client<a name="mobile-sdk-ios-modify"></a>

After you have included AWS Mobile SDK for iOS support in your app, which enables the app to call AWS services, modify the app code to initialize the Amazon Pinpoint client\.

Add the following import to ApplicationDelegate:

------
#### [ Objective\-C ]

```
#import <AWSPinpoint/AWSPinpoint.h>
```

------
#### [ Swift ]

```
import AWSPinpoint
```

------

There are several approaches you can use to initialize the Amazon Pinpoint client\. Examples of each approach follow\.

**To initialize the Amazon Pinpoint client using the default initializer**

+ In the `application:didFinishLaunchingWithOptions:` method in the `ApplicationDelegate` for your app, initialize the Amazon Pinpoint client with the Amazon Pinpoint completion block and pass in the `launchOptions` dictionary to the Amazon Pinpoint initialization block as follows:

------
#### [ Objective\-C ]

```
-(BOOL) application: (UIApplication * ) application 
    didFinishLaunchingWithOptions: (NSDictionary * ) launchOptions {
  _pinpoint = [AWSPinpoint pinpointWithConfiguration: [
    AWSPinpointConfiguration 
        defaultPinpointConfigurationWithLaunchOptions: launchOptions
    ]
  ];
  ...
```

------
#### [ Swift ]

```
func application(application: UIApplication, 
    didFinishLaunchingWithOptions 
    launchOptions: [NSObject: AnyObject]?) -> Bool {
  pinpoint = AWSPinpoint.init(configuration:AWSPinpointConfiguration
      .defaultPinpointConfigurationWithLaunchOptions(launchOptions))
```

------

**To initialize the Amazon Pinpoint client using a custom configuration**

+ In the `application:didFinishLaunchingWithOptions:` method in the `ApplicationDelegate` for your app, initialize the Amazon Pinpoint client using a custom client ID as follows:

------
#### [ Objective\-C ]

```
-(BOOL) application: (UIApplication * ) application 
    didFinishLaunchingWithOptions: (NSDictionary * ) launchOptions {
    AWSCognitoCredentialsProvider * cognitoCredentialsProvider = [
      [AWSCognitoCredentialsProvider alloc] initWithRegionType: 
          AWSRegionUSEast1
          identityPoolId: @ "IDENTITY_POOL_ID"
    ];
    AWSServiceConfiguration * pinpointConfig = [
      [AWSServiceConfiguration alloc] initWithRegion: AWSRegionUSEast1
      credentialsProvider: cognitoCredentialsProvider
    ];

    [[AWSServiceManager defaultServiceManager] 
      setDefaultServiceConfiguration: pinpointConfig
    ];

    AWSPinpointConfiguration * configuration = [
      [AWSPinpointConfiguration alloc] initWithAppId: @ "APP_ID"
      launchOptions: launchOptions
    ];

    _pinpoint = [AWSPinpoint pinpointWithConfiguration: configuration];
    ...
```

------
#### [ Swift ]

```
func application(application: UIApplication, didFinishLaunchingWithOptions 
    launchOptions: [NSObject: AnyObject]?) -> Bool {
  let cognitoProvider:AWSCognitoCredentialsProvider = 
      AWSCognitoCredentialsProvider.init(
        regionType: AWSRegionType.USEast1, 
        identityPoolId: "IDENTITY_POOL_ID");
  let serviceConfig:AWSServiceConfiguration = 
      AWSServiceConfiguration.init(
        region: AWSRegionType.USEast1, 
        credentialsProvider: cognitoProvider);
  
  AWSServiceManager.defaultServiceManager().defaultServiceConfiguration = 
      serviceConfig;

  let pinpointConfig:AWSPinpointConfiguration = 
      AWSPinpointConfiguration.init(
        appId: "APP_ID", 
        launchOptions: launchOptions);

  pinpoint = AWSPinpoint.init(configuration:pinpointConfig)
```

------
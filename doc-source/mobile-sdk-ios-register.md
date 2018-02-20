# Registering Endpoints<a name="mobile-sdk-ios-register"></a>

When a user starts an app session \(the app comes to the foreground\), the AWS Mobile SDK for iOS automatically registers \(or updates\) an *endpoint* with Amazon Pinpoint\. In this case, an endpoint represents a unique user's mobile device\. The endpoint includes attributes that describe the device, and can also include other attributes that you define\.

After your app registers endpoints, you can segment your audience based on endpoint attributes, and you can engage these segments with tailored messaging campaigns\. You can also use the **Analytics** page in the Amazon Pinpoint console to view charts about endpoint registration and activity, such as **New endpoints** and **Daily active endpoints**\.

You can assign a single user ID to multiple endpoints\. In the context of mobile applications, a user ID represents a single user, while the endpoints associated with the user ID represent the devices that the user uses to interact with your app\. Endpoints can also represent other methods of communicating with customers, such as email addresses or mobile phone numbers\. For more information, see [Adding Endpoints](endpoints.md)\. After you assign user IDs to your endpoints, you can view charts about user activity in the console, such as **Daily active users** and **Monthly active users**\. 

## Adding Custom Endpoint Attributes<a name="mobile-sdk-ios-custom-attributes"></a>

Use the `targetingClient` of the `AWSPinpoint` class to add custom attributes to endpoints\. The following examples add a `favoriteTeams` custom attribute\.

------
#### [ Objective\-C ]

```
[[_pinpoint.targetingClient addAttribute:@[@"Lakers",@"Clippers"] forKey
    :@"favoriteTeams"];
[_pinpoint. targetingClient updateEndpointProfile];
```

------
#### [ Swift ]

```
let pinpointTargetingClient = 
    AWSMobileClient.sharedInstance.pinpoint!.targetingClient 
pinpointTargetingClient.addAttribute(["Lakers","Clippers"], 
    forKey: "favoriteTeams")
pinpointTargetingClient.updateEndpointProfile()
```

------

## Assigning User IDs to Endpoints<a name="mobile-sdk-ios-register-user"></a>

Assign user IDs to endpoints by doing either of the following:

+ Manage user sign\-up and sign\-in with Amazon Cognito user pools\.

+ Use the `AWSPinpoint` class to assign user IDs to endpoints\.

Amazon Cognito user pools provide user directories that make it easier to add sign\-up and sign\-in to your app\. When the Mobile SDK for iOS registers an endpoint with Amazon Pinpoint, Amazon Cognito automatically assigns a user ID from the user pool\. For more information, see [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-pinpoint-integration.html) in the *Amazon Cognito Developer Guide*\.

If you don't want to use Amazon Cognito user pools, you can use the `targetingClient` of the `AWSPinpoint` class to assign user IDs to endpoints, as shown in the following examples\.

------
#### [ Objective\-C ]

```
AWSPinpointEndpointProfile *profile = 
    [_pinpoint.targetingClient currentEndpointProfile];
profile.user.userId = @"UserIdValue";
[[_pinpoint targetingClient] updateEndpointProfile:profile];
```

------
#### [ Swift ]

```
let profile: AWSPinpointEndpointProfile = 
    (pinpoint?.targetingClient.currentEndpointProfile())!;
profile.user?.userId = "UserIdValue";
pinpoint?.targetingClient.update(profile);
```

------
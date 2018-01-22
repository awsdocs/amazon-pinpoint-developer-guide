# Registering Endpoints<a name="mobile-sdk-android-register"></a>

When a user starts an app session \(the app comes to the foreground\), the AWS Mobile SDK for Android automatically registers \(or updates\) an *endpoint* with Amazon Pinpoint\. The endpoint uniquely identifies the user's mobile device\. It includes attributes that describe the device, and it can include custom attributes that you define\.

After your app registers endpoints, you can segment your audience based on endpoint attributes, and you can engage these segments with tailored messaging campaigns\. You can also use the **Analytics** page in the Amazon Pinpoint console to view charts about endpoint registration and activity, such as **New endpoints** and **Daily active endpoints**\.

While the endpoint uniquely identifies the mobile device, you can assign user IDs to endpoints to uniquely identify users\. You can assign a single user ID to multiple endpoints\. In this case, the user ID would represent an individual who uses your app on multiple devices, such as a smartphone and a tablet\. After you assign user IDs to your endpoints, you can view charts about user activity in the console, such as **Daily active users** and **Monthly active users**\. 

## Adding Custom Endpoint Attributes<a name="mobile-sdk-android-custom-attributes"></a>

You can add custom attributes to the device profile as shown in the following example, which adds a `favoriteTeams` custom attribute\.

In the following example, the `AWSMobileClient` class is provided in the AWS Mobile Hub sample code to reference the Amazon Pinpoint object\.

```
AWSMobileClient.defaultMobileClient()
    .getPinpointManager().getTargetingClient().addAttribute("favoriteTeams", Arrays.asList(new String[]{"Lakers", "Clippers"}));
AWSMobileClient.defaultMobileClient()
    .getPinpointManager().getTargetingClient().updateEndpointProfile();
```

## Assigning User IDs to Endpoints<a name="mobile-sdk-android-register-user"></a>

Assign user IDs to endpoints by doing either of the following:

+ Manage user sign\-up and sign\-in with Amazon Cognito user pools\.

+ Use the `PinpointManager` class to assign user IDs to endpoints\.

Amazon Cognito user pools provide user directories that make it easier to add sign\-up and sign\-in to your app\. When the Mobile SDK for Android registers an endpoint with Amazon Pinpoint, Amazon Cognito automatically assigns a user ID from the user pool\. For more information, see [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-pinpoint-integration.html) in the *Amazon Cognito Developer Guide*\.

If you don't want to use Amazon Cognito user pools, you can use the `TargetingClient` of the `PinpointManager` class to assign user IDs to endpoints, as shown in the following example\.

```
EndpointProfile profile = this.pinpointManager.getTargetingClient().currentEndpoint();
profile.getUser().setUserId("UserIdValue");
this.pinpointManager.getTargetingClient().updateEndpointProfile(profile);
```
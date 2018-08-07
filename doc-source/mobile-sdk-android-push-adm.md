# Handling Push Notifications from Amazon Device Messaging<a name="mobile-sdk-android-push-adm"></a>

Amazon Device Messaging \(ADM\) is a service used to send push notifications to apps running on Amazon devices, such as Kindle Fire tablets\. By integrating ADM with your app, you can use Amazon Pinpoint to send notifications to your app through the ADM mobile push channel\.

**Prerequisites**  
To send push notifications to your app using Amazon Pinpoint and ADM, you need the following:  
Amazon Developer account\.
Client ID and client secret from Amazon Device Messaging\.
ADM registration ID \(provided by the end device that contains the ADM platform\)\.
You must also integrate the AWS Mobile SDK for Android before you begin\. For more information, see [Integrating the AWS Mobile SDKs or JavaScript Library with Your Application](integrate-sdk.md)\.

## Integrating ADM with Your App<a name="mobile-sdk-android-push-adm-integrate"></a>

If you are already familiar with ADM and have ADM credentials, you can follow the steps for [Integrating Your App with Amazon Device Messaging](https://developer.amazon.com/public/apis/engage/device-messaging/tech-docs/adm-integrating-your-app) in the Amazon Developer documentation\. Otherwise, for an introduction to ADM, see [Understanding Amazon Device Messaging](https://developer.amazon.com/public/apis/engage/device-messaging/tech-docs/adm-understanding)\.

To integrate with Amazon Pinpoint, your subclass implementation of `com.amazon.device.messaging.ADMMessageHandlerBase` should include the following methods and perform the corresponding calls:

`onRegistered`  
Called when the device is registered with the ADM service\. Provides the ADM registration ID that is needed to register the device with Amazon Pinpoint\. Include the following call as part of this method:  

```
pinpointManager.getNotificationClient().registerDeviceToken(registrationId)
```

`onUnregistered`  
Called when the device is no longer registered with the ADM service\.

`onMessage`  
Called when the device receives a message notification from ADM\. Include the following as part of this method:  

```
NotificaitonDetails details = NotificationDetailsBuilder.builder()
                                .intent(intent);
                                .intentAction(NotificationClient.ADM_INTENT_ACTION)
                                .build();

pinpointManager.getNotificationClient().handleCampaignPush(details)
```

## Testing ADM Push Notifications<a name="mobile-sdk-android-push-adm-test"></a>

To test, you need an Amazon Pinpoint project, an ADM client ID, and an ADM client secret\.

Before you begin, augment your app to display the device token after registration\. The device token can be retrieved by calling: 

```
pinpointManager.getNotificationClient().getDeviceToken()
```

Complete the following steps using the AWS CLI\.

**To test ADM push notifications**

1. Register ADM as a channel with your Amazon Pinpoint project\. Provide the ADM client ID and the ADM client secret\.

   ```
   aws pinpoint update-adm-channel --application-id [YourPinpointAppId] --adm-channel-request "{
       \"ClientId\": \"ADM_CLIENT_ID",
       \"Enabled\": true,
       \"ClientSecret\": \"ADM_CLIENT_SECRET"
   }"
   ```

1. Install your app on a device that has ADM enabled, and capture the generated device token\.

1. Send a direct message to the device specifying the device token as the address\.

   ```
   aws pinpoint send-messages --application-id YourPinpointAppId --message-request "{
     \"Addresses\": {
         \"DeviceToken\": {
             \"ChannelType\": \"ADM\"
         } 
     },
     \"MessageConfiguration\": {
         \"ADMMessage\": {
             \"RawContent\":\"{'pinpoint.campaign.campaign_id':'_DIRECT','pinpoint.notification.silentPush':0,'pinpoint.openApp':true,'pinpoint.notification.title':'Hello','pinpoint.notification.body':'Hello World.'}\"
         }
     }
   }"
   ```
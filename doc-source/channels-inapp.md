# Sending and retrieving in\-app messages in Amazon Pinpoint<a name="channels-inapp"></a>

You can use in\-app messages to send targeted messages to users of your applications\. In\-app messages are highly customizable\. They can include buttons that open websites or take users to specific parts of your app\. You can configure background and text colors, position the text, and add buttons and images to the notification\. You can send a single message, or create a carousel that contains up to five unique messages\. For an overview of in\-app messages, including instructions for creating in\-app message templates, see [Creating in\-app templates](https://docs.aws.amazon.com/pinpoint/latest/userguide/message-templates-creating-inapp.html) in the *Amazon Pinpoint User Guide*\.

You can use AWS Amplify to seamlessly integrate the in\-app messaging capabilities of Amazon Pinpoint into your app\. Amplify can automatically handle the processes of fetching messages, rendering messages, and sending analytics data to Amazon Pinpoint\. This integration is currently supported for React Native applications\. For more information, see [In\-App Messaging](https://docs.amplify.aws/lib/in-app-messaging/overview/q/platform/js/) in the *Amplify Framework Documentation*\.

This section provides information about requesting the in\-app messages for an endpoint in your app, and for interpreting the result of that request\.

## Retrieving in\-app messages for an endpoint<a name="channels-inapp-retrieving"></a>

Your applications can call the [GetInAppMessages](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id-inappmessages.html#GetInAppMessages) API to retrieve all of the in\-app messages that a given endpoint is entitled to\. When you call the `GetInAppMessages` API, you provide the following parameters:
+ `ApplicationId` – The unique ID of the Amazon Pinpoint project that the in\-app message campaign is associated with\.
+ `EndpointId` – The unique ID of the endpoint that you're retrieving messages for\.

When you call the API with these values, it returns a list of messages\. For more information about the response produced by this operation, see [Understanding `GetInAppMessages` API responses](#channels-inapp-response)\.

You can use the AWS SDKs to call the `GetInAppMessages` operation\. The following code examples include functions that retrieve in\-app messages\.

------
#### [ JavaScript ]

Create the client in a separate module and export it:

```
import { PinpointClient } from "@aws-sdk/client-pinpoint";
const REGION = "us-east-1";
const pinClient = new PinpointClient({ region: REGION });
export { pinClient };
```

Retrieve in\-app messages for an endpoint:

```
// Import required AWS SDK clients and commands for Node.js
import { PinpointClient, GetInAppMessagesCommand } from "@aws-sdk/client-pinpoint";
import { pinClient } from "./lib/pinClient.js";

("use strict");

//The Amazon Pinpoint application ID.
const projectId = "4c545b28d21a490cb51b0b364example";

//The ID of the endpoint to retrieve messages for.
const endpointId = "c5ac671ef67ee3ad164cf7706example";

const params = {
  ApplicationId: projectId,
  EndpointId: endpointId
};

const run = async () => {
  try {
    const data = await pinClient.send(new GetInAppMessagesCommand(params));
    console.log(JSON.stringify(data, null, 4));
    return data; 
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------
#### [ Python ]

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)

def retrieve_inapp_messages(
            pinpoint_client, project_id, endpoint_id):
    """
    Retrieves the in-app messages that a given endpoint is entitled to.

    :param pinpoint_client: A Boto3 Pinpoint client.
    :param project_id: An Amazon Pinpoint project ID.
    :param endpoint_id: The ID of the endpoint to retrieve messages for.
    :return: A JSON object that contains information about the in-app message.
    """

    try:
        response = pinpoint_client.get_in_app_messages(
            ApplicationId=project_id,
            EndpointId=endpoint_id)
    except ClientError:
        logger.exception("Couldn't retrieve messages.")
        raise
    else:
        return response

def main():
    project_id = "4c545b28d21a490cb51b0b364example"
    endpoint_id = "c5ac671ef67ee3ad164cf7706example"
    inapp_response = retrieve_inapp_messages(
        boto3.client('pinpoint'), project_id, endpoint_id)
    print(inapp_response)

if __name__ == '__main__':
    main()
```

------

## Understanding `GetInAppMessages` API responses<a name="channels-inapp-response"></a>

When you call the [GetInAppMessages](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id-inappmessages.html#GetInAppMessages) API operation, it returns a list of messages that the specified endpoint is entitled to\. Your app can then render the message based on the values in the response\.

The following is an example of the JSON object that is returned when you call the `GetInAppMessages` API:

```
{
  "InAppMessagesResponse":{
    "InAppMessageCampaigns":[
      {
        "CampaignId":"inAppTestCampaign-4c545b28d21a490cb51b0b364example",
        "DailyCap":0,
        "InAppMessage":{
          "Content":[
            {
              "BackgroundColor":"#f8e71c",
              "BodyConfig":{
                "Alignment":"CENTER",
                "Body":"This is a sample in-app message sent using Amazon Pinpoint.",
                "TextColor":"#d0021b"
              },
              "HeaderConfig":{
                "Alignment":"CENTER",
                "Header":"Sample In-App Message",
                "TextColor":"#d0021b"
              },
              "ImageUrl":"https://example.com/images/thumbnail.png",
              "PrimaryBtn":{
                "DefaultConfig":{
                  "BackgroundColor":"#d0021b",
                  "BorderRadius":50,
                  "ButtonAction":"CLOSE",
                  "Text":"Dismiss",
                  "TextColor":"#f8e71c"
                }
              }
            }
          ],
          "Layout":"MIDDLE_BANNER"
        },
        "Priority":3,
        "Schedule":{
          "EndDate":"2021-11-06T00:08:05Z",
          "EventFilter":{
            "Dimensions":{
              "Attributes":{
                
              },
              "EventType":{
                "DimensionType":"INCLUSIVE",
                "Values":[
                  "_session.start"
                ]
              },
              "Metrics":{
                
              }
            }
          }
        },
        "SessionCap":0,
        "TotalCap":0,
        "TreatmentId":"0"
      }
    ]
  }
}
```

The following sections provide more information about the components of this response\.

### `InAppMessageCampaigns` object<a name="channels-inapp-response-inappmessagecampaigns-object"></a>

The `InAppMessageCampaigns` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `InAppMessage` object<a name="channels-inapp-response-inappmessage-object"></a>

The `InAppMessage` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `HeaderConfig` object<a name="channels-inapp-response-headerconfig-object"></a>

The `HeaderConfig` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `BodyConfig` object<a name="channels-inapp-response-bodyconfig-object"></a>

The `BodyConfig` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `InAppMessageContent` object<a name="channels-inapp-response-inappmessagecontent-object"></a>

The `InAppMessageContent` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `Schedule` object<a name="channels-inapp-response-content-schedule"></a>

The `Schedule` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `InAppMessageButton` object<a name="channels-inapp-response-button-object"></a>

An `InAppMessageButton` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `DefaultButtonConfig` object<a name="channels-inapp-response-defaultbuttonconfig-object"></a>

An `DefaultButtonConfig` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)

### `OverrideButtonConfig` object<a name="channels-inapp-response-overridebuttonconfig-object"></a>

The `OverrideButtonConfig` object is only present if the in\-app message template uses override buttons\. An override button is a button that has a specific configuration for a particular device type, such as an iOS device, Android device, or a web browser\.

An `OverrideButtonConfig` object contains the following attributes:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/channels-inapp.html)
# Customizing segments with AWS Lambda<a name="segments-dynamic"></a>


|  | 
| --- |
| This is prerelease documentation for a feature in public beta release\. It is subject to change\. | 

You can use AWS Lambda to tailor how an Amazon Pinpoint campaign engages your target audience\. With AWS Lambda, you can modify the campaign's segment the moment when Amazon Pinpoint sends the campaign's message\.

AWS Lambda is a compute service that you can use to run code without provisioning or managing servers\. You package your code and upload it to Lambda as *Lambda functions*\. Lambda runs a function when the function is invoked, which might be done manually by you or automatically in response to events\. For more information, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.

To assign a Lambda function to a campaign, you define the campaign's `CampaignHook` settings by using the [Campaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id.html) resource in the Amazon Pinpoint API\. These settings include the Lambda function name\. They also include the `CampaignHook` mode, which specifies whether Amazon Pinpoint receives a return value from the function\.

A Lambda function that you assign to a campaign is referred to as an Amazon Pinpoint *extension*\.

With the `CampaignHook` settings defined, Amazon Pinpoint automatically invokes the Lambda function when it runs the campaign, before it sends the campaign's message\. When Amazon Pinpoint invokes the function, it provides *event data* about the message delivery\. This data includes the campaign's segment, which is the list of endpoints that Amazon Pinpoint sends the message to\.

If the `CampaignHook` mode is set to `FILTER`, Amazon Pinpoint allows the function to modify and return the segment before sending the message\. For example, the function might update the endpoint definitions with attributes that contain data from a source that is external to Amazon Pinpoint\. Or, the function might filter the segment by removing certain endpoints, based on conditions in your function code\. After Amazon Pinpoint receives the modified segment from your function, it sends the message to each of the segment's endpoints using the campaign's delivery channel\.

By processing your segments with AWS Lambda, you have more control over who you send messages to and what those messages contain\. You can tailor your campaigns in real time, at the moment when campaign messages are sent\. Filtering segments enables you to engage more narrowly defined subsets of your segments\. Adding or updating endpoint attributes also enables you to make new data available for message variables\.



**Note**  
You can also use the `CampaignHook` settings to assign a Lambda function that handles the message delivery\. This type of function is useful for delivering messages through custom channels that Amazon Pinpoint doesn't support, such as social media platforms\. For more information, see [Creating custom channels in Amazon Pinpoint](channels-custom.md)\.

To modify campaign segments with AWS Lambda, first create a function that processes the event data sent by Amazon Pinpoint and returns a modified segment\. Then, authorize Amazon Pinpoint to invoke the function by assigning a Lambda function policy\. Finally, assign the function to one or more campaigns by defining `CampaignHook` settings\.

## Event data<a name="segments-dynamic-payload"></a>

When Amazon Pinpoint invokes your Lambda function, it provides the following payload as the event data:

```
{
  "MessageConfiguration": {Message configuration}
  "ApplicationId": ApplicationId,
  "CampaignId": CampaignId,
  "TreatmentId": TreatmentId,
  "ActivityId": ActivityId,
  "ScheduledTime": Scheduled Time,
  "Endpoints": {
    EndpointId: {Endpoint definition}
    . . .
  }
}
```

AWS Lambda passes the event data to your function code\. The event data provides the following attributes:
+ `MessageConfiguration` – Has the same structure as the `DirectMessageConfiguration` object of the [Messages](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-messages.html) resource in the Amazon Pinpoint API\. 
+ `ApplicationId` – The ID of the Amazon Pinpoint project that the campaign belongs to\.
+ `CampaignId` – The ID of the Amazon Pinpoint campaign that the function is invoked for\.
+ `TreatmentId` – The ID of a campaign variation that's used for A/B testing\.
+ `ActivityId` – The ID of the activity that's being performed by the campaign\.
+ `ScheduledTime` – The date and time, in ISO 8601 format, when the campaign's messages will be delivered\.
+ `Endpoints` – A map that associates endpoint IDs with endpoint definitions\. Each event data payload contains up to 50 endpoints\. If the campaign segment contains more than 50 endpoints, Amazon Pinpoint invokes the function repeatedly, with up to 50 endpoints at a time, until all endpoints have been processed\. 

## Creating a Lambda function<a name="segments-dynamic-lambda-create"></a>

To learn how to create a Lambda function, see [Getting started](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\. When you create your function, remember that message delivery fails in the following conditions:
+ The Lambda function takes longer than 15 seconds to return the modified segment\.
+ Amazon Pinpoint can't decode the function's return value\.
+ The function requires more than 3 attempts from Amazon Pinpoint to successfully invoke it\.

Amazon Pinpoint only accepts endpoint definitions in the function's return value\. The function can't modify other elements in the event data\.

### Example Lambda function<a name="segments-dynamic-lambda-example"></a>

Your Lambda function processes the event data sent by Amazon Pinpoint, and it returns the modified endpoints, as shown by the following example handler, written in Node\.js:

```
'use strict';
 
exports.handler = (event, context, callback) => {
    for (var key in event.Endpoints) {
        if (event.Endpoints.hasOwnProperty(key)) {
            var endpoint = event.Endpoints[key];
            var attr = endpoint.Attributes;
            if (!attr) {
                attr = {};
                endpoint.Attributes = attr;
            }
            attr["CreditScore"] = [ Math.floor(Math.random() * 200) + 650];
        }
    }
    console.log("Received event:", JSON.stringify(event, null, 2));
    callback(null, event.Endpoints);
};
```

Lambda passes the event data to the handler as the `event` parameter\.

In this example, the handler iterates through each endpoint in the `event.Endpoints` object, and it adds a new attribute, `CreditScore`, to the endpoint\. The value of the `CreditScore` attribute is simply a random number\.

The `console.log()` statement logs the event in CloudWatch Logs\.

The `callback()` statement returns the modified endpoints to Amazon Pinpoint\. Normally, the `callback` parameter is optional in Node\.js Lambda functions, but it is required in this context because the function must return the updated endpoints to Amazon Pinpoint\.

Your function must return endpoints in the same format provided by the event data, which is a map that associates endpoint IDs with endpoint definitions, as in the following example:

```
{
    "eqmj8wpxszeqy/b3vch04sn41yw": {
        "ChannelType": "GCM",
        "Address": "4d5e6f1a2b3c4d5e6f7g8h9i0j1a2b3c",
        "EndpointStatus": "ACTIVE",
        "OptOut": "NONE",
        "Demographic": {
            "Make": "android"
        },
        "EffectiveDate": "2017-11-02T21:26:48.598Z",
        "User": {}
    },
    "idrexqqtn8sbwfex0ouscod0yto": {
        "ChannelType": "APNS",
        "Address": "1a2b3c4d5e6f7g8h9i0j1a2b3c4d5e6f",
        "EndpointStatus": "ACTIVE",
        "OptOut": "NONE",
        "Demographic": {
            "Make": "apple"
        },
        "EffectiveDate": "2017-11-02T21:26:48.598Z",
        "User": {}
    }
}
```

The example function modifies and returns the `event.Endpoints` object that it received in the event data\.

Optionally, you can include the `TitleOverride` and `BodyOverride` attributes in the endpoint definitions that you return\.

**Note**  
When you use this solution to send messages, Amazon Pinpoint honors the `TitleOverride` and `BodyOverride` attributes only for endpoints where the value of the `ChannelType` attribute is one of the following: `ADM`, `APNS`, `APNS_SANDBOX`, `APNS_VOIP`, `APNS_VOIP_SANDBOX`, `BAIDU`, `GCM`, or `SMS`\.  
Amazon Pinpoint **doesn't** honor these attributes for endpoints where the value of the `ChannelType` attribute is `EMAIL`\.

## Assigning a Lambda function policy<a name="segments-dynamic-lambda-trust-policy"></a>

Before you can use your Lambda function to process your endpoints, you must authorize Amazon Pinpoint to invoke your Lambda function\. To grant invocation permission, assign a *Lambda function policy* to the function\. A Lambda function policy is a resource\-based permissions policy that designates which entities can use your function and what actions those entities can take\.

For more information, see [Using resource\-based policies for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html) in the *AWS Lambda Developer Guide*\.

### Example function policy<a name="segments-dynamic-lambda-trust-policy-example"></a>

The following policy grants permission to the Amazon Pinpoint service principal to use the `lambda:InvokeFunction` action for a specific campaign \(*campaign\-id*\):

```
{
  "Sid": "sid",
  "Effect": "Allow",
  "Principal": {
    "Service": "pinpoint.us-east-1.amazonaws.com"
  },
  "Action": "lambda:InvokeFunction",
  "Resource": "{arn:aws:lambda:us-east-1:account-id:function:function-name}",
  "Condition": {
    "ArnLike": {
      "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:account-id:apps/application-id/campaigns/campaign-id"
    }
  }
}
```

Your function policy requires a `Condition` block that includes an `AWS:SourceArn` key\. This code states which Amazon Pinpoint campaign is allowed to invoke the function\. In this example, the policy grants permission to only a single campaign\.

To write a more generic policy, use a multicharacter match wildcard \(\*\)\. For example, you can use the following `Condition` block to allow any campaign in a specific Amazon Pinpoint project \(*application\-id*\) to invoke the function:

```
"Condition": {
  "ArnLike": {
    "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:account-id:apps/application-id/campaigns/*"
  }
}
```

If you want the Lambda function to be the default function that's used by all the campaigns for a project, we recommend that you configure the `Condition` block for the policy in the preceding way\. For information about setting a Lambda function as the default for all campaigns in a project, see *Assigning a Lambda Function to a Campaign* later in this topic\.

### Granting Amazon Pinpoint invocation permission<a name="segments-dynamic-lambda-trust-policy-assign"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to add permissions to the Lambda function policy assigned to your Lambda function\. To allow Amazon Pinpoint to invoke a function for a specific campaign, use the Lambda [https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) command, as shown in the following example:

```
$ aws lambda add-permission \
> --function-name function-name \
> --statement-id sid \
> --action lambda:InvokeFunction \
> --principal pinpoint.us-east-1.amazonaws.com \
> --source-arn arn:aws:mobiletargeting:us-east-1:account-id:apps/application-id/campaigns/campaign-id
```

If you want to provide a campaign ID for the `--source-arn` parameter, you can look up your campaign IDs by using the Amazon Pinpoint [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-campaigns.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-campaigns.html) command with the AWS CLI\. This command requires an `--application-id` parameter\. To look up your application IDs, sign in to the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/), and go to the **All projects** page\. The console shows a **Project ID** for each project, which is the project's application ID\.

When you run the Lambda `add-permission` command, AWS Lambda returns the following output:

```
{
  "Statement": "{\"Sid\":\"sid\",
    \"Effect\":\"Allow\",
    \"Principal\":{\"Service\":\"pinpoint.us-east-1.amazonaws.com\"},
    \"Action\":\"lambda:InvokeFunction\",
    \"Resource\":\"arn:aws:lambda:us-east-1:111122223333:function:function-name\",
    \"Condition\":
      {\"ArnLike\":
        {\"AWS:SourceArn\":
         \"arn:aws:mobiletargeting:us-east-1:111122223333:apps/application-id/campaigns/campaign-id\"}}}"
}
```

The `Statement` value is a JSON string version of the statement that was added to the Lambda function policy\.

## Assigning a Lambda function to a campaign<a name="segments-dynamic-assign"></a>

You can assign a Lambda function to an individual Amazon Pinpoint campaign\. Or, you can set the Lambda function as the default used by all campaigns for a project, except for those campaigns to which you assign a function individually\.

To assign a Lambda function to an individual campaign, use the Amazon Pinpoint API to create or update a [https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns.html) object, and define its `CampaignHook` attribute\. To set a Lambda function as the default for all campaigns in a project, create or update the [https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-settings.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-settings.html) resource for that project, and define its `CampaignHook` object\.

 In both cases, set the following `CampaignHook` attributes:
+ `LambdaFunctionName` – The name or ARN of the Lambda function that Amazon Pinpoint invokes before sending messages for the campaign\.
+ `Mode` – Set to `FILTER`\. With this mode, Amazon Pinpoint invokes the function and waits for it to return the modified endpoints\. After receiving them, Amazon Pinpoint sends the message\. Amazon Pinpoint waits for up to 15 seconds before failing the message delivery\.

With `CampaignHook` settings defined for a campaign, Amazon Pinpoint invokes the specified Lambda function before sending the campaign's messages\. Amazon Pinpoint waits to receive the modified endpoints from the function\. If Amazon Pinpoint receives the updated endpoints, it proceeds with the message delivery, using the updated endpoint data\.
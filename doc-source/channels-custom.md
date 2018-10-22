# Creating Custom Channels with AWS Lambda<a name="channels-custom"></a>


|  | 
| --- |
| This is prerelease documentation for a feature in public beta release\. It is subject to change\. | 

Amazon Pinpoint supports messaging channels for mobile push, email, and SMS\. However, some messaging use cases might require unsupported channels\. For example, you might want to send a message to an instant messaging service, such as Facebook Messenger, or you might want to display a notification within your web application\. In such cases, you can use AWS Lambda to create a custom channel that performs the message delivery outside of Amazon Pinpoint\.

AWS Lambda is a compute service that you can use to run code without provisioning or managing servers\. You package your code and upload it to Lambda as *Lambda functions*\. Lambda runs a function when the function is invoked, which might be done manually by you or automatically in response to events\.

For more information, see [Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction-function.html) in the *AWS Lambda Developer Guide*\.

To create a custom channel, you define a Lambda function that handles the message delivery for an Amazon Pinpoint campaign\. Then, you assign the function to a campaign by defining the campaign's [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-attributes-campaignhook-table](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-attributes-campaignhook-table) settings\. These settings include the Lambda function name and the `CampaignHook` mode\. By setting the mode to `DELIVERY`, you specify that the Lambda function handles the message delivery instead of Amazon Pinpoint\.

A Lambda function that you assign to a campaign is referred to as an Amazon Pinpoint *extension*\.

With the `CampaignHook` settings defined, Amazon Pinpoint automatically invokes the Lambda function when it runs the campaign, without sending the campaign's message to a standard channel\. Instead, Amazon Pinpoint sends *event data* about the message delivery to the function, and it allows the function to handle the delivery\. The event data includes the message body and the list of endpoints to which the message should be delivered\.

After Amazon Pinpoint successfully invokes the function, it generates a successful send event for the campaign\.

**Note**  
You can also use the `CampaignHook` settings to assign a Lambda function that modifies and returns a campaign's segment before Amazon Pinpoint delivers the campaign's message\. For more information, see [Customizing Segments with AWS Lambda](segments-dynamic.md)\.

To create a custom channel with AWS Lambda, first create a function that accepts the event data sent by Amazon Pinpoint and handles the message delivery\. Then, authorize Amazon Pinpoint to invoke the function by assigning a Lambda function policy\. Finally, assign the function to one or more campaigns by defining `CampaignHook` settings\.

## Event Data<a name="channels-custom-event-data"></a>

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

The event data provides the following attributes:
+ `MessageConfiguration` – Has the same structure as the [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-attributes-directmessageconfiguration-table](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-attributes-directmessageconfiguration-table) in the `Messages` resource in the Amazon Pinpoint API\. 
+ `ApplicationId` – The ID of the Amazon Pinpoint project to which the campaign belongs\.
+ `CampaignId` – The ID of the Amazon Pinpoint project for which the function is invoked\.
+ `TreatmentId` – The ID of a campaign variation used for A/B testing\.
+ `ActivityId` – The ID of the activity being performed by the campaign\.
+ `ScheduledTime` – The schedule time at which the campaign's messages are delivered in ISO 8601 format\.
+ `Endpoints` – A map that associates endpoint IDs with endpoint definitions\. Each event data payload contains up to 50 endpoints\. If the campaign segment contains more than 50 endpoints, Amazon Pinpoint invokes the function repeatedly, with up to 50 endpoints at a time, until all endpoints have been processed\. 

## Creating a Lambda Function<a name="channels-custom-lambda-create"></a>

To create a Lambda function, refer to [Building Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-app.html) in the *AWS Lambda Developer Guide*\.

### Example Lambda Function<a name="channels-custom-lambda-example"></a>

The following example Lambda function receives event data when Amazon Pinpoint runs a campaign, and it sends the campaign's message to Facebook Messenger:

```
"use strict";

var https = require("https");
var q = require("q");

var VERIFY_TOKEN = "my_token";
var PAGE_ACCESS_TOKEN = "EAF...DZD";
/* this constant can be put in a constants file and shared between this function and your Facebook Messenger webhook code */
var FACEBOOK_MESSENGER_PSID_ATTRIBUTE_KEY = "facebookMessengerPsid";

exports.handler = function(event, context, callback) {

    var deliverViaMessengerPromises = [];

    if (event.Message && event.Endpoints) {
        for (var endpoint in event.Endpoints) {
            if (isFbookMessengerActive(event.Endpoints[endpoint])) {
                deliverViaMessengerPromises.push(deliverViaMessenger(event.Message, event.Endpoints[endpoint].User));
            }
        }
    }

    /* default OK response */
    var response = {
        body: "ok",
        statusCode: 200
    };

    if (deliverViaMessengerPromises.length > 0) {
        q.all(deliverViaMessengerPromises).done(function() {
            callback(null, response);
        });
    } else {
        callback(null, response);
    }

}

/**
Example Pinpoint Endpoint User object where we've added custom attribute facebookMessengerPsid to store the PSID needed by Facebook's API
{
    "UserId": "7a9870b7-493c-4521-b0ca-08bbbc36e595",
    "UserAttributes": {
        "facebookMessengerPsid": [ "1667566386619741" ]
    }
}
**/
function isFbookMessengerActive(endpoint) {
    return endpoint.User && endpoint.User.UserAttributes && endpoint.User.UserAttributes[FACEBOOK_MESSENGER_PSID_ATTRIBUTE_KEY];
}

/**
Sample message object from Pinpoint. This sample was an SMS so it has "smsmessage" attribute but this will vary for each messaging channel
{
    "smsmessage": {
        "body": "This message should be intercepted by a campaign hook."
    }
}
**/
function deliverViaMessenger(message, user) {
    var deferred = q.defer();

    var messageText = message["smsmessage"]["body"];
    var pinpointUserId = user.UserId;
    var facebookPsid = user.UserAttributes[FACEBOOK_MESSENGER_PSID_ATTRIBUTE_KEY][0];
    console.log("Sending message for user %s and page %s with message:", pinpointUserId, facebookPsid, messageText);

    var messageData = {
        recipient: {
            id: facebookPsid
        },
        message: {
            text: messageText
        }
    };

    var body = JSON.stringify(messageData);
    var path = "/v2.6/me/messages?access_token=" + PAGE_ACCESS_TOKEN;
    var options = {
        host: "graph.facebook.com",
        path: path,
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        }
    };

    var req = https.request(options, httpsCallback);

    req.on("error", function(e) {
        console.log("Error posting to Facebook Messenger: " + e);
        deferred.reject(e);
    });

    req.write(body);
    req.end();

    return deferred.promise;

    function httpsCallback(response) {
        var str = "";
        response.on("data", function(chunk) {
            str += chunk;
        });
        response.on("end", function() {
            console.log(str);
            deferred.resolve(response);
        });
    }
}
```

## Assigning a Lambda Function Policy<a name="segments-dynamic-lambda-trust-policy"></a>

Before you can use your Lambda function to process your endpoints, you must authorize Amazon Pinpoint to invoke your Lambda function\. To grant invocation permission, assign a *Lambda function policy* to the function\. A Lambda function policy is a resource\-based permissions policy that designates which entities can use your function and what actions those entities can take\.

For more information, see [Using Resource\-Based Policies for AWS Lambda \(Lambda Function Policies\)](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html) in the *AWS Lambda Developer Guide*\.

### Example Function Policy<a name="segments-dynamic-lambda-trust-policy-example"></a>

The following policy grants permission to the Amazon Pinpoint service principal to use the `lambda:InvokeFunction` action:

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
      "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:account-id:/apps/application-id/campaigns/campaign-id"
    }
  }
}
```

Your function policy requires a `Condition` block that includes an `AWS:SourceArn` key\. This code states which Amazon Pinpoint campaign is allowed to invoke the function\. In this example, the policy grants permission to only a single campaign ID\. To write a more generic policy, use multi\-character match wildcards \(\*\)\. For example, you can use the following `Condition` block to allow any Amazon Pinpoint campaign in your AWS account to invoke the function:

```
"Condition": {
  "ArnLike": {
    "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:account-id:/apps/*/campaigns/*"
  }
}
```

### Granting Amazon Pinpoint Invocation Permission<a name="channels-custom-lambda-trust-policy-assign"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to add permissions to the Lambda function policy assigned to your Lambda function\. To allow Amazon Pinpoint to invoke a function, use the Lambda [https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) command, as shown by the following example:

```
$ aws lambda add-permission \
> --function-name function-name \
> --statement-id sid \
> --action lambda:InvokeFunction \
> --principal pinpoint.us-east-1.amazonaws.com \
> --source-arn arn:aws:mobiletargeting:us-east-1:account-id:/apps/application-id/campaigns/campaign-id
```

If you want to provide a campaign ID for the `--source-arn` parameter, you can look up your campaign IDs by using the Amazon Pinpoint [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-campaigns.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-campaigns.html) command with the AWS CLI\. This command requires an `--application-id` parameter\. To look up your application IDs, sign in to the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/), and go to the **Projects** page\. The console shows an **ID** for each project, which is the project's application ID\.

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
         \"arn:aws:mobiletargeting:us-east-1:111122223333:/apps/application-id/campaigns/campaign-id\"}}}"
}
```

The `Statement` value is a JSON string version of the statement added to the Lambda function policy\.

## Assigning a Lambda Function to a Campaign<a name="channels-custom-assign"></a>

You can assign a Lambda function to an individual Amazon Pinpoint campaign\. Or, you can set the Lambda function as the default used by all campaigns for a project, except for those campaigns to which you assign a function individually\.

To assign a Lambda function to an individual campaign, use the Amazon Pinpoint API to create or update a [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html) object, and define its `CampaignHook` attribute\. To set a Lambda function as the default for all campaigns in a project, create or update the [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html) resource for that project, and define its `CampaignHook` object\.

In both cases, set the following `CampaignHook` attributes:
+ `LambdaFunctionName` – The name or ARN of the Lambda function that Amazon Pinpoint invokes to send messages for the campaign\.
+ `Mode` – Set to `DELIVERY`\. With this mode, Amazon Pinpoint uses the function to deliver the messages for a campaign, and it doesn't attempt to send the messages through the standard channels\.
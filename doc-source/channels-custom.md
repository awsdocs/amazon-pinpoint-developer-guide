# Creating custom channels in Amazon Pinpoint<a name="channels-custom"></a>

Amazon Pinpoint includes built\-in support for sending messages through the push notification, email, SMS, and voice channels\. You can also configure Amazon Pinpoint to send messages through other channels by creating custom channels\. Custom channels in Amazon Pinpoint allow you to send messages through any service that has an API, including third\-party services\. You can interact with APIs by using a webhook, or by calling an AWS Lambda function\.

The segments that you send custom channel campaigns to can contain endpoints of all types \(that is, endpoints where the value of the `ChannelType` attribute is EMAIL, VOICE, SMS, CUSTOM, or one of the various push notification endpoint types\)\.

## Creating a campaign that sends messages through a custom channel<a name="channels-custom-create"></a>

To assign a Lambda function or webhook to an individual campaign, use the Amazon Pinpoint API to create or update a [Campaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns.html) object\.

The `MessageConfiguration` object in the campaign must also contain a `CustomMessage` object\. This object has one member, `Data`\. The value of `Data` is a JSON string that contains the message payload that you want to send to the custom channel\.

The campaign has to contain a `CustomDeliveryConfiguration` object\. Within the `CustomDeliveryConfiguration` object, specify the following:
+ `EndpointTypes` – An array that contains all of the endpoint types that the custom channel campaign should be sent to\. It can contain any or all of the following channel types: 
  + `ADM`
  + `APNS`
  + `APNS_SANDBOX`
  + `APNS_VOIP`
  + `APNS_VOIP_SANDBOX`
  + `BAIDU`
  + `CUSTOM`
  + `EMAIL`
  + `GCM`
  + `SMS`
  + `VOICE`
+ `DeliveryUri` – The destination that endpoints are sent to\. You can specify only one of the following:
  + The Amazon Resource Name \(ARN\) of a Lambda function that you want to execute when the campaign runs\.
  + The URL of the webhook that you want to send endpoint data to when the campaign runs\.

**Note**  
The `Campaign` object can also contain a `Hook` object\. This object is only used to create segments that are customized by a Lambda function when a campaign is executed\. For more information, see [Customizing segments with AWS Lambda](segments-dynamic.md)\.

## Understanding the event data that Amazon Pinpoint sends to custom channels<a name="channels-custom-event-data"></a>

Before you create a Lambda function that sends messages over a custom channel, you should familiarize yourself with the data that Amazon Pinpoint emits\. When a Amazon Pinpoint campaign sends messages over a custom channel, it sends a payload to the target Lambda function that resembles the following example:

```
{
  "Message":{},
  "Data":"The payload that's provided in the CustomMessage object in MessageConfiguration",
  "ApplicationId":"3a9b1f4e6c764ba7b031e7183example",
  "CampaignId":"13978104ce5d6017c72552257example",
  "TreatmentId":"0",
  "ActivityId":"575cb1929d5ba43e87e2478eeexample",
  "ScheduledTime":"2020-04-08T19:00:16.843Z",
  "Endpoints":{
    "1dbcd396df28ac6cf8c1c2b7fexample":{
      "ChannelType":"EMAIL",
      "Address":"mary.major@example.com",
      "EndpointStatus":"ACTIVE",
      "OptOut":"NONE",
      "Location":{
        "City":"Seattle",
        "Country":"USA"
      },
      "Demographic":{
        "Make":"OnePlus",
        "Platform":"android"
      },
      "EffectiveDate":"2020-04-01T01:05:17.267Z",
      "Attributes":{
        "CohortId":[
          "42"
        ]
      },
      "CreationDate":"2020-04-01T01:05:17.267Z"
    }
  }
}
```

The event data provides the following attributes:
+ `ApplicationId` – The ID of the Amazon Pinpoint project that the campaign belongs to\.
+ `CampaignId` – The ID of the Amazon Pinpoint project that invoked the Lambda function\.
+ `TreatmentId` – The ID of the campaign variant\. If you created a standard campaign, this value is always 0\. If you created an A/B test campaign, this value is an integer between 0 and 4\.
+ `ActivityId` – The ID of the activity being performed by the campaign\.
+ `ScheduledTime` – The time when Amazon Pinpoint executed the campaign, shown in ISO 8601 format\.
+ `Endpoints` – A list of the endpoints that were targeted by the campaign\. Each payload can contain up to 50 endpoints\. If the segment that the campaign was sent to contains more than 50 endpoints, Amazon Pinpoint invokes the function repeatedly, with up to 50 endpoints at a time, until all endpoints have been processed\.

You can use this sample data when creating and testing your custom channel Lambda function\.

## Configuring webhooks<a name="channels-custom-webhook-create"></a>

If you use a webhook to send custom channel messages, the URL of the webhook has to begin with "https://"\. The webhook URL can only contain alphanumeric characters, plus the following symbols: hyphen \(\-\), period \(\.\), underscore \(\_\), tilde \(\~\), question mark \(?\), slash or solidus \(/\), pound or hash sign \(\#\), and semicolon \(:\)\. The URL has to comply with [RFC3986](https://tools.ietf.org/html/rfc3986)\. 

When you create a campaign that specifies a webhook URL, Amazon Pinpoint issues an HTTP `HEAD` to that URL\. The response to the `HEAD` request must contain a header called `X-Amz-Pinpoint-AccountId`\. The value of this header must equal your AWS account ID\.

## Configuring Lambda functions<a name="channels-custom-lambda-create"></a>

This section provides an overview of the steps that you need to take when you create a Lambda function that sends messages over a custom channel\. First, you create the function\. After that, you add an execution policy to the function\. This policy allows Amazon Pinpoint to execute the policy when a campaign runs\.

For an introduction to creating Lambda functions, see [Building Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-app.html) in the *AWS Lambda Developer Guide*\.

### Example Lambda function<a name="channels-custom-lambda-example"></a>

The following code example processes the payload and logs the number of endpoints of each endpoint type in CloudWatch\.

```
import boto3
import random
import pprint
import json
import time

cloudwatch = boto3.client('cloudwatch')
    
def lambda_handler(event, context):
    customEndpoints = 0
    smsEndpoints = 0
    pushEndpoints = 0
    emailEndpoints = 0
    voiceEndpoints = 0
    numEndpoints = len(event['Endpoints'])
    
    print("Payload:\n", event)
    print("Endpoints in payload: " + str(numEndpoints))

    for key in event['Endpoints'].keys():
        if event['Endpoints'][key]['ChannelType'] == "CUSTOM":
            customEndpoints += 1
        elif event['Endpoints'][key]['ChannelType'] == "SMS":
            smsEndpoints += 1
        elif event['Endpoints'][key]['ChannelType'] == "EMAIL":
            emailEndpoints += 1
        elif event['Endpoints'][key]['ChannelType'] == "VOICE":
            voiceEndpoints += 1
        else:
            pushEndpoints += 1
            
    response = cloudwatch.put_metric_data(
        MetricData = [
            {
                'MetricName': 'EndpointCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': len(event['Endpoints'])
            },
            {
                'MetricName': 'CustomCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': customEndpoints
            },
            {
                'MetricName': 'SMSCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': smsEndpoints
            },
            {
                'MetricName': 'EmailCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': emailEndpoints
            },
            {
                'MetricName': 'VoiceCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': voiceEndpoints
            },
            {
                'MetricName': 'PushCount',
                'Dimensions': [
                    {
                        'Name': 'CampaignId',
                        'Value': event['CampaignId']
                    },
                    {
                        'Name': 'ApplicationId',
                        'Value': event['ApplicationId']
                    }
                ],
                'Unit': 'None',
                'Value': pushEndpoints
            },
            {
                'MetricName': 'EndpointCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': len(event['Endpoints'])
            },
            {
                'MetricName': 'CustomCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': customEndpoints
            },
            {
                'MetricName': 'SMSCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': smsEndpoints
            },
            {
                'MetricName': 'EmailCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': emailEndpoints
            },
            {
                'MetricName': 'VoiceCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': voiceEndpoints
            },
            {
                'MetricName': 'PushCount',
                'Dimensions': [
                ],
                'Unit': 'None',
                'Value': pushEndpoints
            }
        ],
        Namespace = 'PinpointCustomChannelExecution'
    )
    print("cloudwatchResponse:\n",response)
```

When an Amazon Pinpoint campaign executes this Lambda function, Amazon Pinpoint sends the function a list of segment members\. The function counts the number of endpoints of each `ChannelType`\. It then sends that data to Amazon CloudWatch\. You can view these metrics in the **Metrics** section of the CloudWatch console\. The metrics are available in the **PinpointCustomChannelExecution** namespace\.

You can modify this code example so that it also connects to the API of an external service in order to send messages through that service\.

### Granting Amazon Pinpoint permission to invoke the Lambda function<a name="channels-custom-lambda-trust-policy-assign"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to add permissions to the Lambda function policy assigned to your Lambda function\. To allow Amazon Pinpoint to invoke a function, use the Lambda [add\-permission](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) command, as shown by the following example:

```
aws lambda add-permission \
--function-name myFunction \ 
--statement-id sid0 \
--action lambda:InvokeFunction \
--principal pinpoint.us-east-1.amazonaws.com \
--source-arn arn:aws:mobiletargeting:us-east-1:111122223333:apps/*
```

In the preceding command, do the following:
+ Replace *myFunction* with the name of the Lambda function\.
+ Replace *us\-east\-1* with the AWS Region where you use Amazon Pinpoint\.
+ Replace *111122223333* with your AWS account ID\.

When you run the `add-permission` command, AWS Lambda returns the following output:

```
{
  "Statement": "{\"Sid\":\"sid\",
    \"Effect\":\"Allow\",
    \"Principal\":{\"Service\":\"pinpoint.us-east-1.amazonaws.com\"},
    \"Action\":\"lambda:InvokeFunction\",
    \"Resource\":\"arn:aws:lambda:us-east-1:111122223333:function:myFunction\",
    \"Condition\":
      {\"ArnLike\":
        {\"AWS:SourceArn\":
         \"arn:aws:mobiletargeting:us-east-1:111122223333:apps/*\"}}}"
}
```

The `Statement` value is a JSON string version of the statement added to the Lambda function policy\.

#### Further restricting the execution policy<a name="channels-custom-lambda-trust-policy-assign-restrict"></a>

You can modify the execution policy by restricting it to a specific Amazon Pinpoint project\. To do this, replace the `*` in the preceding example with the unique ID of the project\. You can further restrict the policy by limiting it to a specific campaign\. For example, to restrict the policy to only allow a campaign with the campaign ID `95fee4cd1d7f5cd67987c1436example` in a project with the project ID `dbaf6ec2226f0a9a8615e3ea5example`, use the following value for the `source-arn` attribute:

```
arn:aws:mobiletargeting:us-east-1:111122223333:apps/dbaf6ec2226f0a9a8615e3ea5example/campaigns/95fee4cd1d7f5cd67987c1436example
```

**Note**  
If you do restrict execution of the Lambda function to a specific campaign, you first have to create the function with a less restrictive policy\. Next, you have to create the campaign in Amazon Pinpoint and choose the function\. Finally, you have to update the execution policy to refer to the specified campaign\.
# IAM Policies for Amazon Pinpoint Users<a name="permissions-actions"></a>

You can add Amazon Pinpoint API actions to AWS Identity and Access Management \(IAM\) policies to allow or deny specific actions for Amazon Pinpoint users in your account\. The Amazon Pinpoint API actions in your policies control what users can do in the Amazon Pinpoint console\. These actions also control which programmatic requests users can make with the AWS SDKs, the AWS Command Line Interface \(AWS CLI\), or the Amazon Pinpoint APIs\.

In a policy, you specify each action with the `mobiletargeting` namespace followed by a colon and the name of the action, such as `GetSegments`\. Most actions correspond to a request to the Amazon Pinpoint API using a specific URI and HTTP method\. For example, if you allow the `mobiletargeting:GetSegments` action in a user's policy, the user is allowed to make an HTTP GET request against the [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list) URI\. This policy also allows the user to view the segments for a project in the console, and to retrieve the segments by using an AWS SDK or the AWS CLI\.

Each action is performed on a specific Amazon Pinpoint resource, which you identify in a policy statement by its Amazon Resource Name \(ARN\)\. For example, the `mobiletargeting:GetSegments` action is performed on a specific app, which you identify with the ARN, `arn:aws:mobiletargeting:region:account-id:apps/project-id`\.

You can refer generically to all Amazon Pinpoint actions or resources by using wildcards \("`*`"\)\. For example, to allow all actions for all resources, include the following in a policy statement:

```
"Effect": "Allow",
"Action": "mobiletargeting:*",
"Resource": "*"
```

## Example Policies<a name="permissions-actions-examples"></a>

The following examples demonstrate how you can manage Amazon Pinpoint access with IAM policies\.

### Amazon Pinpoint API Actions<a name="permissions-actions-examples-pin-api"></a>

#### Amazon Pinpoint Administrator<a name="permissions-actions-examples-admin"></a>

The following administrator policy allows full access to Amazon Pinpoint actions and resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:*"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Read\-Only Access<a name="permissions-actions-examples-readonly"></a>

The following policy allows read\-only access for all the projects \(apps\) in an account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "mobiletargeting:Get*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:mobiletargeting:*:account-id:apps/*"
            ]
        }
    ]
}
```

In the preceding policy example, replace *account\-id* with your AWS account ID\.

You can also create a policy that allows read\-only access to a specific Amazon Pinpoint project\. To do this, specify an AWS Region and a project ID, as shown in the following example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "mobiletargeting:Get*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:mobiletargeting:region:account-id:apps/project-id"
            ]
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of the AWS Region that you're using, *account\-id* with your AWS account ID, and *project\-id* with the unique ID of the Amazon Pinpoint project\.

### Amazon Pinpoint SMS and Voice API Actions<a name="permissions-actions-examples-pin-sms-voice-api"></a>

#### Admin Access<a name="permissions-actions-examples-admin"></a>

The following policy grants full access to the Amazon Pinpoint SMS and Voice API:

```
{
    "Version": "2018-09-05",
    "Statement": [
        {
            "Action": [
                "sms-voice:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

#### Read\-Only Access<a name="permissions-actions-examples-readonly"></a>

The following policy allows read\-only access to the Amazon Pinpoint SMS and Voice API:

```
{
    "Version": "2018-09-05",
    "Statement": [
        {
            "Action": [
                "sms-voice:Get*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

## Amazon Pinpoint API Actions<a name="permissions-actions-apiactions"></a>

This section contains API actions that you can add to the IAM policies in your AWS account\. By adding these policies to an IAM user account, you can specify which Amazon Pinpoint features that user is allowed to use\.

To learn more about the Amazon Pinpoint API, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.

**Topics**
+ [Campaigns](#permissions-actions-apiactions-campaigns)
+ [Channels](#permissions-actions-apiactions-channels)
+ [Endpoints](#permissions-actions-apiactions-endpoints)
+ [Event Streams](#permissions-actions-apiactions-event-streams)
+ [Export Jobs](#permissions-actions-apiactions-export-jobs)
+ [Import Jobs](#permissions-actions-apiactions-import-jobs)
+ [Messages](#permissions-actions-apiactions-messages)
+ [Phone Number Validate](#permissions-actions-apiactions-phone-number-validate)
+ [Projects](#permissions-actions-apiactions-projects)
+ [Reports](#permissions-actions-apiactions-reports)
+ [Segments](#permissions-actions-apiactions-segments)
+ [Tags](#permissions-actions-apiactions-tags)
+ [Users](#permissions-actions-apiactions-users)

### Campaigns<a name="permissions-actions-apiactions-campaigns"></a>

The following permissions are related to managing campaigns in your Amazon Pinpoint account\.

**`mobiletargeting:CreateCampaign`**  
Create a campaign for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns`

**`mobiletargeting:DeleteCampaign`**  
Delete a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaign`**  
Retrieve information about a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaigns-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaigns-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaignActivities`**  
Retrieve information about the activities performed by a campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaigns`**  
Retrieve information about all campaigns for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:GetCampaignVersion`**  
Retrieve information about a specific campaign version\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaignVersions`**  
Retrieve information about the current and prior versions of a campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

**`mobiletargeting:UpdateCampaign`**  
Update a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign.html-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign.html-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/campaigns/campaign-id`

### Channels<a name="permissions-actions-apiactions-channels"></a>

The following permissions are related to managing channels in your Amazon Pinpoint account\. In Amazon Pinpoint, *channels* refer to the methods that you use to contact your customers, such as sending email, SMS messages, or push notifications\.

`mobiletargeting:DeleteAdmChannel`  
Delete the Amazon Device Messaging \(ADM\) channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountid:apps/project-id/channels/adm`

`mobiletargeting:GetAdmChannel`  
Retrieve information about the ADM channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountid:apps/project-id/channels/adm`

`mobiletargeting:UpdateAdmChannel`  
Update the ADM channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountid:apps/project-id/channels/adm`

**`mobiletargeting:DeleteApnsChannel`**  
Delete the Apple Push Notification service \(APNs\) channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns`

**`mobiletargeting:GetApnsChannel`**  
Retrieve information about the APNs channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns`

**`mobiletargeting:UpdateApnsChannel`**  
Update the certificate and private key for the APNs channel for a project\. This allows Amazon Pinpoint to send push notifications to your iOS app\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns`

**`mobiletargeting:DeleteApnsSandboxChannel`**  
Delete the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_sandbox`

**`mobiletargeting:GetApnsSandboxChannel`**  
Retrieve information about the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_sandbox`

**`mobiletargeting:UpdateApnsSandboxChannel`**  
Update the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_sandbox`

**`mobiletargeting:DeleteApnsVoipChannel`**  
Delete the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip`

**`mobiletargeting:GetApnsVoipChannel`**  
Retrieve information about the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip`

**`mobiletargeting:UpdateApnsVoipChannel`**  
Update the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip`

**`mobiletargeting:DeleteApnsVoipChannel`**  
Delete the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip_sandbox`

**`mobiletargeting:GetApnsVoipChannel`**  
Retrieve information about the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip_sandbox`

**`mobiletargeting:UpdateApnsVoipChannel`**  
Update the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/apns_voip_sandbox`

**`mobiletargeting:DeleteBaiduChannel`**  
Delete the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/baidu`

**`mobiletargeting:GetBaiduChannel`**  
Retrieve information about the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/baidu`

**`mobiletargeting:UpdateBaiduChannel`**  
Update the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/baidu`

**`mobiletargeting:DeleteEmailChannel`**  
Delete the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/email`

**`mobiletargeting:GetEmailChannel`**  
Retrieve information about the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/email`

**`mobiletargeting:UpdateEmailChannel`**  
Update the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/email`

**`mobiletargeting:DeleteGcmChannel`**  
Delete the Firebase Cloud Messaging \(FCM\) or Google Cloud Messaging \(GCM\) channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/gcm`

**`mobiletargeting:GetGcmChannel`**  
Retrieve information about the FCM or GCM channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/gcm`

**`mobiletargeting:UpdateGcmChannel`**  
Update the API key for the FCM or GCM channel for a project\. This allows Amazon Pinpoint to send push notifications to your Android app\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/gcm`

**`mobiletargeting:DeleteSmsChannel`**  
Delete the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/sms`

**`mobiletargeting:GetSmsChannel`**  
Retrieve information about the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/sms`

**`mobiletargeting:UpdateSmsChannel`**  
Update the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/channels/sms`

### Endpoints<a name="permissions-actions-apiactions-endpoints"></a>

The following permissions are related to managing endpoints in your Amazon Pinpoint account\. In Amazon Pinpoint, an *endpoint* is a single destination for your messages\. For example, an endpoint could be a customer's email address, telephone number, or mobile device token\.

**`mobiletargeting:DeleteEndpoint`**  
Delete an endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/endpoints/endpoint-id`

**`mobiletargeting:GetEndpoint`**  
Retrieve information about a specific endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/endpoints/endpoint-id`

**`mobiletargeting:UpdateEndpoint`**  
Create an endpoint or update the information for an endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/endpoints/endpoint-id`

**`mobiletargeting:UpdateEndpointsBatch`**  
Create or update endpoints as a batch operation\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

### Event Streams<a name="permissions-actions-apiactions-event-streams"></a>

The following permissions are related to managing event streams for your Amazon Pinpoint account\.

**`mobiletargeting:DeleteEventStream`**  
Delete the event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/eventstream`

**`mobiletargeting:GetEventStream`**  
Retrieve information about the event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/eventstream`

**`mobiletargeting:PutEventStream`**  
Create or update an event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/eventstream`

### Export Jobs<a name="permissions-actions-apiactions-export-jobs"></a>

The following permissions are related to managing export jobs in your Amazon Pinpoint account\. In Amazon Pinpoint, you create *export jobs* to send information about endpoints to an Amazon S3 bucket for storage or analysis\.

**`mobiletargeting:CreateExportJob`**  
Create an export job for exporting endpoint definitions to Amazon S3\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/jobs/export`

**`mobiletargeting:GetExportJob`**  
Retrieve information about a specific export job for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-job.html#rest-api-export-job-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-job.html#rest-api-export-job-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/jobs/export/job-id`

**`mobiletargeting:GetExportJobs`**  
Retrieve a list of all the export jobs for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/jobs/export`

### Import Jobs<a name="permissions-actions-apiactions-import-jobs"></a>

The following permissions are related to managing import jobs in your Amazon Pinpoint account\. In Amazon Pinpoint, you create *import jobs* to create segments based on endpoint definitions stored in an Amazon S3 bucket\.

**`mobiletargeting:CreateImportJob`**  
Import endpoint definitions from Amazon S3 to create a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:GetImportJob`**  
Retrieve information about a specific import job for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/jobs/import/job-id`

**`mobiletargeting:GetImportJobs`**  
Retrieve information about all the import jobs for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

### Messages<a name="permissions-actions-apiactions-messages"></a>

The following permissions are related to sending SMS messages and push notifications from your Amazon Pinpoint account\. You can use the `SendMessages` and `SendUsersMessages` operations to send messages to specific endpoints without creating segments and campaigns first\.

**`mobiletargeting:SendMessages`**  
Send an SMS message or push notification to specific endpoints\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/messages`

**`mobiletargeting:SendUsersMessages`**  
Send an SMS message or push notification to all the endpoints that are associated with a specific user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-users-messages.html#rest-api-users-messages-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-users-messages.html#rest-api-users-messages-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/users-messages`

### Phone Number Validate<a name="permissions-actions-apiactions-phone-number-validate"></a>

The following permissions are related to using the Phone Number Validate feature in Amazon Pinpoint\.

**`mobiletargeting:PhoneNumberValidate`**  
Retrieve information about a phone number\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-phone-number-validate.html#rest-api-phone-number-validate-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-phone-number-validate.html#rest-api-phone-number-validate-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:phone/number/validate`

### Projects<a name="permissions-actions-apiactions-projects"></a>

The following permissions are related to managing projects in your Amazon Pinpoint account\. Originally, projects were referred to as *applications*\. For the purposes of these operations, an Amazon Pinpoint application is the same as an Amazon Pinpoint project\.

**`mobiletargeting:CreateApp`**  
Create a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps`

**`mobiletargeting:DeleteApp`**  
Delete a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:GetApp`**  
Retrieve information about a specific project in your Amazon Pinpoint account\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:GetApps`**  
Retrieve a list of projects in your Amazon Pinpoint account\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps`

**`mobiletargeting:GetApplicationSettings`**  
Retrieve the default settings for an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:UpdateApplicationSettings`**  
Update the default settings for an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

### Reports<a name="permissions-actions-apiactions-reports"></a>

The following permission is related to retrieving reports and metrics for your Amazon Pinpoint account\.

**`mobiletargeting:GetReports`**  
View analytics in the Amazon Pinpoint console\.  
+ URI – Not applicable
+ Method – Not applicable
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:reports`

### Segments<a name="permissions-actions-apiactions-segments"></a>

The following permissions are related to managing segments in your Amazon Pinpoint account\. In Amazon Pinpoint, *segments* are groups of recipients for your campaigns that share certain attributes that you define\.

**`mobiletargeting:CreateSegment`**  
Create a segment\. To allow a user to create a segment by importing endpoint data from outside Amazon Pinpoint, allow the `mobiletargeting:CreateImportJob` action\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:DeleteSegment`**  
Delete a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

**`mobiletargeting:GetSegment`**  
Retrieve information about a specific segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

**`mobiletargeting:GetSegmentExportJobs`**  
Retrieve information about jobs that export endpoint definitions for a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-export-jobs.html#rest-api-segment-export-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-export-jobs.html#rest-api-segment-export-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id/jobs/export`

**`mobiletargeting:GetSegments`**  
Retrieve information about the segments for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id`

**`mobiletargeting:GetSegmentImportJobs`**  
Retrieve information about jobs that create segments by importing endpoint definitions from Amazon S3\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

**`mobiletargeting:GetSegmentVersion`**  
Retrieve information about a specific segment version\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

**`mobiletargeting:GetSegmentVersions`**  
Retrieve information about the current and prior versions of a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

**`mobiletargeting:UpdateSegment`**  
Update a specific segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/segments/segment-id`

### Tags<a name="permissions-actions-apiactions-tags"></a>

The following permissions are related to managing tags for resources in your Amazon Pinpoint account\.

**`mobiletargeting:ListTagsforResource`**  
Retrieve information about the tags that are associated with a project, campaign, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:*`

**`mobiletargeting:TagResource`**  
Add one or more tags to a project, campaign, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:*`

**`mobiletargeting:UntagResource`**  
Remove one or more tags from a project, campaign, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:*`

### Users<a name="permissions-actions-apiactions-users"></a>

The following permissions are related to managing users\. In Amazon Pinpoint, *users* correspond to individuals who receive messages from you\. A single user might be associated with more than one endpoint\.

**`mobiletargeting:DeleteUser`**  
Delete all the endpoints that are associated with a user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/users/user-id`

**`mobiletargeting:GetUser`**  
Retrieve information about all the endpoints that are associated with a user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/project-id/users/user-id`

## Amazon Pinpoint SMS and Voice API Actions<a name="permissions-actions-sms-voice-apiactions"></a>

This section contains API actions that you can add to the IAM policies in your AWS account\. By adding these policies to an IAM user account, you can specify which features of the Amazon Pinpoint SMS and Voice API a user is allowed to use\.

To learn more about the Amazon Pinpoint SMS and Voice API, see the [Amazon Pinpoint SMS and Voice API Reference](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)\.

**`sms-voice:CreateConfigurationSet`**  
Create a configuration set for sending voice messages\.  
+ URI – `/sms-voice/configuration-sets`
+ Method – POST
+ Resource ARN – Not available; use `*`

**`sms-voice:DeleteConfigurationSet`**  
Delete a voice message configuration set\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*
+ Method – DELETE
+ Resource ARN – Not available; use `*`

**`sms-voice:GetConfigurationSetEventDestinations`**  
Get information about a configuration set and the event destinations that it contains\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations
+ Method – GET
+ Resource ARN – Not available; use `*`

**`sms-voice:CreateConfigurationSetEventDestination`**  
Create an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations
+ Method – POST
+ Resource ARN – Not available; use `*`

**`sms-voice:UpdateConfigurationSetEventDestination`**  
Update an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations/*EventDestinationName*
+ Method – PUT
+ Resource ARN – Not available; use `*`

**`sms-voice:DeleteConfigurationSetEventDestination`**  
Delete an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations/*EventDestinationName*
+ Method – DELETE
+ Resource ARN – Not available; use `*`

**`sms-voice:SendVoiceMessage`**  
Create and send voice messages\.  
+ URI – /sms\-voice/voice/message
+ Method – POST
+ Resource ARN – Not available; use `*`
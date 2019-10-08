# IAM Policies for Amazon Pinpoint Users<a name="permissions-actions"></a>

You can add Amazon Pinpoint API actions to AWS Identity and Access Management \(IAM\) policies to allow or deny specific actions for Amazon Pinpoint users in your account\. The Amazon Pinpoint API actions in your policies control what users can do in the Amazon Pinpoint console\. These actions also control which programmatic requests users can make with the AWS SDKs, the AWS Command Line Interface \(AWS CLI\), or the Amazon Pinpoint APIs\.

In a policy, you specify each action with the `mobiletargeting` namespace followed by a colon and the name of the action, such as `GetSegments`\. Most actions correspond to a request to the Amazon Pinpoint API using a specific URI and HTTP method\. For example, if you allow the `mobiletargeting:GetSegments` action in a user's policy, the user is allowed to make an HTTP GET request against the [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list) URI\. This policy also allows the user to view the segments for a project in the console, and to retrieve the segments by using an AWS SDK or the AWS CLI\.

Each action is performed on a specific Amazon Pinpoint resource, which you identify in a policy statement by its Amazon Resource Name \(ARN\)\. For example, the `mobiletargeting:GetSegments` action is performed on a specific project, which you identify with the ARN, `arn:aws:mobiletargeting:region:accountId:apps/projectId`\.

**Note**  
As a best practice, you should create policies that follow the principle of *least privilege*\. In other words, when you create IAM policies, they should include only the minimum permissions required to perform the task that you need to perform\. For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)\.

**Topics**
+ [Example Policies](#permissions-actions-examples)
+ [Amazon Pinpoint API Actions](#permissions-actions-apiactions)
+ [Amazon Pinpoint SMS and Voice API Actions](#permissions-actions-sms-voice-apiactions)

## Example Policies<a name="permissions-actions-examples"></a>

The following examples demonstrate how you can manage Amazon Pinpoint access with IAM policies\.

### Amazon Pinpoint API Actions<a name="permissions-actions-examples-pin-api"></a>

#### Amazon Pinpoint Administrator<a name="permissions-actions-examples-admin"></a>

The following policy allows full access to all Amazon Pinpoint actions and resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:*"
            ],
            "Resource": "arn:aws:mobiletargeting:*:accountId:*"
        }
    ]
}
```

In the preceding policy example, replace *accountId* with your AWS account ID\.

#### Read\-Only Access<a name="permissions-actions-examples-readonly"></a>

The following policy allows read\-only access to all the projects in your Amazon Pinpoint account in a specific AWS Region\. This policy applies only to the Amazon Pinpoint API\. For a policy that you can use to create read\-only access for console users, see the [next section](#permissions-actions-examples-console-readonly)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "mobiletargeting:Get*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of an AWS Region, and replace *accountId* with your AWS account ID\.

#### Console Read\-Only Access<a name="permissions-actions-examples-console-readonly"></a>

The following policy provides users with read\-only access to the Amazon Pinpoint console in a specific AWS Region\. It includes read\-only access to other services that the Amazon Pinpoint console depends on, such as Amazon SES, IAM, and Kinesis\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "mobiletargeting:Get*",
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "firehose:ListDeliveryStreams",
                "iam:ListRoles",
                "kinesis:ListStreams",
                "s3:List*",
                "ses:Describe*",
                "ses:Get*",
                "ses:List*",
                "sns:ListTopics"
            ],
            "Resource": "*"
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of an AWS Region, and replace *accountId* with your AWS account ID\.

You can also create read\-only policies that provide access to only specific projects\. The following policy lets users sign in to the console and view a list of projects\. However, it only lets users view additional information about the project that's specified in the policy\. You can modify this policy to allow access to additional projects or Regions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "mobiletargeting:GetApps",
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        },
        {
            "Effect": "Allow",
            "Action": "mobiletargeting:Get*",
            "Resource": [
                "arn:aws:mobiletargeting:region:accountId:apps/projectId",
                "arn:aws:mobiletargeting:region:accountId:apps/projectId/*",
                "arn:aws:mobiletargeting:region:accountId:reports"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ses:Get*",
                "kinesis:ListStreams",
                "firehose:ListDeliveryStreams",
                "iam:ListRoles",
                "ses:List*",
                "sns:ListTopics",
                "ses:Describe*",
                "s3:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of an AWS Region, replace *accountId* with your AWS account ID, and replace *projectId* with the ID of the Amazon Pinpoint project that you want to provide access to\.

### Amazon Pinpoint SMS and Voice API Actions<a name="permissions-actions-examples-pin-sms-voice-api"></a>

#### Administrator Access<a name="permissions-actions-examples-pin-sms-voice-api-admin"></a>

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

#### Read\-Only Access<a name="permissions-actions-examples-pin-sms-voice-api-readonly"></a>

The following policy allows read\-only access to the Amazon Pinpoint SMS and Voice API:

```
{
    "Version": "2018-09-05",
    "Statement": [
        {
            "Action": [
                "sms-voice:Get*",
                "sms-voice:List*",
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Amazon Pinpoint Email API Actions<a name="permissions-actions-examples-pin-email-api"></a>

#### Administrator Access<a name="permissions-actions-examples-pin-email-api-admin"></a>

The following policy grants full access to the Amazon Pinpoint Email API:

```
{
    "Version": "2018-09-05",
    "Statement": [
        {
            "Action": [
                "ses:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

**Note**  
This policy also grants full access to the Amazon Simple Email Service \(Amazon SES\) API\.

#### Read\-Only Access<a name="permissions-actions-examples-pin-email-api-readonly"></a>

The following policy allows read\-only access to the Amazon Pinpoint Email API:

```
{
    "Version": "2018-09-05",
    "Statement": [
        {
            "Action": [
                "ses:Describe*",
                "ses:Get*",
                "ses:List*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

**Note**  
This policy also grants read\-only access to the Amazon SES API\.

## Amazon Pinpoint API Actions<a name="permissions-actions-apiactions"></a>

This section identifies API actions that you can add to the IAM policies in your AWS account\. By adding policies to an IAM user account, you can specify which Amazon Pinpoint features that user is allowed to use\.

To learn more about the Amazon Pinpoint API, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.

**Topics**
+ [Analytics and Metrics](#permissions-actions-apiactions-metrics)
+ [Campaigns](#permissions-actions-apiactions-campaigns)
+ [Channels](#permissions-actions-apiactions-channels)
+ [Endpoints](#permissions-actions-apiactions-endpoints)
+ [Event Streams](#permissions-actions-apiactions-event-streams)
+ [Export Jobs](#permissions-actions-apiactions-export-jobs)
+ [Import Jobs](#permissions-actions-apiactions-import-jobs)
+ [Message Templates](#permissions-actions-apiactions-templates-messages)
+ [Messages](#permissions-actions-apiactions-messages)
+ [Phone Number Validation](#permissions-actions-apiactions-phone-number-validate)
+ [Projects](#permissions-actions-apiactions-projects)
+ [Segments](#permissions-actions-apiactions-segments)
+ [Tags](#permissions-actions-apiactions-tags)
+ [Users](#permissions-actions-apiactions-users)

### Analytics and Metrics<a name="permissions-actions-apiactions-metrics"></a>

The following permissions are related to viewing analytics data on the Amazon Pinpoint console and retrieving \(querying\) aggregated data for standard metrics, also referred to as *key performance indicators \(KPIs\)*, that apply to projects and campaigns\.

**`mobiletargeting:GetReports`**  
View analytics data on the Amazon Pinpoint console\.  
+ URI – Not applicable
+ Method – Not applicable
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:reports`

**`mobiletargeting:GetApplicationDateRangeKpi`**  
Retrieve \(query\) aggregated data for a standard application metric, which is a metric that applies to all the campaigns for a project\. An example of an application metric is the number of messages that were opened by recipients for each campaign that's associated with a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/kpis/daterange/kpi-name`

**`mobiletargeting:GetCampaignDateRangeKpi`**  
Retrieve \(query\) aggregated data for a standard campaign metric, which is a metric that applies to an individual campaign\. An example of a campaign metric is the number of endpoints that a campaign message was sent to\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaign-id/kpis/daterange/kpi-name`

### Campaigns<a name="permissions-actions-apiactions-campaigns"></a>

The following permissions are related to managing campaigns in your Amazon Pinpoint account\.

**`mobiletargeting:CreateCampaign`**  
Create a campaign for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns`

**`mobiletargeting:DeleteCampaign`**  
Delete a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

**`mobiletargeting:GetCampaign`**  
Retrieve information about a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaigns-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaigns-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

**`mobiletargeting:GetCampaignActivities`**  
Retrieve information about the activities performed by a campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

**`mobiletargeting:GetCampaigns`**  
Retrieve information about all campaigns for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:GetCampaignVersion`**  
Retrieve information about a specific campaign version\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

**`mobiletargeting:GetCampaignVersions`**  
Retrieve information about the current and prior versions of a campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

**`mobiletargeting:UpdateCampaign`**  
Update a specific campaign\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign.html-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign.html-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/campaigns/campaignId`

### Channels<a name="permissions-actions-apiactions-channels"></a>

The following permissions are related to managing channels in your Amazon Pinpoint account\. In Amazon Pinpoint, *channels* refer to the methods that you use to contact your customers, such as sending email, SMS messages, or push notifications\.

`mobiletargeting:DeleteAdmChannel`  
Disable the Amazon Device Messaging \(ADM\) channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/adm`

`mobiletargeting:GetAdmChannel`  
Retrieve information about the ADM channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/adm`

`mobiletargeting:UpdateAdmChannel`  
Enable or update the ADM channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/adm`

**`mobiletargeting:DeleteApnsChannel`**  
Disable the Apple Push Notification service \(APNs\) channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns`

**`mobiletargeting:GetApnsChannel`**  
Retrieve information about the APNs channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns`

**`mobiletargeting:UpdateApnsChannel`**  
Enable or update the APNs channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns`

**`mobiletargeting:DeleteApnsSandboxChannel`**  
Disable the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_sandbox`

**`mobiletargeting:GetApnsSandboxChannel`**  
Retrieve information about the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_sandbox`

**`mobiletargeting:UpdateApnsSandboxChannel`**  
Enable or update the APNs sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_sandbox`

**`mobiletargeting:DeleteApnsVoipChannel`**  
Disable the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip`

**`mobiletargeting:GetApnsVoipChannel`**  
Retrieve information about the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip`

**`mobiletargeting:UpdateApnsVoipChannel`**  
Enable or update the APNs VoIP channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip`

**`mobiletargeting:DeleteApnsVoipChannel`**  
Disable the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip_sandbox`

**`mobiletargeting:GetApnsVoipChannel`**  
Retrieve information about the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip_sandbox`

**`mobiletargeting:UpdateApnsVoipChannel`**  
Enable or update the APNs VoIP sandbox channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/apns_voip_sandbox`

**`mobiletargeting:DeleteBaiduChannel`**  
Disable the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/baidu`

**`mobiletargeting:GetBaiduChannel`**  
Retrieve information about the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/baidu`

**`mobiletargeting:UpdateBaiduChannel`**  
Enable or update the Baidu Cloud Push channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/baidu`

**`mobiletargeting:DeleteEmailChannel`**  
Disable the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/email`

**`mobiletargeting:GetEmailChannel`**  
Retrieve information about the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/email`

**`mobiletargeting:UpdateEmailChannel`**  
Enable or update the email channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/email`

**`mobiletargeting:DeleteGcmChannel`**  
Disable the Firebase Cloud Messaging \(FCM\) channel for a project\. This channel allows Amazon Pinpoint to send push notifications to an Android app through the FCM service, which replaces the Google Cloud Messaging \(GCM\) service\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/gcm`

**`mobiletargeting:GetGcmChannel`**  
Retrieve information about the FCM channel for a project\. This channel allows Amazon Pinpoint to send push notifications to an Android app through the FCM service, which replaces the Google Cloud Messaging \(GCM\) service\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/gcm`

**`mobiletargeting:UpdateGcmChannel`**  
Enable or update the FCM channel for a project\. This channel allows Amazon Pinpoint to send push notifications to an Android app through the FCM service, which replaces the Google Cloud Messaging \(GCM\) service\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/gcm`

**`mobiletargeting:DeleteSmsChannel`**  
Disable the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/sms`

**`mobiletargeting:GetSmsChannel`**  
Retrieve information about the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/sms`

**`mobiletargeting:UpdateSmsChannel`**  
Enable or update the SMS channel for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/channels/sms`

### Endpoints<a name="permissions-actions-apiactions-endpoints"></a>

The following permissions are related to managing endpoints in your Amazon Pinpoint account\. In Amazon Pinpoint, an *endpoint* is a single destination for your messages\. For example, an endpoint could be a customer's email address, telephone number, or mobile device token\.

**`mobiletargeting:DeleteEndpoint`**  
Delete an endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/endpoints/endpointId`

**`mobiletargeting:GetEndpoint`**  
Retrieve information about a specific endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/endpoints/endpointId`

**`mobiletargeting:UpdateEndpoint`**  
Create an endpoint or update the information for an endpoint\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html#rest-api-endpoint-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/endpoints/endpointId`

**`mobiletargeting:UpdateEndpointsBatch`**  
Create or update endpoints as a batch operation\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

### Event Streams<a name="permissions-actions-apiactions-event-streams"></a>

The following permissions are related to managing event streams for your Amazon Pinpoint account\.

**`mobiletargeting:DeleteEventStream`**  
Delete the event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/eventstream`

**`mobiletargeting:GetEventStream`**  
Retrieve information about the event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/eventstream`

**`mobiletargeting:PutEventStream`**  
Create or update an event stream for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/eventstream`

### Export Jobs<a name="permissions-actions-apiactions-export-jobs"></a>

The following permissions are related to managing export jobs in your Amazon Pinpoint account\. In Amazon Pinpoint, you create *export jobs* to send information about endpoints to an Amazon S3 bucket for storage or analysis\.

**`mobiletargeting:CreateExportJob`**  
Create an export job for exporting endpoint definitions to Amazon S3\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/jobs/export`

**`mobiletargeting:GetExportJob`**  
Retrieve information about a specific export job for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-job.html#rest-api-export-job-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-job.html#rest-api-export-job-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/jobs/export/jobId`

**`mobiletargeting:GetExportJobs`**  
Retrieve a list of all the export jobs for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html#rest-api-export-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/jobs/export`

### Import Jobs<a name="permissions-actions-apiactions-import-jobs"></a>

The following permissions are related to managing import jobs in your Amazon Pinpoint account\. In Amazon Pinpoint, you create *import jobs* to create segments based on endpoint definitions that are stored in an Amazon S3 bucket\.

**`mobiletargeting:CreateImportJob`**  
Import endpoint definitions from Amazon S3 to create a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:GetImportJob`**  
Retrieve information about a specific import job for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/jobs/import/jobId`

**`mobiletargeting:GetImportJobs`**  
Retrieve information about all the import jobs for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

### Message Templates<a name="permissions-actions-apiactions-templates-messages"></a>

The following permissions are related to creating and managing message templates for your Amazon Pinpoint account\. A *message template* is a set of content and settings that you optionally define, save, and reuse in email messages, push notifications, or SMS messages for your Amazon Pinpoint projects\.

**`mobiletargeting:ListTemplates`**  
Retrieve information about all the message templates that are associated with your Amazon Pinpoint account\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates`

**`mobiletargeting:GetEmailTemplate`**  
Retrieve information about a message template for messages that are sent through the email channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/EMAIL`

**`mobiletargeting:CreateEmailTemplate`**  
Create a message template for messages that are sent through the email channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/EMAIL`

**`mobiletargeting:UpdateEmailTemplate`**  
Update an existing message template for messages that are sent through the email channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/EMAIL`

**`mobiletargeting:DeleteEmailTemplate`**  
Delete a message template for messages that were sent through the email channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/EMAIL`

**`mobiletargeting:GetPushTemplate`**  
Retrieve information about a message template for messages that are sent through a push notification channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/PUSH`

**`mobiletargeting:CreatePushTemplate`**  
Create a message template for messages that are sent through a push notification channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/PUSH`

**`mobiletargeting:UpdatePushTemplate`**  
Update an existing message template for messages that are sent through a push notification channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/PUSH`

**`mobiletargeting:DeletePushTemplate`**  
Delete a message template for messages that were sent through a push notification channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/PUSH`

**`mobiletargeting:GetSmsTemplate`**  
Retrieve information about a message template for messages that are sent through the SMS channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/SMS`

**`mobiletargeting:CreateSmsTemplate`**  
Create a message template for messages that are sent through the SMS channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/SMS`

**`mobiletargeting:UpdateSmsTemplate`**  
Update an existing message template for messages that are sent through the SMS channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/SMS`

**`mobiletargeting:DeleteSmsTemplate`**  
Delete a message template for messages that were sent through the SMS channel\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:templates/template-name/SMS`

### Messages<a name="permissions-actions-apiactions-messages"></a>

The following permissions are related to sending messages and push notifications from your Amazon Pinpoint account\. You can use the `SendMessages` and `SendUsersMessages` operations to send messages to specific endpoints without creating segments and campaigns first\.

**`mobiletargeting:SendMessages`**  
Send a message or push notification to specific endpoints\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-messages.html#rest-api-messages-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/messages`

**`mobiletargeting:SendUsersMessages`**  
Send a message or push notification to all the endpoints that are associated with a specific user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-users-messages.html#rest-api-users-messages-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-users-messages.html#rest-api-users-messages-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/users-messages`

### Phone Number Validation<a name="permissions-actions-apiactions-phone-number-validate"></a>

The following permissions are related to using the phone number validation service in Amazon Pinpoint\.

**`mobiletargeting:PhoneNumberValidate`**  
Retrieve information about a phone number\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-phone-number-validate.html#rest-api-phone-number-validate-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-phone-number-validate.html#rest-api-phone-number-validate-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:phone/number/validate`

### Projects<a name="permissions-actions-apiactions-projects"></a>

The following permissions are related to managing projects in your Amazon Pinpoint account\. Originally, projects were referred to as *applications*\. For the purposes of these operations, an Amazon Pinpoint application is the same as an Amazon Pinpoint project\.

**`mobiletargeting:CreateApp`**  
Create an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps`

**`mobiletargeting:DeleteApp`**  
Delete an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:GetApp`**  
Retrieve information about an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:GetApps`**  
Retrieve information about all the projects that are associated with your Amazon Pinpoint account\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps`

**`mobiletargeting:GetApplicationSettings`**  
Retrieve the default settings for an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:UpdateApplicationSettings`**  
Update the default settings for an Amazon Pinpoint project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

### Segments<a name="permissions-actions-apiactions-segments"></a>

The following permissions are related to managing segments in your Amazon Pinpoint account\. In Amazon Pinpoint, *segments* are groups of recipients for your campaigns that share certain attributes that you define\.

**`mobiletargeting:CreateSegment`**  
Create a segment\. To allow a user to create a segment by importing endpoint data from outside Amazon Pinpoint, allow the `mobiletargeting:CreateImportJob` action\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:DeleteSegment`**  
Delete a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

**`mobiletargeting:GetSegment`**  
Retrieve information about a specific segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

**`mobiletargeting:GetSegmentExportJobs`**  
Retrieve information about jobs that export endpoint definitions for a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-export-jobs.html#rest-api-segment-export-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-export-jobs.html#rest-api-segment-export-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId/jobs/export`

**`mobiletargeting:GetSegments`**  
Retrieve information about all the segments for a project\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId`

**`mobiletargeting:GetSegmentImportJobs`**  
Retrieve information about jobs that create segments by importing endpoint definitions from Amazon S3\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

**`mobiletargeting:GetSegmentVersion`**  
Retrieve information about a specific segment version\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

**`mobiletargeting:GetSegmentVersions`**  
Retrieve information about the current and prior versions of a segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

**`mobiletargeting:UpdateSegment`**  
Update a specific segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/segments/segmentId`

### Tags<a name="permissions-actions-apiactions-tags"></a>

The following permissions are related to tagging resources in your Amazon Pinpoint account\.

**`mobiletargeting:ListTagsforResource`**  
Retrieve information about the tags that are associated with a project, campaign, message template, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:*`

**`mobiletargeting:TagResource`**  
Add one or more tags to a project, campaign, message template, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-post](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-post)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:*`

**`mobiletargeting:UntagResource`**  
Remove one or more tags from a project, campaign, message template, or segment\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html#rest-api-tags-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:*`

### Users<a name="permissions-actions-apiactions-users"></a>

The following permissions are related to managing users\. In Amazon Pinpoint, *users* correspond to individuals who receive messages from you\. A single user might be associated with more than one endpoint\.

**`mobiletargeting:DeleteUserEndpoints`**  
Delete all the endpoints that are associated with a user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-delete](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-delete)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/users/userId`

**`mobiletargeting:GetUserEndpoints`**  
Retrieve information about all the endpoints that are associated with a user ID\.  
+ URI – [https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-get](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-user.html#rest-api-user-methods-get)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:accountId:apps/projectId/users/userId`

## Amazon Pinpoint SMS and Voice API Actions<a name="permissions-actions-sms-voice-apiactions"></a>

This section identifies API actions that you can add to the IAM policies in your AWS account\. By adding policies to an IAM user account, you can specify which features of the Amazon Pinpoint SMS and Voice API that user is allowed to use\.

To learn more about the Amazon Pinpoint SMS and Voice API, see the [Amazon Pinpoint SMS and Voice API Reference](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)\.

**`sms-voice:CreateConfigurationSet`**  
Create a configuration set for sending voice messages\.  
+ URI – `/sms-voice/configuration-sets`
+ Method – POST
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:DeleteConfigurationSet`**  
Delete a configuration set for sending voice messages\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*
+ Method – DELETE
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:GetConfigurationSetEventDestinations`**  
Retrieve information about a configuration set and the event destinations that it contains\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations
+ Method – GET
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:CreateConfigurationSetEventDestination`**  
Create an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations
+ Method – POST
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:UpdateConfigurationSetEventDestination`**  
Update an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations/*EventDestinationName*
+ Method – PUT
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:DeleteConfigurationSetEventDestination`**  
Delete an event destination for voice events\.  
+ URI – /sms\-voice/configuration\-sets/*ConfigurationSetName*/event\-destinations/*EventDestinationName*
+ Method – DELETE
+ Resource ARN – Not available\. Use `*`\.

**`sms-voice:SendVoiceMessage`**  
Create and send voice messages\.  
+ URI – /sms\-voice/voice/message
+ Method – POST
+ Resource ARN – Not available\. Use `*`\.
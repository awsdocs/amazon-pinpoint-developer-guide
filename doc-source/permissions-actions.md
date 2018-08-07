# IAM Policies for Amazon Pinpoint Users<a name="permissions-actions"></a>

You can add Amazon Pinpoint API actions to AWS Identity and Access Management \(IAM\) policies to allow or deny specific actions for Amazon Pinpoint users in your account\. The Amazon Pinpoint API actions in your policies control what users can do in the Amazon Pinpoint console\. These actions also control which programmatic requests users can make with the AWS SDKs, the AWS CLI, or the Amazon Pinpoint REST API\.

In a policy, you specify each action with the `mobiletargeting` namespace followed by a colon and the name of the action, such as `GetSegments`\. Most actions correspond to a request to the Amazon Pinpoint REST API using a specific URI and HTTP method\. For example, if you allow the `mobiletargeting:GetSegments` action in a user's policy, the user is allowed to make an HTTP GET request against the [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list) URI\. This policy also allows the user to view the segments for an app in the console, and to retrieve the segments by using an AWS SDK or the AWS CLI\.

Each action is performed on a specific Amazon Pinpoint resource, which you identify in a policy statement by its Amazon Resource Name \(ARN\)\. For example, the `mobiletargeting:GetSegments` action is performed on a specific app, which you identify with the ARN, `arn:aws:mobiletargeting:region:account-id:/apps/application-id`\.

You can refer generically to all Amazon Pinpoint actions or resources by using wildcards \("`*`"\)\. For example, to allow all actions for all resources, include the following in a policy statement:

```
"Effect": "Allow",
"Action": "mobiletargeting:*",
"Resource": "*"
```

## Example Policies<a name="permissions-actions-examples"></a>

The following examples demonstrate how you can manage Amazon Pinpoint access with IAM policies\.

### Amazon Pinpoint Administrator<a name="permissions-actions-examples-admin"></a>

The following administrator policy allows full access to Amazon Pinpoint actions and resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:*",
                "mobileanalytics:*"
            ],
            "Resource": "*"
        }
    ]
}
```

In addition to the Amazon Pinpoint actions, this policy allows all Amazon Mobile Analytics actions with `mobileanalytics:*`\. Amazon Pinpoint and Amazon Mobile Analytics share data about your apps, so you must include permissions for both services in policies for Amazon Pinpoint users\.

### Read\-Only Access<a name="permissions-actions-examples-readonly"></a>

The following policy allows read\-only access for all apps in an account:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "mobiletargeting:GetEndpoint",
        "mobiletargeting:GetSegment*",
        "mobiletargeting:GetCampaign*",
        "mobiletargeting:GetImport*",
        "mobiletargeting:GetApnsChannel",
        "mobiletargeting:GetGcmChannel",
        "mobiletargeting:GetApplicationSettings",
        "mobiletargeting:GetEventStream"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:mobiletargeting:*:account-id:apps/*"
    },
    {
      "Action": "mobiletargeting:GetReports",
      "Effect": "Allow",
      "Resource": "arn:aws:mobiletargeting:*:account-id:reports"
    },
    {
      "Action": "mobileanalytics:ListApps",
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

## API Actions for IAM Policies<a name="permissions-actions-apiactions"></a>

You can add the following API actions to IAM policies to manage what Amazon Pinpoint users in your account are allowed to do\.

**`mobiletargeting:GetEndpoint`**  
Retrieve information about a specific endpoint\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/endpoints/endpoint-id`

**`mobiletargeting:UpdateEndpoint`**  
Create an endpoint or update the information for an endpoint\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-instance)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/endpoints/endpoint-id`

**`mobiletargeting:UpdateEndpointsBatch`**  
Create or update endpoints as a batch operation\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html#rest-api-endpoints-list)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:CreateSegment`**  
Create a segment that is based on endpoint data reported to Amazon Pinpoint by your app\. To allow a user to create a segment by importing endpoint data from outside of Amazon Pinpoint, allow the `mobiletargeting:CreateImportJob` action\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:DeleteSegment`**  
Delete a specific segment\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:GetSegment`**  
Retrieve information about a specific segment\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:GetSegments`**  
Retrieve information about the segments for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:GetSegmentImportJobs`**  
Retrieve information about jobs that create segments by importing endpoint definitions from Amazon S3\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list-segment](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list-segment)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:GetSegmentVersion`**  
Retrieve information about a specific segment version\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-version-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-version-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:GetSegmentVersions`**  
Retrieve information about the current and prior versions of a segment\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-versions-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-versions-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:UpdateSegment`**  
Update a specific segment\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-instance)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/segments/segment-id`

**`mobiletargeting:CreateCampaign`**  
Create a campaign for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-list)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:DeleteCampaign`**  
Delete a specific campaign\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:/apps/application-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaign`**  
Retrieve information about a specific campaign\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaignActivities`**  
Retrieve information about the activities performed by a campaign\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-activities.html#rest-api-activities-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-activities.html#rest-api-activities-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaigns`**  
Retrieve information about all campaigns for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:GetCampaignVersion`**  
Retrieve information about a specific campaign version\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-version-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-version-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/campaigns/campaign-id`

**`mobiletargeting:GetCampaignVersions`**  
Retrieve information about the current and prior versions of a campaign\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-versions-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-versions-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/campaigns/campaign-id`

**`mobiletargeting:UpdateCampaign`**  
Update a specific campaign\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-instance)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/campaigns/campaign-id`

**`mobiletargeting:CreateImportJob`**  
Import endpoint definitions from Amazon S3 to create a segment\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list)
+ Method – POST
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:GetImportJob`**  
Retrieve information about a specific import job\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/jobs/import/job-id`

**`mobiletargeting:GetImportJobs`**  
Retrieve information about all import jobs for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-jobs-import.html#rest-api-jobs-import-list)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:DeleteApnsChannel`**  
Delete the APNs channel for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/apns`

**`mobiletargeting:GetApnsChannel`**  
Retrieve information about the APNs channel for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/apns`

**`mobiletargeting:UpdateApnsChannel`**  
Update the Apple Push Notification service \(APNs\) certificate and private key, which allow Amazon Pinpoint to send push notifications to your iOS app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-apns.html#rest-api-channels-apns)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/apns`

**`mobiletargeting:DeleteGcmChannel`**  
Delete the GCM channel for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/gcm`

**`mobiletargeting:GetGcmChannel`**  
Retrieve information about the GCM channel for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/gcm`

**`mobiletargeting:UpdateGcmChannel`**  
Update the Firebase Cloud Messaging \(FCM\) or Google Cloud Messaging \(GCM\) API key, which allows Amazon Pinpoint to send push notifications to your Android app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-channels-gcm.html#rest-api-channels-gcm)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/channels/gcm`

**`mobiletargeting:GetApplicationSettings`**  
Retrieve the default settings for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:UpdateApplicationSettings`**  
Update the default settings for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id`

**`mobiletargeting:DeleteEventStream`**  
Delete the event stream for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance)
+ Method – DELETE
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/eventstream`

**`mobiletargeting:GetEventStream`**  
Retrieve information about the event stream for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance)
+ Method – GET
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/eventstream`

**`mobiletargeting:PutEventStream`**  
Create or update an event stream for an app\.  
+ URI – [http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-eventstreams.html#rest-api-eventstream-instance)
+ Method – PUT
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:apps/application-id/eventstream`

**`mobiletargeting:GetReports`**  
View analytics in the Amazon Pinpoint console\.  
+ URI – Not applicable
+ Method – Not applicable
+ Resource ARN – `arn:aws:mobiletargeting:region:account-id:reports`
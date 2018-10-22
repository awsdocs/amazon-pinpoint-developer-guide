# Logging Amazon Pinpoint API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Pinpoint is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Pinpoint\. CloudTrail captures API calls for Amazon Pinpoint as events\. The calls captured include calls from the Amazon Pinpoint console and code calls to the Amazon Pinpoint API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon Pinpoint\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Pinpoint, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon Pinpoint Information in CloudTrail<a name="pinpoint-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in Amazon Pinpoint, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon Pinpoint, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

Amazon Pinpoint supports logging the following actions as events in CloudTrail log files:
+ [CreateApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post)
+ [CreateCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post)
+ [CreateImportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post)
+ [CreateSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post)
+ [DeleteAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete)
+ [DeleteApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete)
+ [DeleteApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete)
+ [DeleteApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete)
+ [DeleteApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete)
+ [DeleteApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete)
+ [DeleteBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete)
+ [DeleteCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete)
+ [DeleteEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete)
+ [DeleteEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete)
+ [DeleteGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete)
+ [DeleteSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete)
+ [DeleteSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete)
+ [GetAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get)
+ [GetApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get)
+ [GetApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get)
+ [GetApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get)
+ [GetApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get)
+ [GetApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get)
+ [GetApplicationSettings](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get)
+ [GetApps](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get)
+ [GetBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get)
+ [GetCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-get)
+ [GetCampaignActivities](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get)
+ [GetCampaignVersion](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get)
+ [GetCampaignVersions](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get)
+ [GetCampaigns](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get)
+ [GetEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get)
+ [GetEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get)
+ [GetGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get)
+ [GetImportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get)
+ [GetImportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get)
+ [GetSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get)
+ [GetSegmentImportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get)
+ [GetSegmentVersion](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get)
+ [GetSegmentVersions](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get)
+ [GetSegments](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get)
+ [GetSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get)
+ [PutEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post)
+ [UpdateAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put)
+ [UpdateApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put)
+ [UpdateApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put)
+ [UpdateApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put)
+ [UpdateApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put)
+ [UpdateApplicationSettings](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put)
+ [UpdateBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put)
+ [UpdateCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-put)
+ [UpdateEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put)
+ [UpdateGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put)
+ [UpdateSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put)
+ [UpdateSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put)

**Note**  
The following Amazon Pinpoint actions **aren't** logged in CloudTrail:  
GetEndpoint
SendMessages
SendUsersMessages
UpdateEndpoint
UpdateEndpointsBatch

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

 You can create a trail and store your log files in your Amazon S3 bucket for as long as you want, and define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

To be notified of log file delivery, configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon Pinpoint log files from multiple AWS Regions and multiple AWS accounts into a single Amazon S3 bucket\. 

For more information, see [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

## Example: Amazon Pinpoint Log File Entries<a name="understanding-pinpoint-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\.

The following example shows a CloudTrail log entry that demonstrates the `GetCampaigns` and `CreateCampaign` actions\.

```
{
  "Records": [
    {
      "awsRegion": "us-east-1",
      "eventID": "example0-09a3-47d6-a810-c5f9fd2534fe",
      "eventName": "GetCampaigns",
      "eventSource": "pinpoint.amazonaws.com",
      "eventTime": "2018-02-03T00:56:48Z",
      "eventType": "AwsApiCall",
      "eventVersion": "1.05",
      "readOnly": true,
      "recipientAccountId": "123456789012",
      "requestID": "example1-b9bb-50fa-abdb-80f274981d60",
      "requestParameters": {
        "application-id": "example71dfa4c1aab66332a5839798f",
        "page-size": "1000"
      },
      "responseElements": null,
      "sourceIPAddress": "192.0.2.0",
      "userAgent": "Jersey/${project.version} (HttpUrlConnection 1.8.0_144)",
      "userIdentity": {
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "accountId": "123456789012",
        "arn": "arn:aws:iam::123456789012:root",
        "principalId": "123456789012",
        "sessionContext": {
          "attributes": {
            "creationDate": "2018-02-02T16:55:29Z",
            "mfaAuthenticated": "false"
          }
        },
        "type": "Root"
      }
    },
    {
      "awsRegion": "us-east-1",
      "eventID": "example0-09a3-47d6-a810-c5f9fd2534fe",
      "eventName": "CreateCampaign",
      "eventSource": "pinpoint.amazonaws.com",
      "eventTime": "2018-02-03T01:05:16Z",
      "eventType": "AwsApiCall",
      "eventVersion": "1.05",
      "readOnly": false,
      "recipientAccountId": "123456789012",
      "requestID": "example1-b9bb-50fa-abdb-80f274981d60",
      "requestParameters": {
        "Description": "***",
        "HoldoutPercent": 0,
        "IsPaused": false,
        "MessageConfiguration": "***",
        "Name": "***",
        "Schedule": {
          "Frequency": "ONCE",
          "IsLocalTime": true,
          "StartTime": "2018-02-03T00:00:00-08:00",
          "Timezone": "utc-08"
        },
        "SegmentId": "exampleda204adf991a80281aa0e591",
        "SegmentVersion": 1,
        "application-id": "example71dfa4c1aab66332a5839798f"
      },
      "responseElements": {
        "ApplicationId": "example71dfa4c1aab66332a5839798f",
        "CreationDate": "2018-02-03T01:05:16.425Z",
        "Description": "***",
        "HoldoutPercent": 0,
        "Id": "example54a654f80948680cbba240ede",
        "IsPaused": false,
        "LastModifiedDate": "2018-02-03T01:05:16.425Z",
        "MessageConfiguration": "***",
        "Name": "***",
        "Schedule": {
          "Frequency": "ONCE",
          "IsLocalTime": true,
          "StartTime": "2018-02-03T00:00:00-08:00",
          "Timezone": "utc-08"
        },
        "SegmentId": "example4da204adf991a80281example",
        "SegmentVersion": 1,
        "State": {
          "CampaignStatus": "SCHEDULED"
        },
        "Version": 1
      },
      "sourceIPAddress": "192.0.2.0",
      "userAgent": "aws-cli/1.14.9 Python/3.4.3 Linux/3.4.0+ botocore/1.8.34",
      "userIdentity": {
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "accountId": "123456789012",
        "arn": "arn:aws:iam::123456789012:user/userName",
        "principalId": "AIDAIHTHRCDA62EXAMPLE",
        "type": "IAMUser",
        "userName": "userName"
      }
    }
  ]
}
```
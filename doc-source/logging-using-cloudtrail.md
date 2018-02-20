# Logging Amazon Pinpoint API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Pinpoint is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Pinpoint\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, Amazon CloudWatch Logs, and Amazon CloudWatch Events\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Pinpoint, the IP address the request was made from, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon Pinpoint Information in CloudTrail<a name="pinpoint-info-in-cloudtrail"></a>

Amazon Pinpoint supports logging the following actions as events in CloudTrail log files:

+ [CreateApp](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post)

+ [CreateCampaign](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post)

+ [CreateImportJob](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post)

+ [CreateSegment](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post)

+ [DeleteAdmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete)

+ [DeleteApnsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete)

+ [DeleteApnsSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete)

+ [DeleteApnsVoipChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete)

+ [DeleteApnsVoipSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete)

+ [DeleteApp](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete)

+ [DeleteBaiduChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete)

+ [DeleteCampaign](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete)

+ [DeleteEmailChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete)

+ [DeleteEventStream](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete)

+ [DeleteGcmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete)

+ [DeleteSegment](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete)

+ [DeleteSmsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete)

+ [GetAdmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get)

+ [GetApnsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get)

+ [GetApnsSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get)

+ [GetApnsVoipChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get)

+ [GetApnsVoipSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get)

+ [GetApp](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get)

+ [GetApplicationSettings](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get)

+ [GetApps](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get)

+ [GetBaiduChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get)

+ [GetCampaign](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-get)

+ [GetCampaignActivities](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get)

+ [GetCampaignVersion](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get)

+ [GetCampaignVersions](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get)

+ [GetCampaigns](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get)

+ [GetEmailChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get)

+ [GetEventStream](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get)

+ [GetGcmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get)

+ [GetImportJob](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get)

+ [GetImportJobs](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get)

+ [GetSegment](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get)

+ [GetSegmentImportJobs](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get)

+ [GetSegmentVersion](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get)

+ [GetSegmentVersions](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get)

+ [GetSegments](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get)

+ [GetSmsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get)

+ [PutEventStream](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post)

+ [UpdateAdmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put)

+ [UpdateApnsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put)

+ [UpdateApnsSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put)

+ [UpdateApnsVoipChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put)

+ [UpdateApnsVoipSandboxChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put)

+ [UpdateApplicationSettings](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put)

+ [UpdateBaiduChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put)

+ [UpdateCampaign](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-put)

+ [UpdateEmailChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put)

+ [UpdateGcmChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put)

+ [UpdateSegment](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put)

+ [UpdateSmsChannel](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put)

**Important**  
The following Amazon Pinpoint actions are not logged in CloudTrail:  
GetEndpoint
SendMessages
SendUsersMessages
UpdateEndpoint
UpdateEndpointsBatch

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 

+ Whether the request was made with root or IAM user credentials\.

+ Whether the request was made with temporary security credentials for a role or federated user\.

+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

 You can create a trail and store your log files in your Amazon S3 bucket for as long as you want, and define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

To be notified of log file delivery, configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon Pinpoint log files from multiple AWS Regions and multiple AWS accounts into a single Amazon S3 bucket\. 

For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

## Example: Amazon Pinpoint Log File Entries<a name="understanding-pinpoint-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\.

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
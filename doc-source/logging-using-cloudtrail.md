# Logging Amazon Pinpoint API calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Pinpoint is integrated with AWS CloudTrail, which is a service that provides a record of actions taken by a user, role, or AWS service in Amazon Pinpoint\. CloudTrail captures API calls for Amazon Pinpoint as events\. The calls that are captured include calls from the Amazon Pinpoint console and code calls to Amazon Pinpoint API operations\. 

If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon Simple Storage Service \(Amazon S3\) bucket, including events for Amazon Pinpoint\. If you don't configure a trail, you can still view the most recent events by using **Event history** on the CloudTrail console\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Pinpoint, the IP address that the request was made from, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon Pinpoint information in CloudTrail<a name="pinpoint-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in Amazon Pinpoint, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing events with CloudTrail event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon Pinpoint, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following:
+ [Overview for creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail supported services and integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail log files from multiple regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

Every event or log entry contains information about who generated the request\. The identity information helps you determine: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see [CloudTrail userIdentity element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

You can create a trail and store your log files in your Amazon S3 bucket for as long as you want\. Also, you can define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

To be notified of log file delivery, configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon Pinpoint log files from multiple AWS Regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Receiving CloudTrail log files from multiple regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

You can use CloudTrail to log actions for the following Amazon Pinpoint APIs:
+ [Amazon Pinpoint API](#pinpoint-cloudtrail-actions)
+ [Amazon Pinpoint SMS and Voice API](#pinpoint-sms-voice-cloudtrail-actions)

## Amazon Pinpoint API actions that can be logged by CloudTrail<a name="pinpoint-cloudtrail-actions"></a>

The Amazon Pinpoint API supports logging the following actions as events in CloudTrail log files:
+ [CreateApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-post)
+ [CreateCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-post)
+ [CreateEmailTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ [CreateExportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-export.html#CreateExportJob)
+ [CreateImportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-post)
+ [CreateJourney](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys.html)
+ [CreatePushTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ [CreateRecommenderConfiguration](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders.html)
+ [CreateSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-post)
+ [CreateSmsTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ [CreateVoiceTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-voice.html)
+ [DeleteAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-delete)
+ [DeleteApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-delete)
+ [DeleteApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-delete)
+ [DeleteApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-delete)
+ [DeleteApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-delete)
+ [DeleteApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-delete)
+ [DeleteBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-delete)
+ [DeleteCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-delete)
+ [DeleteEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-delete)
+ [DeleteEmailTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ [DeleteEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#DeleteEndpoint)
+ [DeleteEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-delete)
+ [DeleteGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-delete)
+ [DeleteJourney](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id.html)
+ [DeletePushTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ [DeleteRecommenderConfiguration](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders-recommender-id.html)
+ [DeleteSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-delete)
+ [DeleteSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-delete)
+ [DeleteSmsTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ [DeleteUserEndpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-users-user-id.html#DeleteUserEndpoints)
+ [DeleteVoiceChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-channels-voice.html#DeleteVoiceChannel)
+ [DeleteVoiceTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-voice.html)
+ [GetAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-get)
+ [GetApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-get)
+ [GetApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-get)
+ [GetApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-get)
+ [GetApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-get)
+ [GetApp](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-app.html#rest-api-app-methods-get)
+ [GetApplicationDateRangeKpi](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html)
+ [GetApplicationSettings](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-get)
+ [GetApps](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apps.html#rest-api-apps-methods-get)
+ [GetBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-get)
+ [GetCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-get)
+ [GetCampaignActivities](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-activities.html#rest-api-campaign-activities-methods-get)
+ [GetCampaignDateRangeKpi](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html)
+ [GetCampaignVersion](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-version.html#rest-api-campaign-version-methods-get)
+ [GetCampaignVersions](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign-versions.html#rest-api-campaign-versions-methods-get)
+ [GetCampaigns](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaigns.html#rest-api-campaigns-methods-get)
+ [GetChannels](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-channels.html#GetChannels)
+ [GetEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-get)
+ [GetEmailTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ [GetEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/)
+ [GetEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-get)
+ [GetExportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-export-job-id.html#GetExportJob)
+ [GetExportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-export.html#GetExportJobs)
+ [GetGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-get)
+ [GetImportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-job.html#rest-api-import-job-methods-get)
+ [GetImportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-import-jobs.html#rest-api-import-jobs-methods-get)
+ [GetJourney](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id.html)
+ [GetJourneyDateRangeKpi](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id-kpis-daterange-kpi-name.html)
+ [GetJourneyExecutionActivityMetrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id-activities-journey-activity-id-execution-metrics.html)
+ [GetJourneyExecutionMetrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id-execution-metrics.html)
+ [GetPushTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ [GetRecommenderConfiguration](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders-recommender-id.html)
+ [GetRecommenderConfigurations](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders.html)
+ [GetSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-get)
+ [GetSegmentExportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-segments-segment-id-jobs-export.html#GetSegmentExportJobs)
+ [GetSegmentImportJobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-import-jobs.html#rest-api-segment-import-jobs-methods-get)
+ [GetSegmentVersion](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-version.html#rest-api-segment-version-methods-get)
+ [GetSegmentVersions](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment-versions.html#rest-api-segment-versions-methods-get)
+ [GetSegments](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segments.html#rest-api-segments-methods-get)
+ [GetSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-get)
+ [GetSmsTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ [GetUserEndpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-users-user-id.html#GetUserEndpoints)
+ [GetVoiceChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-channels-voice.html#GetVoiceChannel)
+ [GetVoiceTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-voice.html)
+ [ListJourneys](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys.html)
+ [ListTagsForResource](https://docs.aws.amazon.com/pinpoint/latest/apireference/tags-resource-arn.html)
+ [ListTemplates](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates.html)
+ [ListTemplateVersions](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-template-type-versions.html)
+ [PhoneNumberValidate](https://docs.aws.amazon.com/pinpoint/latest/apireference/phone-number-validate.html)
+ [PutEventStream](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-event-stream.html#rest-api-event-stream-methods-post)
+ [RemoveAttributes](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-attributes-attribute-type.html#RemoveAttributes)
+ [TagResource](https://docs.aws.amazon.com/pinpoint/latest/apireference/tags-resource-arn.html)
+ [UntagResource](https://docs.aws.amazon.com/pinpoint/latest/apireference/tags-resource-arn.html)
+ [UpdateAdmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-adm-channel.html#rest-api-adm-channel-methods-put)
+ [UpdateApnsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-channel.html#rest-api-apns-channel-methods-put)
+ [UpdateApnsSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-sandbox-channel.html#rest-api-apns-sandbox-channel-methods-put)
+ [UpdateApnsVoipChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-channel.html#rest-api-apns-voip-channel-methods-put)
+ [UpdateApnsVoipSandboxChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-apns-voip-sandbox-channel.html#rest-api-apns-voip-sandbox-channel-methods-put)
+ [UpdateApplicationSettings](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-settings.html#rest-api-settings-methods-put)
+ [UpdateBaiduChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-baidu-channel.html#rest-api-baidu-channel-methods-put)
+ [UpdateCampaign](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-campaign.html#rest-api-campaign-methods-put)
+ [UpdateEmailChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-email-channel.html#rest-api-email-channel-methods-put)
+ [UpdateEmailTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-email.html)
+ [UpdateEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#UpdateEndpoint)
+ [UpdateEndpointsBatch](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints.html#UpdateEndpointsBatch)
+ [UpdateGcmChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-gcm-channel.html#rest-api-gcm-channel-methods-put)
+ [UpdateJourney](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id.html)
+ [UpdateJourneyState](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-journeys-journey-id-state.html)
+ [UpdatePushTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-push.html)
+ [UpdateRecommenderConfiguration](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders-recommender-id.html)
+ [UpdateSegment](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-segment.html#rest-api-segment-methods-put)
+ [UpdateSmsChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-sms-channel.html#rest-api-sms-channel-methods-put)
+ [UpdateSmsTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-sms.html)
+ [UpdateTemplateActiveVersion](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-template-type-active-version.html)
+ [UpdateVoiceChannel](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-channels-voice.html#UpdateVoiceChannel)
+ [UpdateVoiceTemplate](https://docs.aws.amazon.com/pinpoint/latest/apireference/templates-template-name-voice.html)

The following Amazon Pinpoint API actions **aren't** logged in CloudTrail:
+ PutEvents
+ SendMessages
+ SendUsersMessages

## Amazon Pinpoint email API actions that can be logged by CloudTrail<a name="pinpoint-email-cloudtrail-actions"></a>

The Amazon Pinpoint Email API supports logging the following actions as events in CloudTrail log files:
+ [CreateConfigurationSet](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_CreateConfigurationSet.html)
+ [CreateConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_CreateConfigurationSetEventDestination.html)
+ [CreateDedicatedIpPool](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_CreateDedicatedIpPool.html)
+ [CreateEmailIdentity](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_CreateEmailIdentity.html)
+ [DeleteConfigurationSet](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_DeleteConfigurationSet.html)
+ [DeleteConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_DeleteConfigurationSetEventDestination.html)
+ [DeleteDedicatedIpPool](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_DeleteDedicatedIpPool.html)
+ [DeleteEmailIdentity](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_DeleteEmailIdentity.html)
+ [GetAccount](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetAccount.html)
+ [GetConfigurationSet](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetConfigurationSet.html)
+ [GetConfigurationSetEventDestinations](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetConfigurationSetEventDestinations.html)
+ [GetDedicatedIp](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetDedicatedIp.html)
+ [GetDedicatedIps](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetDedicatedIps.html)
+ [GetEmailIdentity](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_GetEmailIdentity.html)
+ [ListConfigurationSets](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_ListConfigurationSets.html)
+ [ListDedicatedIpPools](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_ListDedicatedIpPools.html)
+ [ListEmailIdentities](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_ListEmailIdentities.html)
+ [PutAccountDedicatedIpWarmupAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutAccountDedicatedIpWarmupAttributes.html)
+ [PutAccountSendingAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutAccountSendingAttributes.html)
+ [PutConfigurationSetDeliveryOptions](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutConfigurationSetDeliveryOptions.html)
+ [PutConfigurationSetReputationOptions](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutConfigurationSetReputationOptions.html)
+ [PutConfigurationSetSendingOptions](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutConfigurationSetSendingOptions.html)
+ [PutConfigurationSetTrackingOptions](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutConfigurationSetTrackingOptions.html)
+ [PutDedicatedIpInPool](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutDedicatedIpInPool.html)
+ [PutDedicatedIpWarmupAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutDedicatedIpWarmupAttributes.html)
+ [PutEmailIdentityDkimAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutEmailIdentityDkimAttributes.html)
+ [PutEmailIdentityFeedbackAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutEmailIdentityFeedbackAttributes.html)
+ [PutEmailIdentityMailFromAttributes](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_PutEmailIdentityMailFromAttributes.html)
+ [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-email/latest/APIReference/API_UpdateConfigurationSetEventDestination.html)

The following Amazon Pinpoint Email API action **isn't** logged in CloudTrail:
+ SendEmail

## Amazon Pinpoint SMS and voice API actions that can be logged by CloudTrail<a name="pinpoint-sms-voice-cloudtrail-actions"></a>

The Amazon Pinpoint SMS and Voice API supports logging the following actions as events in CloudTrail log files:
+ [CreateConfigurationSet](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets.html#v1-sms-voice-configuration-setspost)
+ [CreateConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets-configurationsetname-event-destinations.html#v1-sms-voice-configuration-sets-configurationsetname-event-destinationspost)
+ [DeleteConfigurationSet](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets-configurationsetname.html#v1-sms-voice-configuration-sets-configurationsetnamedelete)
+ [DeleteConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets-configurationsetname-event-destinations-eventdestinationname.html#v1-sms-voice-configuration-sets-configurationsetname-event-destinations-eventdestinationnamedelete)
+ [GetConfigurationSetEventDestinations](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets-configurationsetname-event-destinations.html#v1-sms-voice-configuration-sets-configurationsetname-event-destinationsget)
+ [UpdateConfigurationSetEventDestination](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/v1-sms-voice-configuration-sets-configurationsetname-event-destinations-eventdestinationname.html#v1-sms-voice-configuration-sets-configurationsetname-event-destinations-eventdestinationnameput)

The following Amazon Pinpoint SMS and Voice API action **isn't** logged in CloudTrail:
+ SendVoiceMessage

## Examples: Amazon Pinpoint log file entries<a name="understanding-pinpoint-entries"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source\. It includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\.

The following example shows a CloudTrail log entry that demonstrates the `GetCampaigns` and `CreateCampaign` actions of the Amazon Pinpoint API\.

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

The following example shows a CloudTrail log entry that demonstrates the `CreateConfigurationSet` and `CreateConfigurationSetEventDestination` actions in the Amazon Pinpoint SMS and Voice API\.

```
{
  "Records": [
    {
      "eventVersion":"1.05",
      "userIdentity":{
        "type":"IAMUser",
        "principalId":"AIDAIHTHRCDA62EXAMPLE",
        "arn":"arn:aws:iam::111122223333:user/SampleUser",
        "accountId":"111122223333",
        "accessKeyId":"AKIAIOSFODNN7EXAMPLE",
        "userName":"SampleUser"
      },
      "eventTime":"2018-11-06T21:45:55Z",
      "eventSource":"sms-voice.amazonaws.com",
      "eventName":"CreateConfigurationSet",
      "awsRegion":"us-east-1",
      "sourceIPAddress":"192.0.0.1",
      "userAgent":"PostmanRuntime/7.3.0",
      "requestParameters":{
        "ConfigurationSetName":"MyConfigurationSet"
      },
      "responseElements":null,
      "requestID":"56dcc091-e20d-11e8-87d2-9994aexample",
      "eventID":"725843fc-8846-41f4-871a-7c52dexample",
      "readOnly":false,
      "eventType":"AwsApiCall",
      "recipientAccountId":"123456789012"
    },
    {
      "eventVersion":"1.05",
      "userIdentity":{
        "type":"IAMUser",
        "principalId":"AIDAIHTHRCDA62EXAMPLE",
        "arn":"arn:aws:iam::111122223333:user/SampleUser",
        "accountId":"111122223333",
        "accessKeyId":"AKIAIOSFODNN7EXAMPLE",
        "userName":"SampleUser"
      },
      "eventTime":"2018-11-06T21:47:08Z",
      "eventSource":"sms-voice.amazonaws.com",
      "eventName":"CreateConfigurationSetEventDestination",
      "awsRegion":"us-east-1",
      "sourceIPAddress":"192.0.0.1",
      "userAgent":"PostmanRuntime/7.3.0",
      "requestParameters":{
        "EventDestinationName":"CloudWatchEventDestination",
        "ConfigurationSetName":"MyConfigurationSet",
        "EventDestination":{
          "Enabled":true,
          "MatchingEventTypes":[
            "INITIATED_CALL",
            "INITIATED_CALL"
          ],
          "CloudWatchLogsDestination":{
            "IamRoleArn":"arn:aws:iam::111122223333:role/iamrole-01",
            "LogGroupArn":"arn:aws:logs:us-east-1:111122223333:log-group:clientloggroup-01"
          }
        }
      },
      "responseElements":null,
      "requestID":"81de1e73-e20d-11e8-b158-d5536example",
      "eventID":"fcafc21f-7c93-4a3f-9e72-fca2dexample",
      "readOnly":false,
      "eventType":"AwsApiCall",
      "recipientAccountId":"111122223333"
    }
  ]
}
```
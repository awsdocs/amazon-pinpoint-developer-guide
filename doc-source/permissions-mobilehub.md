# AWS Mobile Hub Service Role<a name="permissions-mobilehub"></a>

AWS Mobile Hub creates an AWS Identity and Access Management \(IAM\) role in your AWS account when you agree to a one\-time request in the Mobile Hub console to manage AWS resources and services for you\. This role, called `MobileHub_Service_Role`, allows Mobile Hub to create and modify your AWS resources and services for your Mobile Hub project\.

For more information about the Mobile Hub service role, see [Mobile Hub Service Role and Policies Used on Your Behalf](https://docs.aws.amazon.com/mobile-hub/latest/developerguide/mobilehub-service-role.html) in the *AWS Mobile Hub Developer Guide*\.

To add an app to Amazon Pinpoint, you create a Mobile Hub project and configure it to include the User Engagement feature\. To support this feature, Mobile Hub adds the following permissions to the Mobile Hub service role:

```
{
  "Effect": "Allow",
  "Action": [
    "mobiletargeting:UpdateApnsChannel",
    "mobiletargeting:UpdateApnsSandboxChannel",
    "mobiletargeting:UpdateGcmChannel",
    "mobiletargeting:DeleteApnsChannel",
    "mobiletargeting:DeleteApnsSandboxChannel",
    "mobiletargeting:DeleteGcmChannel"
  ],
  "Resource": [
    "arn:aws:mobiletargeting:*:*:apps/*/channels/*"
  ]
}
```

These permissions allow Mobile Hub to manage the channels that Amazon Pinpoint uses to deliver messages to the push notification services for iOS and Android\. Mobile Hub creates or updates a channel when you provide your credentials for Apple Push Notification service, Firebase Cloud Messaging, or Google Cloud Messaging\. You provide your credentials by using the Mobile Hub console or Amazon Pinpoint console\.
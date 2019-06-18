# Permissions<a name="permissions"></a>

To use Amazon Pinpoint, users in your AWS account require permissions that allow them to view analytics data, define user segments, create and deploy campaigns, and more\. Users of your app also require access to Amazon Pinpoint so that your app can register endpoints and report usage data\. To grant access to Amazon Pinpoint features, create AWS Identity and Access Management \(IAM\) [policies that allow Amazon Pinpoint actions](permissions-actions.md)\.

IAM is a service that helps you securely control access to AWS resources\. IAM policies include statements that allow or deny specific actions that users can perform on specific resources\. Amazon Pinpoint provides [a set of actions for IAM policies](permissions-actions.md#permissions-actions-apiactions) that you can use to specify granular permissions for Amazon Pinpoint users\. You can grant the appropriate level of access to Amazon Pinpoint without creating overly permissive policies that might expose important data or compromise your campaigns\. For example, you can grant unrestricted access to an Amazon Pinpoint administrator, and grant read\-only access to individuals in your organization who need access to only analytics\.

For more information about IAM policies, see [Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

To import endpoint definitions, you must grant Amazon Pinpoint [read\-only access to an Amazon S3 bucket](permissions-import-segment.md)\.

**Topics**
+ [IAM Policies for Amazon Pinpoint Users](permissions-actions.md)
+ [IAM Role for Importing Endpoints or Segments](permissions-import-segment.md)
+ [IAM Role for Exporting Endpoints or Segments](permissions-export-endpoints.md)
+ [IAM Role for Streaming Events to Kinesis](permissions-streams.md)
+ [IAM Role for Streaming Email Events to Kinesis Data Firehose](permissions-stream-email-events-kinesis.md)
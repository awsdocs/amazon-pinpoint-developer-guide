# Permissions<a name="permissions"></a>

To use Amazon Pinpoint, users in your AWS account require permissions that allow them to view usage data reported by your app, define user segments, create push notification campaigns, and more\. Your app users require access to Amazon Pinpoint so that your app can register endpoints and report usage data\. To grant access to Amazon Pinpoint features, create AWS Identity and Access Management \(IAM\) policies that allow Amazon Pinpoint actions\.

IAM is a service that helps you securely control access to AWS resources\. IAM policies include statements that allow or deny specific actions that users can perform on specific resources\. Amazon Pinpoint provides a set of actions for IAM policies that you can use to specify granular permissions for Amazon Pinpoint users\. You can grant the appropriate level of access to Amazon Pinpoint without creating overly permissive policies that might expose important data or compromise your campaigns\. For example, you can grant unrestricted access to an Amazon Pinpoint administrator, and grant read\-only access to individuals in your organization who only need access to analytics\.

For more information about IAM policies, see [Overview of IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

When you add your app to Amazon Pinpoint by creating a project in AWS Mobile Hub, Mobile Hub automatically provisions AWS resources for app user authentication\. Mobile Hub creates an Amazon Cognito identity pool so that app users can authenticate with AWS\. Mobile Hub also creates an IAM role that allows app users to register with Amazon Pinpoint and report usage data\. You can customize these resources as needed for your app\.

To import endpoint definitions, you must grant Amazon Pinpoint read\-only access to an Amazon S3 bucket\.  


+ [IAM Policies for Amazon Pinpoint Users](permissions-actions.md)
+ [User Authentication in Amazon Pinpoint Apps](permissions-authentication.md)
+ [AWS Mobile Hub Service Role](permissions-mobilehub.md)
+ [IAM Role for Importing Segments](permissions-import.md)
+ [IAM Role for Streaming Events to Kinesis](permissions-streams.md)
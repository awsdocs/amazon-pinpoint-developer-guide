# IAM roles for common Amazon Pinpoint tasks<a name="security_iam_roles-common"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an AWS Identity and Access Management \(IAM\) identity that you can create in your AWS account and grant specific permissions\. An IAM role is similar to an IAM user, in that it's an AWS identity with permission policies that determine what the identity can and cannot do in AWS\. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it\. 

Also, a role doesn't have standard long\-term credentials such as a password or access keys associated with it\. Instead, it provides temporary security credentials for a session\. You can use IAM roles to delegate access to users, apps, applications, or services that don't normally have access to your AWS resources\.

For these reasons, you can use IAM roles to integrate Amazon Pinpoint with certain AWS services and resources for your account\. For example, you might want to allow Amazon Pinpoint to access endpoint definitions that you store in an Amazon Simple Storage Service \(Amazon S3\) bucket and want to use for segments\. Or you might want to allow Amazon Pinpoint to stream event data to an Amazon Kinesis stream for your account\. Similarly, you might want to use IAM roles to allow web or mobile apps to register endpoints or report usage data for Amazon Pinpoint projects, without embedding AWS keys in the apps \(where they can be difficult to rotate and users can potentially extract them\)\.

For these scenarios, you can delegate access to Amazon Pinpoint by using IAM roles\. This section explains and provides examples of common Amazon Pinpoint tasks that use IAM roles to work with other AWS services\. For information about using IAM roles with web and mobile apps more specifically, see [Providing access to externally authenticated users \(identity federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html) in the *IAM User Guide*\.

**Topics**
+ [IAM role for importing endpoints or segments](permissions-import-segment.md)
+ [IAM role for exporting endpoints or segments](permissions-export-endpoints.md)
+ [IAM role for retrieving recommendations from Amazon Personalize](permissions-get-recommendations.md)
+ [IAM role for streaming events to Kinesis](permissions-streams.md)
+ [IAM role for streaming email events to Kinesis Data Firehose](permissions-stream-email-events-kinesis.md)
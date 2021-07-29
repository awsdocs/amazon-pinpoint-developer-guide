# Data protection in Amazon Pinpoint<a name="security-data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in Amazon Pinpoint\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with Amazon Pinpoint or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

Depending on how you configure and use the service, Amazon Pinpoint might store the following types of personal data for you or about your customers:

**Configuration data**  
This includes project configuration data such as credentials and settings that define how and when Amazon Pinpoint sends messages through supported channels, and the user segments that it sends messages to\. To send messages, this data can include dedicated IP addresses for email messages, short codes and sender IDs for SMS text messages, and credentials for communicating with push notification services such as the Apple Push Notification service \(APNs\) and Firebase Cloud Messaging \(FCM\)\.

**User and endpoint data**  
This includes standard and custom attributes that you use to store and manage data about users and endpoints for an Amazon Pinpoint project\. An attribute can store information about a specific user \(such as a user's name\) or a specific endpoint for a user \(such as a user's email address, mobile phone number, or mobile device token\)\. This data can also include external user IDs that correlate users for an Amazon Pinpoint project with users in an external system, such as a customer relationship management system\. For more information about what this data can include, see the [User](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-users-user-id.html) and [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) schemas in the *Amazon Pinpoint API Reference*\.

**Analytics data**  
This includes data for metrics, also referred to as *key performance indicators \(KPIs\)*, that provide insight into the performance of an Amazon Pinpoint project for areas such as user engagement and purchase activity\. This also includes data for metrics that provide insight into user demographics for a project\. The data can derive from standard and custom attributes for users and endpoints, such as the city where a user lives\. It can also derive from events, such as open and click events for the email messages that you send for a project\.

**Imported data**  
This includes any user, segmentation, and analytics data that you add or import from external sources and use in Amazon Pinpoint\. An example is a JSON file that you import into Amazon Pinpoint \(through the console directly or from an Amazon S3 bucket\) to build a static segment\. Other examples are endpoint data that you add programmatically to build a dynamic segment, endpoint addresses that you send direct messages to, and events that you configure an app to report to Amazon Pinpoint\.

**Topics**
+ [Data encryption](security-data-protection-encryption.md)
+ [Internetwork traffic privacy](security-data-protection-internetwork-traffic.md)
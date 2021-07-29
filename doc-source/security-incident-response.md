# Logging and monitoring in Amazon Pinpoint<a name="security-incident-response"></a>

Logging and monitoring are an important part of maintaining the reliability, availability, and performance of your Amazon Pinpoint projects and other types of Amazon Pinpoint resources\. You should log and collect monitoring data from all parts of your Amazon Pinpoint projects and resources to more easily debug a multipoint failure if one occurs\. AWS provides several tools that can help you log and collect this data, and respond to potential incidents:

**AWS CloudTrail**  
Amazon Pinpoint integrates with AWS CloudTrail, which is a service that provides a record of actions that were taken in Amazon Pinpoint by a user, a role, or another AWS service\. This includes actions from the Amazon Pinpoint console and programmatic calls to Amazon Pinpoint API operations\. By using the information collected by CloudTrail, you can determine which requests were made to Amazon Pinpoint\. For each request, you can identify when it was made, the IP address from which it was made, who made it, and additional details\. For more information, see [Logging Amazon Pinpoint API calls with AWS CloudTrail](logging-using-cloudtrail.md) in this guide\.

**Amazon CloudWatch**  
You can use Amazon CloudWatch to collect, view, and analyze several important metrics related to your Amazon Pinpoint account and projects\. You can also use CloudWatch to create alarms that notify you if the value for a metric meets certain conditions and is within or exceeds a threshold that you define\. If you create an alarm, CloudWatch sends a notification to an Amazon Simple Notification Service \(Amazon SNS\) topic that you specify\. For more information, see [Monitoring Amazon Pinpoint with amazon CloudWatch](https://docs.aws.amazon.com/pinpoint/latest/userguide/monitoring.html) in the *Amazon Pinpoint User Guide*\.

**AWS Health Dashboards**  
By using AWS Health dashboards, you can check and monitor the status of your Amazon Pinpoint environment\. To check the status of the Amazon Pinpoint service overall, use the AWS Service Health Dashboard\. To check, monitor, and view historical data about any events or issues that might affect your AWS environment more specifically, use the AWS Personal Health Dashboard\. To learn more about these dashboards, see the [AWS Health User Guide](https://docs.aws.amazon.com/health/latest/ug/)\.

**AWS Trusted Advisor**  
AWS Trusted Advisor inspects your AWS environment and provides recommendations for opportunities to address security gaps, improve system availability and performance, and save money\. All AWS customers have access to a core set of Trusted Advisor checks\. Customers who have a Business or Enterprise support plan have access to additional Trusted Advisor checks\.  
Many of these checks can help you assess the security posture of your Amazon Pinpoint resources as part of your AWS account overall\. For example, the core set of Trusted Advisor checks includes the following:  
+ Logging configurations for your AWS account, for each supported AWS Region\.
+ Access permissions for your Amazon Simple Storage Service \(Amazon S3\) buckets, which might contain files that you import into Amazon Pinpoint to build segments\.
+ Use of AWS Identity and Access Management \(IAM\) users, groups, and roles to control access to Amazon Pinpoint resources\.
+ IAM configurations and policy settings that might compromise the security of your AWS environment and Amazon Pinpoint resources\.
For more information, see [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html#trusted-advisor) in the *AWS Support User Guide*\.
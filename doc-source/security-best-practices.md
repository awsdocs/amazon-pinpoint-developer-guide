# Security best practices for Amazon Pinpoint<a name="security-best-practices"></a>

Use AWS Identity and Access Management \(IAM\) accounts to control access to Amazon Pinpoint API operations, especially operations that create, modify, or delete Amazon Pinpoint resources\. For the Amazon Pinpoint API, such resources include projects, campaigns and journeys\. For the Amazon Pinpoint SMS and Voice API, such resources include phone numbers, pools and configuration sets\. 
+ Create an individual  user for each person who manages Amazon Pinpoint resources, including yourself\. Don't use AWS root credentials to manage Amazon Pinpoint resources\.
+ Grant each user the minimum set of permissions required to perform his or her duties\.
+ Use IAM groups to effectively manage permissions for multiple users\.
+ Rotate your IAM credentials regularly\.

For more information about Amazon Pinpoint security, see [Security in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/developerguide/security_iam_service-with-iam.html)\. For more information about IAM, see [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-set-up.html)\. For information on IAM best practices, see [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)\. 
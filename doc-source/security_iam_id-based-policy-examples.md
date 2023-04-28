# Amazon Pinpoint identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, users and roles don't have permission to create or modify Amazon Pinpoint resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or an AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the resources that they need\. The administrator must then attach those policies to the users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the Amazon Pinpoint console](#permissions-actions-examples-console-readonly)
+ [Example: Accessing a single Amazon Pinpoint project](#security_iam_id-based-policy-examples-access-one-project)
+ [Example: Viewing Amazon Pinpoint resources based on tags](#security_iam_id-based-policy-examples-view-resource-tags)
+ [Example: Allowing users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Examples: Providing access to Amazon Pinpoint API actions](#permissions-actions-examples-pin-api)
+ [Examples: Providing access to Amazon Pinpoint SMS and voice API actions](#permissions-actions-examples-pin-sms-voice-api)
+ [Example: Restricting Amazon Pinpoint project access to specific IP addresses](#security_iam_resource-based-policy-examples-restrict-project-access-by-ip)
+ [Example: Restricting Amazon Pinpoint access based on tags](#security_iam_resource-based-policy-examples-restrict-access-by-tag)
+ [Example: Allow Amazon Pinpoint to send email using identities that were verified in Amazon SES](#security_iam_resource-based-policy-examples-access-ses-identities)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete Amazon Pinpoint resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the Amazon Pinpoint console<a name="permissions-actions-examples-console-readonly"></a>

To access the Amazon Pinpoint console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the Amazon Pinpoint resources in your AWS account\. If you create an identity\-based policy that applies permissions that are more restrictive than the minimum required permissions, the console won't function as intended for entities \(users or roles\) with that policy\. To ensure that those entities can use the Amazon Pinpoint console, attach a policy to the entities\. For more information, see [Adding permissions to a user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

The following example policy provides read\-only access to the Amazon Pinpoint console in a specific AWS Region\. It includes read\-only access to other services that the Amazon Pinpoint console depends on, such as Amazon Simple Email Service \(Amazon SES\), IAM, and Amazon Kinesis\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "UseConsole",
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*"
             ],
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "firehose:ListDeliveryStreams",
                "iam:ListRoles",
                "kinesis:ListStreams",
                "s3:List*",
                "ses:Describe*",
                "ses:Get*",
                "ses:List*",
                "sns:ListTopics"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                }
            }
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of an AWS Region, and replace *accountId* with your AWS account ID\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that they're trying to perform\.

## Example: Accessing a single Amazon Pinpoint project<a name="security_iam_id-based-policy-examples-access-one-project"></a>

You can also create read\-only policies that provide access to only specific projects\. The following example policy lets users sign in to the console and view a list of projects\. It also lets users view information about related resources for other AWS services that the Amazon Pinpoint console depends on, such as Amazon SES, IAM, and Amazon Kinesis\. However, the policy lets users view additional information about only the project that's specified in the policy\. You can modify this policy to allow access to additional projects or AWS Regions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewProject",
            "Effect": "Allow",
            "Action": "mobiletargeting:GetApps",
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*"
            ],
            "Resource": [
                "arn:aws:mobiletargeting:region:accountId:apps/projectId",
                "arn:aws:mobiletargeting:region:accountId:apps/projectId/*",
                "arn:aws:mobiletargeting:region:accountId:reports"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ses:Get*",
                "kinesis:ListStreams",
                "firehose:ListDeliveryStreams",
                "iam:ListRoles",
                "ses:List*",
                "sns:ListTopics",
                "ses:Describe*",
                "s3:List*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                }
            }
        }
    ]
}
```

In the preceding example, replace *region* with the name of an AWS Region, replace *accountId* with your AWS account ID, and replace *projectId* with the ID of the Amazon Pinpoint project that you want to provide access to\.

Similarly, you can create policies that grant a  user in your AWS account with limited write access to one of your Amazon Pinpoint projects, for example the project that has the `810c7aab86d42fb2b56c8c966example` project ID\. In this case, you want to allow the user to view, add, and update project components, such as segments and campaigns, but not delete any components\.

In addition to granting permissions for `mobiletargeting:Get` and `mobiletargeting:List` actions, create a policy that grants permissions for the following actions: `mobiletargeting:Create`; `mobiletargeting:Update`; and `mobiletargeting:Put`\. These are the additional permissions required to create and manage most project components\. For example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "LimitedWriteProject",
            "Effect": "Allow",
            "Action": "mobiletargeting:GetApps",
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*",
                "mobiletargeting:Create*",
                "mobiletargeting:Update*",
                "mobiletargeting:Put*"
            ],
            "Resource": [
                "arn:aws:mobiletargeting:region:accountId:apps/810c7aab86d42fb2b56c8c966example",
                "arn:aws:mobiletargeting:region:accountId:apps/810c7aab86d42fb2b56c8c966example/*",
                "arn:aws:mobiletargeting:region:accountId:reports"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ses:Get*",
                "kinesis:ListStreams",
                "firehose:ListDeliveryStreams",
                "iam:ListRoles",
                "ses:List*",
                "sns:ListTopics",
                "ses:Describe*",
                "s3:List*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                }
            }
        }
    ]
}
```

## Example: Viewing Amazon Pinpoint resources based on tags<a name="security_iam_id-based-policy-examples-view-resource-tags"></a>

You can use conditions in an identity\-based policy to control access to Amazon Pinpoint resources based on tags\. This example policy shows how you might create this kind of policy to allow viewing Amazon Pinpoint resources\. However, permission is granted only if the `Owner` resource tag has the value of that user's user name\. This policy also grants the permissions necessary to complete this action on the console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListResources",
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ViewResourceIfOwner",
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*"
            ],
            "Resource": "arn:aws:mobiletargeting:*:*:*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Owner": "userName"
                },
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                },
                "ArnLike": {
                    "aws:SourceArn": "arn:aws:mobiletargeting:region:accountId:*"
                }
            }
        }
    ]
}
```

You can attach this type of policy to the users in your account\. If a user named `richard-roe` attempts to view an Amazon Pinpoint resource, the resource must be tagged `Owner=richard-roe` or `owner=richard-roe`\. Otherwise he is denied access\. The condition tag key `Owner` matches both `Owner` and `owner` because condition key names are not case\-sensitive\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Example: Allowing users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Examples: Providing access to Amazon Pinpoint API actions<a name="permissions-actions-examples-pin-api"></a>

This section provides example policies that allow access to features that are available from the Amazon Pinpoint API, which is the primary API for Amazon Pinpoint\. To learn more about this API, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.

### Read\-only access<a name="permissions-actions-examples-readonly"></a>

The following example policy allows read\-only access to all the resources in your Amazon Pinpoint account in a specific AWS Region\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewAllResources",
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:Get*",
                "mobiletargeting:List*"
            ],
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        }
    ]
}
```

In the preceding example, replace *region* with the name of an AWS Region, and replace *accountId* with your AWS account ID\.

### Administrator access<a name="permissions-actions-examples-admin"></a>

The following example policy allows full access to all Amazon Pinpoint actions and resources in your Amazon Pinpoint account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccess",
            "Effect": "Allow",
            "Action": [
                "mobiletargeting:*"
            ],
            "Resource": "arn:aws:mobiletargeting:region:accountId:*"
        }
    ]
}
```

In the preceding example, replace *accountId* with your AWS account ID\.

## Examples: Providing access to Amazon Pinpoint SMS and voice API actions<a name="permissions-actions-examples-pin-sms-voice-api"></a>

This section provides example policies that allow access to features that are available from the Amazon Pinpoint SMS and Voice API\. This is a supplemental API that provides advanced options for using and managing the SMS and voice channels in Amazon Pinpoint\. To learn more about this API, see the [Amazon Pinpoint SMS and voice API reference](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)\.

### Read\-only access<a name="permissions-actions-examples-pin-sms-voice-api-readonly"></a>

The following example policy allows read\-only access to all Amazon Pinpoint SMS and Voice API actions and resources in your AWS account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SMSVoiceReadOnly",
            "Effect": "Allow",
            "Action": [
                "sms-voice:Get*",
                "sms-voice:List*"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                },
                "ArnLike": {
                    "aws:SourceArn": "arn:aws::sms-voice:region:accountId:*"
                }
            }
        }
    ]
}
```

### Administrator access<a name="permissions-actions-examples-pin-sms-voice-api-admin"></a>

The following example policy allows full access to all Amazon Pinpoint SMS and Voice API actions and resources in your AWS account:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SMSVoiceFullAccess",
            "Effect": "Allow",
            "Action": [
                "sms-voice:*",
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "accountId"
                },
                "ArnLike": {
                    "aws:SourceArn": "arn:aws::sms-voice:region:accountId:*"
                }
            }
        }
    ]
}
```

## Example: Restricting Amazon Pinpoint project access to specific IP addresses<a name="security_iam_resource-based-policy-examples-restrict-project-access-by-ip"></a>

The following example policy grants permissions to any user to perform any Amazon Pinpoint action on a specified project \(*projectId*\)\. However, the request must originate from the range of IP addresses that are specified in the condition\.

The condition in this statement identifies the `54.240.143.*` range of allowed Internet Protocol version 4 \(IPv4\) addresses, with one exception: `54.240.143.188`\. The `Condition` block uses the `IpAddress` and `NotIpAddress` conditions and the `aws:SourceIp` condition key, which is an AWS\-wide condition key\. For more information about these condition keys, see [Specifying conditions in a policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/amazon-s3-policy-keys.html) *IAM User Guide*\. The `aws:SourceIp` IPv4 values use standard CIDR notation\. For more information, see [IP address condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IPAddress) in the *IAM User Guide*\.

```
{
    "Version":"2012-10-17",
    "Id":"AMZPinpointPolicyId1",
    "Statement":[
        {
            "Sid":"IPAllow",
            "Effect":"Allow",
            "Principal":"*",
            "Action":"mobiletargeting:*",
            "Resource":[
                "arn:aws:mobiletargeting:region:accountId:apps/projectId",
                "arn:aws:mobiletargeting:region:accountId:apps/projectId/*"
            ],
            "Condition":{
                "IpAddress":{
                    "aws:SourceIp":"54.240.143.0/24"
                },
                "NotIpAddress":{
                    "aws:SourceIp":"54.240.143.188/32"
                }
            }
        }
    ]
}
```

## Example: Restricting Amazon Pinpoint access based on tags<a name="security_iam_resource-based-policy-examples-restrict-access-by-tag"></a>

The following example policy grants permissions to perform any Amazon Pinpoint action on a specified project \(*projectId*\)\. However, permissions are granted only if the request originates from a user whose name is a value in the `Owner` resource tag for the project, as specified in the condition\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ModifyResourceIfOwner",
            "Effect": "Allow",
            "Action": "mobiletargeting:*",
            "Resource": [
                "arn:aws:mobiletargeting:region:accountId:apps/projectId",
                "arn:aws:mobiletargeting:region:accountId:apps/projectId/*"
                ],
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Owner": "userName"
                }
            }
        }
    ]
}
```

## Example: Allow Amazon Pinpoint to send email using identities that were verified in Amazon SES<a name="security_iam_resource-based-policy-examples-access-ses-identities"></a>

When you verify an email identity \(such as an email address or domain\) through the Amazon Pinpoint console, that identity is automatically configured so that it can be used by both Amazon Pinpoint and Amazon SES\. However, if you verify an email identity through Amazon SES, and you want to use that identity with Amazon Pinpoint, you must apply a policy to that identity\.

The following example policy grants Amazon Pinpoint permission to send email using an email identity that was verified through Amazon SES\.

```
{
    "Version":"2008-10-17",
    "Statement":[
        {
            "Sid":"PinpointEmail",
            "Effect":"Allow",
            "Principal":{
                "Service":"pinpoint.amazonaws.com"
            },
            "Action":"ses:*",
            "Resource":"arn:aws:ses:region:accountId:identity/emailId",
            "Condition":{
                "StringEquals":{
                    "aws:SourceAccount":"accountId"
                },
                "StringLike":{
                    "aws:SourceArn":"arn:aws:mobiletargeting:region:accountId:apps/*"
                }
            }
        }
    ]
}
```

If you use Amazon Pinpoint in the AWS GovCloud \(US\-West\) Region, use the following policy example instead:

```
{
    "Version":"2008-10-17",
    "Statement":[
        {
            "Sid":"PinpointEmail",
            "Effect":"Allow",
            "Principal":{
                "Service":"pinpoint.amazonaws.com"
            },
            "Action":"ses:*",
            "Resource":"arn:aws-us-gov:ses:us-gov-west-1:accountId:identity/emailId",
            "Condition":{
                "StringEquals":{
                    "aws:SourceAccount":"accountId"
                },
                "StringLike":{
                    "aws:SourceArn":"arn:aws-us-gov:mobiletargeting:us-gov-west-1:accountId:apps/*"
                }
            }
        }
    ]
}
```
# Amazon Pinpoint Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify Amazon Pinpoint resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or an AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the resources that they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy Best Practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the Amazon Pinpoint Console](#permissions-actions-examples-console-readonly)
+ [Example: Accessing a Single Amazon Pinpoint Project](#security_iam_id-based-policy-examples-access-one-project)
+ [Example: Viewing Amazon Pinpoint Resources Based on Tags](#security_iam_id-based-policy-examples-view-resource-tags)
+ [Example: Allowing Users to View Their Own Permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Examples: Providing Access to Amazon Pinpoint API Actions](#permissions-actions-examples-pin-api)
+ [Examples: Providing Access to Amazon Pinpoint SMS and Voice API Actions](#permissions-actions-examples-pin-sms-voice-api)

## Policy Best Practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete Amazon Pinpoint resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get Started Using AWS Managed Policies** – To start using Amazon Pinpoint quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get Started Using Permissions With AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant Least Privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for Sensitive Operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use Policy Conditions for Extra Security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Using the Amazon Pinpoint Console<a name="permissions-actions-examples-console-readonly"></a>

To access the Amazon Pinpoint console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the Amazon Pinpoint resources in your AWS account\. If you create an identity\-based policy that applies permissions that are more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\. To ensure that those entities can use the Amazon Pinpoint console, attach a policy to the entities\. For more information, see [Adding Permissions to a User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

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
            "Resource": "*"
        }
    ]
}
```

In the preceding policy example, replace *region* with the name of an AWS Region, and replace *accountId* with your AWS account ID\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that they're trying to perform\.

## Example: Accessing a Single Amazon Pinpoint Project<a name="security_iam_id-based-policy-examples-access-one-project"></a>

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
            "Resource": "*"
        }
    ]
}
```

In the preceding example, replace *region* with the name of an AWS Region, replace *accountId* with your AWS account ID, and replace *projectId* with the ID of the Amazon Pinpoint project that you want to provide access to\.

Similarly, you can create policies that grant an IAM user in your AWS account with limited write access to one of your Amazon Pinpoint projects, for example the project that has the `810c7aab86d42fb2b56c8c966example` project ID\. In this case, you want to allow the user to view, add, and update project components, such as segments and campaigns, but not delete any components\.

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
            "Resource": "*"
        }
    ]
}
```

## Example: Viewing Amazon Pinpoint Resources Based on Tags<a name="security_iam_id-based-policy-examples-view-resource-tags"></a>

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
                "StringEquals": {"aws:ResourceTag/Owner": "${aws:username}"}
            }
        }
    ]
}
```

You can attach this type of policy to the IAM users in your account\. If a user named `richard-roe` attempts to view an Amazon Pinpoint resource, the resource must be tagged `Owner=richard-roe` or `owner=richard-roe`\. Otherwise he is denied access\. The condition tag key `Owner` matches both `Owner` and `owner` because condition key names are not case\-sensitive\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Example: Allowing Users to View Their Own Permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

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

## Examples: Providing Access to Amazon Pinpoint API Actions<a name="permissions-actions-examples-pin-api"></a>

This section provides example policies that allow access to features that are available from the Amazon Pinpoint API, which is the primary API for Amazon Pinpoint\. To learn more about this API, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.

### Read\-Only Access<a name="permissions-actions-examples-readonly"></a>

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

### Administrator Access<a name="permissions-actions-examples-admin"></a>

The following example policy allows full access to all Amazon Pinpoint actions and resources in your Amazon Pinpoint account in all AWS Regions:

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
            "Resource": "arn:aws:mobiletargeting:*:accountId:*"
        }
    ]
}
```

In the preceding example, replace *accountId* with your AWS account ID\.

## Examples: Providing Access to Amazon Pinpoint SMS and Voice API Actions<a name="permissions-actions-examples-pin-sms-voice-api"></a>

This section provides example policies that allow access to features that are available from the Amazon Pinpoint SMS and Voice API\. This is a supplemental API that provides advanced options for using and managing the SMS and voice channels in Amazon Pinpoint\. To learn more about this API, see the [Amazon Pinpoint SMS and Voice API Reference](https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/)\.

### Read\-Only Access<a name="permissions-actions-examples-pin-sms-voice-api-readonly"></a>

The following example policy allows read\-only access to all Amazon Pinpoint SMS and Voice API actions and resources in your AWS account in all AWS Regions:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewAllResources",
            "Effect": "Allow",
            "Action": [
                "sms-voice:Get*",
                "sms-voice:List*"
            ],

            "Resource": "*"
        }
    ]
}
```

### Administrator Access<a name="permissions-actions-examples-pin-sms-voice-api-admin"></a>

The following example policy allows full access to all Amazon Pinpoint SMS and Voice API actions and resources in your AWS account in all AWS Regions:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccess",
            "Effect": "Allow",
            "Action": [
                "sms-voice:*"
            ],

            "Resource": "*"
        }
    ]
}
```
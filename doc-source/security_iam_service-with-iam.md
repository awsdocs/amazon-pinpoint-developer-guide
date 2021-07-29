# How Amazon Pinpoint works with IAM<a name="security_iam_service-with-iam"></a>

To use Amazon Pinpoint, users in your AWS account require permissions that allow them to view analytics data, create projects, define user segments, deploy campaigns, and more\. If you integrate a mobile or web app with Amazon Pinpoint, users of your app also require access to Amazon Pinpoint\. This access enables your app to register endpoints and report usage data to Amazon Pinpoint\. To grant access to Amazon Pinpoint features, create AWS Identity and Access Management \(IAM\) policies that allow Amazon Pinpoint actions for IAM identities or Amazon Pinpoint resources\.

IAM is a service that helps administrators securely control access to AWS resources\. IAM policies include statements that allow or deny specific actions by specific users or for specific resources\. Amazon Pinpoint provides a [set of actions](permissions-actions.md) that you can use in IAM policies to specify granular permissions for Amazon Pinpoint users and resources\. This means that you can grant the appropriate level of access to Amazon Pinpoint without creating overly permissive policies that might expose important data or compromise your resources\. For example, you can grant unrestricted access to an Amazon Pinpoint administrator, and grant read\-only access to individuals who need access to only a specific project\.

Before you use IAM to manage access to Amazon Pinpoint, you should understand what IAM features are available for use with Amazon Pinpoint\. To get a high\-level view of how Amazon Pinpoint and other AWS services work with IAM, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Amazon Pinpoint identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [Amazon Pinpoint resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on Amazon Pinpoint tags](#security_iam_service-with-iam-tags)
+ [Amazon Pinpoint IAM roles](#security_iam_service-with-iam-roles)

## Amazon Pinpoint identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. Amazon Pinpoint supports specific actions, resources, and condition keys\. To learn about all the elements that you can use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

This means that policy actions control what users can do on the Amazon Pinpoint console\. They also control what users can do programmatically by using the AWS SDKs, the AWS Command Line Interface \(AWS CLI\), or the Amazon Pinpoint APIs directly\.

Policy actions in Amazon Pinpoint use the following prefixes:
+ **`mobiletargeting`** – For actions that derive from the Amazon Pinpoint API, which is the primary API for Amazon Pinpoint\.
+ **`sms-voice`** – For actions that derive from the Amazon Pinpoint SMS and Voice API, which is a supplemental API that provides advanced options for using and managing the SMS and voice channels in Amazon Pinpoint\.

For example, to grant someone permission to view information about all the segments for a project, which is an action that corresponds to the `GetSegments` operation in the Amazon Pinpoint API, include the `mobiletargeting:GetSegments` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. Amazon Pinpoint defines its own set of actions that describe the tasks that users can perform with it\.

To specify multiple actions in a single statement, separate them with commas:

```
"Action": [
      "mobiletargeting:action1",
      "mobiletargeting:action2"
```

You can also specify multiple actions by using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Get`, include the following action:

```
"Action": "mobiletargeting:Get*"
```

However, as a best practice, you should create policies that follow the principle of *least privilege*\. In other words, you should create policies that include only the permissions that are required to perform a specific action\.

For a list of Amazon Pinpoint actions that you can use in IAM policies, see [Amazon Pinpoint actions for IAM policies](permissions-actions.md)\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



For example, the `mobiletargeting:GetSegments` action retrieves information about all the segments that are associated with a specific Amazon Pinpoint project\. You identify a project with an ARN in the following format:



```
arn:aws:mobiletargeting:${Region}:${Account}:apps/${projectId}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS General Reference*\.

In IAM policies, you can specify ARNs for the following types of Amazon Pinpoint resources:
+ Campaigns
+ Journeys
+ Message templates \(referred to as *templates* in some contexts\)
+ Projects \(referred to as *apps* or *applications* in some contexts\)
+ Recommender models \(referred to as *recommenders* in some contexts\)
+ Segments

For example, to create a policy statement for the project that has the project ID `810c7aab86d42fb2b56c8c966example`, use the following ARN:

```
"Resource": "arn:aws:mobiletargeting:us-east-1:123456789012:apps/810c7aab86d42fb2b56c8c966example"
```

To specify all the projects that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:mobiletargeting:us-east-1:123456789012:apps/*"
```

Some Amazon Pinpoint actions, such as certain actions for creating resources, can't be performed on a specific resource\. In those cases, you must use the wildcard \(\*\):

```
"Resource": "*"
```

Some Amazon Pinpoint API actions involve multiple resources\. For example, the `TagResource` action can add a tag to multiple projects\. To specify multiple resources in a single statement, separate the ARNs with commas:

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of Amazon Pinpoint resource types and their ARNs, see [Resources Defined by Amazon Pinpoint](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonpinpoint.html#amazonpinpoint-resources-for-iam-policies) in the *IAM User Guide*\. To learn which actions you can specify with the ARN of each resource type, see [Actions Defined by Amazon Pinpoint](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonpinpoint.html#amazonpinpoint-actions-as-permissions) in the *IAM User Guide*\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



Amazon Pinpoint defines its own set of condition keys and also supports some global condition keys\. To see a list of all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\. To see a list of Amazon Pinpoint condition keys, see [Condition Keys for Amazon Pinpoint](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonpinpoint.html#amazonpinpoint-policy-keys) in the *IAM User Guide*\. To learn which actions and resources you can use a condition key with, see [Actions Defined by Amazon Pinpoint](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonpinpoint.html#amazonpinpoint-actions-as-permissions) in the *IAM User Guide*\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Amazon Pinpoint identity\-based policies, see [Amazon Pinpoint identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## Amazon Pinpoint resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on an Amazon Pinpoint resource and under what conditions\. Amazon Pinpoint supports resource\-based permissions policies for campaigns, journeys, message templates \(*templates*\), recommender models \(*recommenders*\), projects \(*apps*\), and segments\. Resource\-based policies let you grant usage permission to other accounts on a per\-resource basis\. You can also use a resource\-based policy to allow another AWS service to access these types of Amazon Pinpoint resources\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the [principal in a resource\-based policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, you must also grant the principal entity permission to access the resource\. Grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM roles differ from resource\-based policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.

### Examples<a name="security_iam_service-with-iam-resource-based-policies-examples"></a>



To view examples of Amazon Pinpoint resource\-based policies, see [Amazon Pinpoint identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## Authorization based on Amazon Pinpoint tags<a name="security_iam_service-with-iam-tags"></a>

You can associate tags with certain types of Amazon Pinpoint resources or pass tags in a request to Amazon Pinpoint\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `aws:ResourceTag/${TagKey}`, `aws:RequestTag/${TagKey}`, or `aws:TagKeys` condition keys\.

For information about tagging Amazon Pinpoint resources, including an example IAM policy, see [Tagging Amazon Pinpoint resources](tagging-resources.md)\.

## Amazon Pinpoint IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with Amazon Pinpoint<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or assume a cross\-account role\. You obtain temporary security credentials by calling AWS Security Token Service \(AWS STS\) API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

Amazon Pinpoint supports using temporary credentials\.

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

Amazon Pinpoint doesn't use service\-linked roles\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

Amazon Pinpoint supports using service roles\.
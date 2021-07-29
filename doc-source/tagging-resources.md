# Tagging Amazon Pinpoint resources<a name="tagging-resources"></a>

A *tag* is a label that you optionally define and associate with AWS resources, including certain types of Amazon Pinpoint resources\. Tags can help you categorize and manage resources in different ways, such as by purpose, owner, environment, or other criteria\. For example, you can use tags to apply policies or automation, or to identify resources that are subject to certain compliance requirements\. You can add tags to the following types of Amazon Pinpoint resources:
+ Campaigns
+ Message templates
+ Projects \(applications\)
+ Segments

A resource can have as many as 50 tags\.

## Managing tags<a name="tags-manage"></a>

Each tag consists of a required *tag key* and an optional *tag value*, both of which you define\. A *tag key* is a general label that acts as a category for more specific tag values\. A *tag value* acts as a descriptor for a tag key\. For example, if you have two versions of an Amazon Pinpoint project \(one for internal testing and another for external use\), you might assign a `Stack` tag key to both projects\. The value of the `Stack` tag key might be `Test` for one version of the project and `Production` for the other version\.

A tag key can contain as many as 128 characters\. A tag value can contain as many as 256 characters\. The characters can be Unicode letters, numbers, white space, or one of the following symbols: \_ \. : / = \+ \-\. The following additional restrictions apply to tags:
+ Tag keys and values are case sensitive\.
+ For each associated resource, each tag key must be unique and it can have only one value\.
+ The `aws:` prefix is reserved for use by AWS; you can’t use it in any tag keys or values that you define\. In addition, you can't edit or remove tag keys or values that use this prefix\. Tags that use this prefix don’t count against the quota of 50 tags per resource\.
+ You can't update or delete a resource based only on its tags\. You must also specify the Amazon Resource Name \(ARN\) or resource ID, depending on the operation that you use\.
+ You can associate tags with public or shared resources\. However, the tags are available only for your AWS account, not any other accounts that share the resource\. In addition, the tags are available only for resources that are located in the specified AWS Region for your AWS account\.

To add, display, update, and remove tag keys and values from Amazon Pinpoint resources, you can use the AWS Command Line Interface \(AWS CLI\), the Amazon Pinpoint API, the AWS Resource Groups Tagging API, or an AWS SDK\. To manage tag keys and values across all the AWS resources that are located in a specific AWS Region for your AWS account \(including Amazon Pinpoint resources\), use the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\.

## Using tags in IAM policies<a name="tags-iam"></a>

After you start implementing tags, you can apply tag\-based, resource\-level permissions to AWS Identity and Access Management \(IAM\) policies and API operations\. This includes operations that support adding tags to resources when resources are created\. By using tags in this way, you can implement granular control of which groups and users in your AWS account have permission to create and tag resources, and which groups and users have permission to create, update, and remove tags more generally\.

For example, you can create a policy that allows a user to have full access to all the Amazon Pinpoint resources where their name is a value in the `Owner` tag for the resource:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "ModifyResourceIfOwner",
         "Effect": "Allow",
         "Action": "mobiletargeting:*",
         "Resource": "*",
         "Condition": {
            "StringEqualsIgnoreCase": {
               "aws:ResourceTag/Owner": "${aws:username}"
            }
         }
      }
   ]
}
```

If you define tag\-based, resource\-level permissions, the permissions take effect immediately\. This means that your resources are more secure as soon as they're created, and you can quickly start enforcing the use of tags for new resources\. You can also use resource\-level permissions to control which tag keys and values can be associated with new and existing resources\. For more information, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *AWS IAM User Guide*\.

## Adding tags to resources<a name="tags-add"></a>

The following examples show how to add a tag to an Amazon Pinpoint resource by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\. You can also use any supported AWS SDK to add a tag to a resource\.

To add a tag to multiple Amazon Pinpoint resources in a single operation, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\.

------
#### [ AWS CLI ]

To create a new resource and add a tag to it by using the AWS CLI, use the appropriate `create` command for the resource\. Include the `tags` parameter and values\. For example, the following command creates a new project named `ExampleCorp` and adds a `Stack` tag key with a `Test` tag value to the project:

```
C:\> aws pinpoint create-app ^
     --create-application-request ^
     --Name=ExampleCorp ^
     --tags={Stack=Test}
```

For information about the commands that you can use to create an Amazon Pinpoint resource, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

To add a tag to an existing resource, use the `tag-resource` command and specify the appropriate values for the required parameters:

```
C:\> aws pinpoint tag-resource ^
     --resource-arn resource-arn ^
     --tags-model tags={key=value}
```

Where:
+ *resource\-arn* is the Amazon Resource Name \(ARN\) of the resource that you want to add a tag to\.
+ *key* is the tag key that you want to add to the resource\. The `key` argument is required\.
+ *value* is the optional tag value that you want to add for the specified tag key \(`key`\)\. The `value` argument is required\. If you don’t want the resource to have a specific tag value, don’t specify a value for the `value` argument\. Amazon Pinpoint sets the value to an empty string\.

------
#### [ REST API ]

To create a new resource and add a tag to it by using the Amazon Pinpoint REST API, send a POST request to the appropriate resource URI\. Include the `tags` parameter and values in the body of the request\. For example, the following request creates a new project named `ExampleCorp` and adds a `Stack` tag key with a `Test` tag value to the project:

```
POST /v1/apps HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Cache-Control: no-cache

{
   "Name":"ExampleCorp",
   "tags":{
      "Stack":"Test"
   }
}
```

To add a tag to an existing resource, send a POST request to the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) URI\. Include the Amazon Resource Name \(ARN\) of the resource in the URI\. The ARN should be URL encoded\. In the body of the request, include the `tags` parameter and values\. For example, the following request adds a `Stack` tag key with a `Test` tag value to the specified project \(*resource\-arn*\):

```
POST /v1/tags/resource-arn HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache

{
   "tags":{
      "Stack":"Test"
   }
}
```

Alternatively, you can send a PUT request to the appropriate resource URI and include the `tags` parameter and values in the body of the request\. For example, the following request adds a `Stack` tag key with a `Test` tag value to the specified campaign:

```
PUT /v1/apps/application-id/campaigns/campaign-id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Cache-Control: no-cache

{
   "tags":{
      "Stack":"Test"
   }
}
```

Where:
+ *application\-id* is the ID of the project that contains the campaign\.
+ *campaign\-id* is the ID of the campaign\.

Note that the Amazon Pinpoint API currently doesn’t support PUT requests for projects\. Therefore, you have to use the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) resource to add a tag to an existing project\. 

------

## Displaying tags for resources<a name="tags-display"></a>

The following examples show how to use the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/) to display a list of all the tags \(keys and values\) that are associated with an Amazon Pinpoint resource\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\. You can also use any supported AWS SDK to display all the tags that are associated with a resource\.

------
#### [ AWS CLI ]

 To use the AWS CLI to display a list of the tags that are associated with a specific resource, run the `list-tags-for-resource` command and specify the Amazon Resource Name \(ARN\) of the resource for the `resource-arn` parameter:

```
C:\> aws pinpoint list-tags-for-resource ^
     --resource-arn resource-arn
```

To display a list of all the Amazon Pinpoint resources that have tags, and all the tags that are associated with each of those resources, use the `get-resources` command of the AWS Resource Groups Tagging API\. Set the `resource-type-filters` parameter to `mobiletargeting`\. For example:

```
C:\> aws resourcegroupstaggingapi get-resources ^
     --resource-type-filters "mobiletargeting"
```

The output of the command is a list of ARNs for all the Amazon Pinpoint resources that have tags\. The list includes all the tag keys and values that are associated with each resource\.

------
#### [ REST API ]

To use the Amazon Pinpoint REST API to display all the tags that are associated with a specific resource, send a GET request to the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) URI and include the Amazon Resource Name \(ARN\) of the resource in the URI\. The ARN should be URL encoded\. For example, the following request retrieves all the tags that are associated with a specified campaign \(*resource\-arn*\):

```
GET /v1/tags/resource-arn HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache
```

The JSON response to the request includes a `tags` object\. The `tags` object lists all the tag keys and values that are associated with the campaign\.

To display all the tags that are associated with more than one resource of the same type, send a GET request to the appropriate URI for that type of resource\. For example, the following request retrieves information about all the campaigns in the specified project \(*application\-id*\):

```
GET /v1/apps/application-id/campaigns HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache
```

The JSON response to the request lists all the campaigns in the project\. The `tags` object of each campaign lists all the tag keys and values that are associated with the campaign\.

------

## Updating tags for resources<a name="tags-update"></a>

There are several ways to update \(overwrite\) a tag for an Amazon Pinpoint resource\. The best way to update a tag depends on:
+ The type of resource that you want to update tags for\.
+ Whether you want to update a tag for one or multiple resources at the same time\.
+ Whether you want to update a tag key, a tag value, or both\.

To update a tag for an Amazon Pinpoint project or for multiple resources at the same time, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\. The Amazon Pinpoint API currently doesn’t provide direct support for either of those tasks\.

To update a tag key for one resource, you can [remove the current tag](#tags-remove) and [add a new tag](#tags-add) by using the Amazon Pinpoint API\.

To update a tag value \(of a tag key\) for only one resource, you can use the Amazon Pinpoint API\. The following examples show how to do this by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\. You can also use any supported AWS SDK to update a tag value for a resource\.

------
#### [ AWS CLI ]

You can use the AWS CLI to update a tag value for a resource\.

To do this by specifying a resource ID, use the appropriate `update` command for the resource type\. Include the `tags` parameter and arguments\. For example, the following command changes the tag value from `Test` to `Production` for the `Stack` tag key that's associated with the specified campaign:

```
C:\> aws pinpoint update-campaign ^
     --application-id application-id ^
     --campaign-id campaign-id ^
     --write-campaign-request tags={Stack=Production}
```

Where:
+ *application\-id* is the ID of the project that contains the campaign\.
+ *campaign\-id* is the ID of the campaign whose tag you want to update\.

For information about the commands that you can use to update an Amazon Pinpoint resource, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

To update a tag value by specifying an Amazon Resource Name \(ARN\), run the `tag-resource` command and include the `tags-model` parameter and arguments:

```
C:\> aws pinpoint tag-resource ^
     --resource-arn resource-arn ^
     --tags-model tags={key=value}
```

Where:
+ *resource\-arn* is the ARN of the resource whose tag you want to update\. To get a list of ARNs for Amazon Pinpoint resources, you can [display a list of Amazon Pinpoint resources that have tags](#tags-display)\.
+ *key* is the tag key\. The `key` argument is required\.
+ *value* is the new tag value to use for the specified tag key \(`key`\)\. The `value` argument is required\. To remove the tag value, don’t specify a value for this argument\. Amazon Pinpoint sets the value to an empty string\.

------
#### [ REST API ]

You can use the Amazon Pinpoint REST API to update a tag value for a resource\. To do this, send a PUT request to the appropriate URI for the type of resource whose tag you want to update\. In the body of the request, include the `tags` parameter and values\. For the `tags` parameter, specify the corresponding tag key for the `tag` property\. For the `value` property, do one of the following:
+ To use a new value, specify the value\.
+ To remove the value, don’t specify a value\. Amazon Pinpoint sets the value to an empty string\.

For example, the following request changes the tag value from `Test` to `Production` for the `Stack` tag key that's associated with the specified campaign:

```
PUT /v1/apps/application-id/campaigns/campaign-id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache

{
    "tags": { 
        "Stack": "Production"
    }
}
```

Where:
+ *application\-id* is the ID of the project that contains the campaign\.
+ *campaign\-id* is the ID of the campaign whose tag you want to update\.

------

## Removing tags from resources<a name="tags-remove"></a>

The following examples show how to remove a tag \(both the key and value\) from an Amazon Pinpoint resource by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\. You can also use any supported AWS SDK to remove a tag from a resource\.

To remove a tag from multiple Amazon Pinpoint resources in a single operation, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\. To remove only a specific tag value \(not a tag key\) from a resource, [update the tag for the resource](#tags-update)\.

------
#### [ AWS CLI ]

To remove a tag from a resource by using the AWS CLI, run the `untag-resource` command\. Include the `tag-keys` parameter and argument:

```
C:\> aws pinpoint untag-resource ^
     --resource-arn resource-arn ^
     --tag-keys key
```

Where:
+ *resource\-arn* is the Amazon Resource Name \(ARN\) of the resource that you want to remove a tag from\. To get a list of ARNs for Amazon Pinpoint resources, you can [display a list of all Amazon Pinpoint resources that have tags](#tags-display)\.
+ *key* is the tag that you want to remove from the resource\. The `key` argument is required\.

To remove multiple tags from a resource, add each additional key as an argument for the `tag-keys` parameter:

```
C:\> aws pinpoint untag-resource ^
     --resource-arn resource-arn ^
     --tag-keys key1 key2
```

Where:
+ *resource\-arn* is the ARN of the resource that you want to remove tags from\.
+ *key\#* is each tag that you want to remove from the resource\.

------
#### [ REST API ]

To remove a tag from a resource by using the Amazon Pinpoint REST API, send a DELETE request to the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) URI\. In the URI, include the Amazon Resource Name \(ARN\) of the resource that you want to remove a tag from, followed by the `tagKeys` parameter and the tag to remove\. For example:

```
https://endpoint/v1/tags/resource-arn?tagKeys=key
```

 Where:
+ *endpoint* is the Amazon Pinpoint endpoint for the AWS Region that hosts the resource\.
+ *resource\-arn* is the ARN of the resource that you want to remove a tag from\.
+ *key* is the tag that you want to remove from the resource\.

All the parameters should be URL encoded\.

To remove multiple tag keys and their associated values from a resource, append the `tagKeys` parameter and argument for each additional tag to remove, separated by an ampersand \(&\)\. For example: 

```
https://endpoint/v1/tags/resource-arn?tagKeys=key1&tagKeys=key2
```

All the parameters should be URL encoded\.

------

## Related information<a name="tags-related-information"></a>

For more information about the CLI commands that you can use to manage Amazon Pinpoint resources, see the Amazon Pinpoint section of the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

For more information about resources in the Amazon Pinpoint API, including supported HTTP\(S\) methods, parameters, and schemas, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.
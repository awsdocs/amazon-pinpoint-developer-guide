# Tagging Amazon Pinpoint resources<a name="tagging-resources"></a>

A *tag* is a label that you optionally define and associate with AWS resources, including certain types of Amazon Pinpoint resources\. Tags can help you categorize and manage resources in different ways, such as by purpose, owner, environment, or other criteria\. For example, you can use tags to apply policies or automation, or to identify resources that are subject to certain compliance requirements\. You can add tags to the following types of Amazon Pinpoint resources:
+ Campaigns
+ Message templates
+ Projects \(applications\)
+ Segments

A resource can have as many as 50 tags\.

## Managing tags<a name="tags-manage"></a>

Each tag consists of a required *tag key* and an optional *tag value*, both of which you define\. A *tag key* is a general label that acts as a category for more specific tag values\. A *tag value* acts as a descriptor for a tag key\.

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

The following examples show how to add a tag to an Amazon Pinpoint resource by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. You can also use any supported AWS SDK to add a tag to a resource\.

To add a tag to multiple Amazon Pinpoint resources in a single operation, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\.

### Adding tags by using the API<a name="tags-add-api"></a>

To create a new resource and add a tag to it by using the Amazon Pinpoint REST API, send a POST request to the appropriate resource URI\. Include the `tags` parameter and values in the body of the request\. The following example shows how to specify a tag when you create a new project\.

```
POST /v1/apps HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Cache-Control: no-cache

{
   "Name":"MyProject",
   "tags":{
      "key1":"value1"
   }
}
```

To add a tag to an existing resource, send a POST request to the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) URI\. Include the Amazon Resource Name \(ARN\) of the resource in the URI\. The ARN should be URL encoded\. In the body of the request, include the `tags` parameter and values, as shown in the following example\.

```
POST /v1/tags/resource-arn HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache

{
   "tags":{
      "key1":"value1"
   }
}
```

Alternatively, you can send a PUT request to the appropriate resource URI and include the `tags` parameter and values in the body of the request, as shown in the following example\.

```
PUT /v1/apps/application-id/campaigns/campaign-id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Cache-Control: no-cache

{
   "tags":{
      "key1":"value1"
   }
}
```

Where:
+ *application\-id* is the ID of the project that contains the campaign\.
+ *campaign\-id* is the ID of the campaign\.

Note that the Amazon Pinpoint API currently doesn’t support PUT requests for projects\. Therefore, you have to use the [Tags](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-tags.html) resource to add a tag to an existing project\. 

### Adding tags by using the AWS CLI<a name="add-tags-cli"></a>

To create a new resource and add a tag to it by using the AWS CLI, use the appropriate `create` command for the resource\. Include the `tags` parameter and values\. The following example shows how to specify tags when you create a new project\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint create-app \
  --create-application-request '{
    "Name":"MyProject",
    "tags": {
      "key1":"value1",
      "key2":"value2"
    } 
  }'
```

------
#### [ Windows Command prompt ]

```
C:\> aws pinpoint create-app ^
     --create-application-request Name=MyProject,tags={key1=value1,key2=value2}
```

------

In the preceding example, do the following:
+ Replace *MyProject* with the name that you want to give the project\.
+ Replace *key1* and *key2* with the keys of the tags that you want to add to the resource\.
+ Replace *value1* and *value2* with the values of the tags that you want to add for the respective keys\.

For information about the commands that you can use to create an Amazon Pinpoint resource, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

To add a tag to an existing resource, use the `tag-resource` command and specify the appropriate values for the required parameters:

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint tag-resource \
  --resource-arn resource-arn \
  --tags-model '{
    "tags": {
      "key1":"value1",
      "key2":"value2"
    }
  }'
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint tag-resource ^
     --resource-arn resource-arn ^
     --tags-model tags={key1=value1,key2=value2}
```

------

In the preceding example, do the following:
+ Replace *resource\-arn* with the Amazon Resource Name \(ARN\) of the resource that you want to add a tag to\.
+ Replace *key1* and *key2* with the keys of the tags that you want to add to the resource\.
+ Replace *value1* and *value2* with the values of the tags that you want to add for the respective keys\.

## Displaying tags for resources<a name="tags-display"></a>

The following examples show how to use the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/) to display a list of all the tags \(keys and values\) that are associated with an Amazon Pinpoint resource\. You can also use any supported AWS SDK to display the tags associated with a resource\.

### Displaying tags by using the API<a name="tags-display-api"></a>

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

### Displaying tags by using the AWS CLI<a name="tags-display-cli"></a>

 To use the AWS CLI to display a list of the tags that are associated with a specific resource, run the `list-tags-for-resource` command and specify the Amazon Resource Name \(ARN\) of the resource for the `resource-arn` parameter, as shown in the following example\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint list-tags-for-resource \
  --resource-arn resource-arn
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint list-tags-for-resource ^
     --resource-arn resource-arn
```

------

To display a list of all the Amazon Pinpoint resources that have tags, and all the tags that are associated with each of those resources, use the [get\-resources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_GetResources.html) command of the AWS Resource Groups Tagging API\. Set the `resource-type-filters` parameter to `mobiletargeting`, as shown in the following example\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws resourcegroupstaggingapi get-resources \
     --resource-type-filters "mobiletargeting"
```

------
#### [ Windows Command Prompt ]

```
C:\> aws resourcegroupstaggingapi get-resources ^
     --resource-type-filters "mobiletargeting"
```

------

The output of the command is a list of ARNs for all the Amazon Pinpoint resources that have tags\. The list includes all the tag keys and values that are associated with each resource\.

## Updating tags for resources<a name="tags-update"></a>

There are several ways to update \(overwrite\) a tag for an Amazon Pinpoint resource\. The best way to update a tag depends on:
+ The type of resource that you want to update tags for\.
+ Whether you want to update a tag for one or multiple resources at the same time\.
+ Whether you want to update a tag key, a tag value, or both\.

To update a tag for an Amazon Pinpoint project or for multiple resources at the same time, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\. The Amazon Pinpoint API currently doesn’t provide direct support for either of those tasks\.

To update a tag key for one resource, you can [remove the current tag](#tags-remove) and [add a new tag](#tags-add) by using the Amazon Pinpoint API\.

To update a tag value \(of a tag key\) for only one resource, you can use the Amazon Pinpoint API\. The following examples show how to do this by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. The AWS CLI examples are formatted for Microsoft Windows\. You can also use any supported AWS SDK to update a tag value for a resource\.

### Updating tags by using the API<a name="tags-update-api"></a>

You can use the Amazon Pinpoint REST API to update a tag value for a resource\. To do this, send a PUT request to the appropriate URI for the type of resource whose tag you want to update\. In the body of the request, include the `tags` parameter and values\. For the `tags` parameter, specify the corresponding tag key for the `tag` property, as shown in the following example\.

```
PUT /v1/apps/application-id/campaigns/campaign-id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache

{
    "tags": { 
        "key1": "value1"
    }
}
```

Where:
+ *application\-id* is the ID of the project that contains the campaign\.
+ *campaign\-id* is the ID of the campaign whose tag you want to update\.

### Updating tags by using the AWS CLI<a name="tags-update-cli"></a>

You can use the AWS CLI to update a tag value for a resource\.

To do this by specifying a resource ID, use the appropriate `Update` command for the resource type\. Include the `tags` parameter and arguments\. For example, you can use the `UpdateCampaign` command to update the tags for a campaign, as shown in the following example\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint update-campaign \
  --application-id application-id \
  --campaign-id campaign-id \
  --write-campaign-request tags={key1=valueA}
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint update-campaign ^
     --application-id application-id ^
     --campaign-id campaign-id ^
     --write-campaign-request tags={key1=valueA}
```

------

In the preceding example, do the following:
+ Replace *application\-id* with the ID of the project that contains the campaign\.
+ Replace *campaign\-id* with the ID of the campaign that contains the tag that you want to update\.
+ Replace *key1* with the key that you want to update\.
+ Replace *valueA* with the new tag value to use for the specified tag key \(`key1`\)\.

For information about the commands that you can use to update an Amazon Pinpoint resource, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

To update a tag value by specifying an Amazon Resource Name \(ARN\), use the `tag-resource` command and include the `tags-model` parameter and arguments, as shown in the following example\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint tag-resource \
  --resource-arn resource-arn \
  --tags-model tags={key1=valueA}
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint tag-resource ^
     --resource-arn resource-arn ^
     --tags-model tags={key1=valueA}
```

------

In the preceding example, make the following changes:
+ Replace *resource\-arn* with the ARN of the resource whose tag you want to update\. To get a list of ARNs for Amazon Pinpoint resources, you can [display a list of Amazon Pinpoint resources that have tags](#tags-display)\.
+ Replace *key1* with the key that you want to update\.
+ Replace *valueA* with the new tag value to use for the specified tag key \(`key1`\)\.

## Removing tags from resources<a name="tags-remove"></a>

The following examples show how to remove a tag \(both the key and value\) from an Amazon Pinpoint resource by using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/) and the [Amazon Pinpoint REST API](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\. You can also use any supported AWS SDK to remove a tag from a resource\.

To remove a tag from multiple Amazon Pinpoint resources in a single operation, use the resource groups tagging operations of the AWS CLI or the [AWS Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/Welcome.html)\. To remove only a specific tag value \(not a tag key\) from a resource, [update the tag for the resource](#tags-update)\.

### Removing tags by using the API<a name="tags-remove-api"></a>

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

### Removing tags by using the AWS CLI<a name="tags-remove-cli"></a>

To remove a tag from a resource by using the AWS CLI, run the `untag-resource` command\. Include the `tag-keys` parameter and argument, as shown in the following example\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint untag-resource \
  --resource-arn resource-arn \
  --tag-keys key1 key2
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint untag-resource ^
     --resource-arn resource-arn ^
     --tag-keys key1 key2
```

------

In the preceding example, make the following changes:
+ Replace *resource\-arn* with the ARN of the resource that you want to remove tags from\.
+ Replace *key1* and *key2* with the keys of the tags that you want to remove from the resource\.

## Related information<a name="tags-related-information"></a>

For more information about the CLI commands that you can use to manage Amazon Pinpoint resources, see the Amazon Pinpoint section of the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/)\.

For more information about resources in the Amazon Pinpoint API, including supported HTTP\(S\) methods, parameters, and schemas, see the [Amazon Pinpoint API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference/)\.
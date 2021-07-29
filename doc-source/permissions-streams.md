# IAM role for streaming events to Kinesis<a name="permissions-streams"></a>

Amazon Pinpoint can automatically send app usage data, or *event data*, from your app to an Amazon Kinesis data stream or Amazon Kinesis Data Firehose delivery stream in your AWS account\. Before Amazon Pinpoint can begin streaming the event data, you must delegate the required permissions to Amazon Pinpoint\. 

If you use the console to set up event streaming, Amazon Pinpoint automatically creates an AWS Identity and Access Management \(IAM\) role with the required permissions\. For more information, see [Streaming Amazon Pinpoint events to amazon Kinesis](https://docs.aws.amazon.com/pinpoint/latest/userguide/analytics-streaming-kinesis.html) in the *Amazon Pinpoint User Guide*\.

If you want to create the role manually, attach the following policies to the role: 
+ A permissions policy that allows Amazon Pinpoint to send event data to your stream\.
+ A trust policy that allows Amazon Pinpoint to assume the role\.

After you create the role, you can configure Amazon Pinpoint to automatically send events to your stream\. For more information, see [Streaming Amazon Pinpoint events to Kinesis](event-streams.md) in this guide\.

## Permissions policies<a name="permissions-streams-permissionspolicies"></a>

To allow Amazon Pinpoint to send event data to your stream, attach one of the following policies to the role\.

### Amazon Kinesis Data Streams<a name="permissions-streams-permissionspolicies-aks"></a>

The following policy allows Amazon Pinpoint to send event data to a Kinesis stream\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Action": [
            "kinesis:PutRecords",
            "kinesis:DescribeStream"
        ],
        "Effect": "Allow",
        "Resource": [
            "arn:aws:kinesis:region:account-id:stream/stream-name"
        ]
    }
}
```

### Amazon Kinesis Data Firehose<a name="permissions-streams-permissionspolicies-akf"></a>

The following policy allows Amazon Pinpoint to send event data to a Kinesis Data Firehose delivery stream\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
        	"firehose:PutRecordBatch",
        	"firehose:DescribeDeliveryStream"
        ],
        "Resource": [
        	"arn:aws:firehose:region:account-id:deliverystream/delivery-stream-name"
    	]
    }
}
```

## Trust policy<a name="permissions-streams-trustpolicy"></a>

To allow Amazon Pinpoint to assume the IAM role and perform the actions allowed by the permissions policy, attach the following trust policy to the role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "pinpoint.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Creating the IAM role \(AWS CLI\)<a name="permissions-streams-create"></a>

Complete the following steps to create the IAM role by using the AWS Command Line Interface \(AWS CLI\)\. To learn how to create the role by using the Amazon Pinpoint console, see [Streaming Amazon Pinpoint events to Kinesis](https://docs.aws.amazon.com/pinpoint/latest/userguide/analytics-streaming-kinesis.html#analytics-streaming-kinesis-setup) in the *Amazon Pinpoint User Guide*\.

If you haven't installed the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**To create the IAM role by using the AWS CLI**

1. Create a JSON file that contains the trust policy for your role, and save the file locally\. You can copy the trust policy provided in this topic\.

1. Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the role and attach the trust policy:

   ```
   aws iam create-role --role-name PinpointEventStreamRole --assume-role-policy-document file://PinpointEventStreamTrustPolicy.json
   ```

   Following the `file://` prefix, specify the path to the JSON file that contains the trust policy\.

   After you run this command, the AWS CLI prints the following output in your terminal:

   ```
   {
       "Role": {
           "AssumeRolePolicyDocument": {
               "Version": "2012-10-17", 
               "Statement": [
                   {
                       "Action": "sts:AssumeRole", 
                       "Effect": "Allow", 
                       "Principal": {
                           "Service": "pinpoint.amazonaws.com"
                       }
                   }
               ]
           }, 
           "RoleId": "AIDACKCEVSQ6C2EXAMPLE", 
           "CreateDate": "2017-02-28T18:02:48.220Z", 
           "RoleName": "PinpointEventStreamRole", 
           "Path": "/", 
           "Arn": "arn:aws:iam::111122223333:role/PinpointEventStreamRole"
       }
   }
   ```

1. Create a JSON file that contains the permissions policy for your role, and save the file locally\. You can copy one of the policies provided in the [Permissions policies](#permissions-streams-permissionspolicies) section of this topic\.

1. Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html) command to attach the permissions policy to the role:

   ```
   aws iam put-role-policy --role-name PinpointEventStreamRole --policy-name PinpointEventStreamPermissionsPolicy --policy-document file://PinpointEventStreamPermissionsPolicy.json
   ```

   Following the `file://` prefix, specify the path to the JSON file that contains the permissions policy\.
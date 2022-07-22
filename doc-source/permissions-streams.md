# IAM role for streaming events to Kinesis<a name="permissions-streams"></a>

Amazon Pinpoint can automatically send app usage data, or *event data*, from your app to an Amazon Kinesis data stream or Amazon Kinesis Data Firehose delivery stream in your AWS account\. Before Amazon Pinpoint can begin streaming the event data, you must delegate the required permissions to Amazon Pinpoint\. 

If you use the console to set up event streaming, Amazon Pinpoint automatically creates an AWS Identity and Access Management \(IAM\) role with the required permissions\. For more information, see [Streaming Amazon Pinpoint events to amazon Kinesis](https://docs.aws.amazon.com/pinpoint/latest/userguide/analytics-streaming-kinesis.html) in the *Amazon Pinpoint User Guide*\.

If you want to create the role manually, attach the following policies to the role: 
+ A permissions policy that allows Amazon Pinpoint to send event data to your stream\.
+ A trust policy that allows Amazon Pinpoint to assume the role\.

After you create the role, you can configure Amazon Pinpoint to automatically send events to your stream\. For more information, see [Streaming Amazon Pinpoint events to Kinesis](event-streams.md) in this guide\.

## Creating the IAM role \(AWS CLI\)<a name="permissions-streams-create"></a>

Complete the following steps to manually create an IAM role by using the AWS Command Line Interface \(AWS CLI\)\. To learn how to create the role by using the Amazon Pinpoint console, see [Streaming Amazon Pinpoint events to Kinesis](https://docs.aws.amazon.com/pinpoint/latest/userguide/analytics-streaming-kinesis.html#analytics-streaming-kinesis-setup) in the *Amazon Pinpoint User Guide*\.

If you haven't installed the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**To create the IAM role by using the AWS CLI**

1. Create a new file\. Paste the following policy into the document:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": "pinpoint.amazonaws.com"
               },
               "Action": "sts:AssumeRole",
               "Condition": {
                   "StringEquals": {
                       "aws:SourceAccount": "accountId"
                   },
                   "ArnLike": {
                       "aws:SourceArn": "arn:aws:mobiletargeting:region:accountId:apps/applicationId"
                   }
               }
           }
       ]
   }
   ```

   When you finish, save the file as `PinpointEventStreamTrustPolicy.json`\.

1. Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the role and attach the trust policy:

   ```
   aws iam create-role --role-name PinpointEventStreamRole --assume-role-policy-document file://PinpointEventStreamTrustPolicy.json
   ```

1. Create a new file that contains the permissions policy for your role\.

   If you are configuring Amazon Pinpoint to send data to an Kinesis stream, paste the following policy into the file:

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
               "arn:aws:kinesis:region:accountId:stream/streamName"
           ]
       }
   }
   ```

   Alternatively, if you are configuring Amazon Pinpoint to send data to an Kinesis Data Firehose stream, paste the following policy into the file:

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
           	"arn:aws:firehose:region:accountId:deliverystream/delivery-stream-name"
       	]
       }
   }
   ```

   When you finish, save the file as `PinpointEventStreamPermissionsPolicy.json`\.

1. Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html) command to attach the permissions policy to the role:

   ```
   aws iam put-role-policy --role-name PinpointEventStreamRole --policy-name PinpointEventStreamPermissionsPolicy --policy-document file://PinpointEventStreamPermissionsPolicy.json
   ```
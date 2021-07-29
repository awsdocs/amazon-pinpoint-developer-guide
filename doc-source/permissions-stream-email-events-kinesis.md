# IAM role for streaming email events to Kinesis Data Firehose<a name="permissions-stream-email-events-kinesis"></a>

In the Amazon Pinpoint Email API, you can create *configuration sets* that specify how to handle certain email events\. For example, you can create a configuration set that sends delivery notifications to a specific *event destination*, such as an Amazon SNS topic or a Kinesis Data Firehose delivery stream\. When you send email through the Amazon Pinpoint Email API using that configuration set, Amazon Pinpoint sends information about email\-related events to the event destination that you specified in the configuration set\.

The Amazon Pinpoint Email API can deliver information about the following types of email events to the event destinations that you specify:
+ **Sends** – The call to Amazon Pinpoint was successful, and Amazon Pinpoint attempted to deliver the email\. 
+ **Deliveries** – Amazon Pinpoint successfully delivered the email to the recipient's mail server\.
+ **Rejections** – Amazon Pinpoint accepted the email, determined that it contained malware, and rejected it\. Amazon Pinpoint didn't attempt to deliver the email to the recipient's mail server\.
+ **Rendering Failures** – The email wasn't sent because of a template rendering issue\. This event type only occurs when you send an email that includes substitution tags\. This event type can occur when substitution values are missing\. It can also occur when there's a mismatch between the substitution tags that you used in the email and the substitution data that you provided\.
**Note**  
If you use substitution tags in the emails that you send by using the Amazon Pinpoint Email API, you should always create a configuration set that records Rendering Failure events\.
+ **Bounces** – The recipient's mail server permanently rejected the email\.
+ **Complaints** – The email was successfully delivered to the recipient, but the recipient used the "Report Spam" \(or equivalent\) feature of their email client to report the message\. 
+ **Opens** – The recipient received the message and opened it in their email client\. 
+ **Clicks** – The recipient clicked one or more links that were contained in the email\.
**Note**  
Every time a recipient opens or clicks an email, Amazon Pinpoint generates unique open or click events, respectively\. In other words, if a specific recipient opens a message five times, Amazon Pinpoint reports five separate Open events\.

If you want to send data about these events to a Kinesis Data Firehose stream, you have to create an IAM role that has the appropriate permissions\. The role must use the following policies:
+ A trust policy that allows Amazon Pinpoint to assume the role\.
+ A permissions policy that allows the Amazon Pinpoint Email API to send email delivery and response records to your stream\.

After you create the role, you can configure Amazon Pinpoint to automatically send events to your stream\. For more information, see [Streaming Amazon Pinpoint events to Kinesis](event-streams.md)\.

## Trust policy<a name="permissions-stream-email-events-kinesis-trustpolicy"></a>

To allow the Amazon Pinpoint Email API to assume the IAM role and perform the actions allowed by the permissions policy, attach the following trust policy to the role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ses.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "accountId"
        }
      }
    }
  ]
}
```

In the example above, replace *accountId* with the ID of your AWS account\.

## Permissions policy<a name="permissions-stream-email-events-kinesis-permissionspolicies"></a>

To allow the Amazon Pinpoint Email API to send email event data to a Kinesis Data Firehose delivery stream, attach the following permissions policy to a role\.

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
        	"arn:aws:firehose:region:accountId:deliverystream/deliveryStreamName"
    	]
    }
}
```

In the example above, replace *region* with the name of the AWS Region that you created the delivery stream in\. Replace *accountId* with the ID of your AWS account\. Finally, replace *deliveryStreamName* with the name of the delivery stream\.

## Creating the IAM role \(AWS CLI\)<a name="permissions-stream-email-events-kinesis-create"></a>

Complete the following steps to create the IAM role by using the AWS Command Line Interface \(AWS CLI\)\. For information about installing and configuring the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**To create the IAM role by using the AWS CLI**

1. Create a JSON file that contains the trust policy for your role, and then save the file locally\. You can copy the [trust policy](#permissions-stream-email-events-kinesis-trustpolicy) that's provided earlier in this topic\.

1. Use the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the role and attach the trust policy:

   ```
   aws iam create-role --role-name PinpointEventStreamRole \ 
   --assume-role-policy-document file://PinpointEventStreamTrustPolicy.json
   ```

   In the preceding example, replace *PinpointEventStreamTrustPolicy\.json* with the full path to the file that contains the trust policy\.

   After you run this command, the AWS CLI returns the following output:

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
                           "Service": "ses.amazonaws.com"
                       }
                   }
               ]
           }, 
           "RoleId": "AKIAIOSFODNN7EXAMPLE", 
           "CreateDate": "2019-04-10T14:20:42.314Z", 
           "RoleName": "PinpointEventStreamRole", 
           "Path": "/", 
           "Arn": "arn:aws:iam::111122223333:role/PinpointEventStreamRole"
       }
   }
   ```

1. Create a JSON file that contains the permissions policy for your role, and then save the file locally\. You can copy the [permissions policy](#permissions-stream-email-events-kinesis-permissionspolicies) that's provided earlier in this topic\.

1. Use the [put\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html) command to attach the permissions policy to the role:

   ```
   aws iam put-role-policy \
   --role-name PinpointEventStreamRole \
   --policy-name PinpointEventStreamPermissionsPolicy 
   --policy-document file://PinpointEventStreamPermissionsPolicy.json
   ```

   In the preceding example, replace *PinpointEventStreamPermissionsPolicy\.json* with the full path to the file that contains the permissions policy\.
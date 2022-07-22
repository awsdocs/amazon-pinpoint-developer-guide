# Creating and updating Kinesis Data Firehose event destinations<a name="sms-voice-v2-event-destinations-akf"></a>

Amazon Kinesis Data Firehose is a fully managed service for delivering real\-time streaming data to multiple types of destinations\. Kinesis Data Firehose is part of the Kinesis streaming data platform\. To learn more about Kinesis Data Firehose, see the [Amazon Kinesis Data Firehose Developer Guide](https://docs.aws.amazon.com/firehose/latest/dev/)\.

The examples in this section assume that you've already installed and configured the AWS Command Line Interface\. For more information about setting up the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**Topics**
+ [Creating Kinesis Data Firehose event destinations](#sms-voice-v2-event-destinations-akf-creating)
+ [Updating Kinesis Data Firehose event destinations](#sms-voice-v2-event-destinations-akf-updating)

## Creating Kinesis Data Firehose event destinations<a name="sms-voice-v2-event-destinations-akf-creating"></a>

Before you can create a Kinesis Data Firehose event destination, you must first create a Kinesis Data Firehose stream\. For more inforation about creating log groups, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

You also have to create an IAM role that allows the SMS and Voice API to send data to the stream\. The following section contains information about the requirements for this role\.

### IAM policy for Kinesis Data Firehose<a name="sms-voice-v2-event-destinations-akf-creating-role"></a>

Use the following example to create a policy for sending events to a Kinesis Data Firehose stream\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "firehose:PutRecord",
            "Resource": "arn:aws:firehose:us-east-1:111122223333:deliverystream/*"
        }
    ]
}
```

For more information about IAM policies, see [Policies and permissions in IAM](IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

After you create the policy, create a new IAM role, and then attach the policy to it\. When you create the role, also add the following trust policy to it:

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Principal": {
            "Service": "sms-voice.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
    }
}
```

For more information about creating IAM roles, see [Creating IAM roles](IAM/latest/UserGuide/id_roles_create.html) in the *IAM User Guide*\.

### Creating the event destination<a name="sms-voice-v2-event-destinations-akf-creating-cli"></a>

After you create the IAM role and the Kinesis Data Firehose delivery stream, you can create the event destination\.

You can use the [CreateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreateEventDestination.html) API to create an event destination\.

**To create a Kinesis Data Firehose event destination using the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event-types eventTypes \
  > --kinesis-firehose-destination IamRoleArn=arn:aws:iam::111122223333:role/AKFSMSRole,DeliveryStreamArn=arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType eventTypes `
  >> -KinesisFirehoseDestination_IamRoleArn_IamRoleArn arn:aws:iam::111122223333:role/AKFSMSRole `
  >> -KinesisFirehoseDestination_DeliveryStreamArn arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-event-destination ^
      --event-destination-name eventDestinationName ^
      --configuration-set-name configurationSet ^
      --matching-event-types eventTypes ^
      --kinesis-firehose-destination IamRoleArn=arn:aws:iam::111122223333:role/AKFSMSRole,DeliveryStreamArn=arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a name that describes the event destination\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\.
  + Replace *eventTypes* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `IamRoleArn` with the Amazon Resource Name \(ARN\) of an IAM role that has the policies described in [IAM policy for CloudWatch Logs](sms-voice-v2-event-destinations-cwl.md#sms-voice-v2-event-destinations-cwl-creating-role)\.
  + Replace the value of `DeliveryStreamArn` with the ARN of the Kinesis Data Firehose stream that you want to send events to\. 

## Updating Kinesis Data Firehose event destinations<a name="sms-voice-v2-event-destinations-akf-updating"></a>

You can use the [UpdateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_UpdateEventDestination.html) API to update an event destination\.

The procedure for updating a Kinesis Data Firehose event destination is similar to the process for creating an event destination\.

**To update a Kinesis Data Firehose event destination using the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event-types eventTypes \
  > --kinesis-firehose-destination IamRoleArn=arn:aws:iam::111122223333:role/AKFSMSRole,DeliveryStreamArn=arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Update-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType eventTypes `
  >> -KinesisFirehoseDestination_IamRoleArn_IamRoleArn 111122223333:role/AKFSMSRole `
  >> -KinesisFirehoseDestination_DeliveryStreamArn arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-event-destination ^
   --event-destination-name eventDestinationName ^
   --configuration-set-name configurationSet ^
   --matching-event-types eventTypes ^
   --kinesis-firehose-destination IamRoleArn=111122223333:role/AKFSMSRole,DeliveryStreamArn=arn:aws:firehose:us-east-1:111122223333:deliverystream/MyDeliveryStream
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a name of the event destination that you want to modify\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\. You can associate the event destination with a different configuration set\.
  + Replace *eventTypes* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `IamRoleArn` with the Amazon Resource Name \(ARN\) of an IAM role that has the policies described in [IAM policy for CloudWatch Logs](sms-voice-v2-event-destinations-cwl.md#sms-voice-v2-event-destinations-cwl-creating-role)\.
  + Replace the value of `DeliveryStreamArn` with the ARN of the Kinesis Data Firehose stream that you want to send events to\. 
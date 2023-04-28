# Creating and updating Amazon SNS event destinations<a name="sms-voice-v2-event-destinations-sns"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a web service that enables applications, end\-users, and devices to instantly send and receive notifications\. To learn more about Amazon SNS, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\.

The examples in this section assume that you've already installed and configured the AWS Command Line Interface\. For more information about setting up the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**Topics**
+ [Creating Amazon SNS event destinations](#sms-voice-v2-event-destinations-sns-creating)

## Creating Amazon SNS event destinations<a name="sms-voice-v2-event-destinations-sns-creating"></a>

Before you can create an Amazon SNS event destination, you must first create an Amazon SNS topic\. For more inforation about creating Amazon SNS topics, see [Creating a topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) in the *Amazon Simple Notification Service Developer Guide*\.

You also have to create an IAM role that allows the SMS and Voice API to send data to the stream\. The following section contains information about the requirements for this role\.

### IAM policy for Amazon SNS<a name="sms-voice-v2-event-destinations-sns-creating-role"></a>

Use the following example to create a policy for sending events to an Amazon SNS topic\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "SNS:Publish",
            "Resource": "arn:aws:sns:us-east-1:111122223333:MyTopic"
        }
    ]
}
```

For more information about IAM policies, see [Policies and permissions in IAM](/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

### Creating the event destination<a name="sms-voice-v2-event-destinations-sns-creating-cli"></a>

You can use the [CreateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreateEventDestination.html) API to create an event destination\.

**To create an Amazon SNS event destination in the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event-types eventTypes \
  > --sns-destination TopicArn=arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType eventTypes `
  >> -SnsDestination_TopicArn arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-event-destination ^
   --event-destination-name eventDestinationName ^
   --configuration-set-name configurationSet ^
   --matching-event types eventTypes ^
   --sns-destination TopicArn=arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a descriptive name for the event destination\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\.
  + Replace *eventTypes* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `TopicArn` with the Amazon Resource Name \(ARN\) of the Amazon SNS topic that you want to send events to\.

### Updating Amazon SNS event destinations<a name="sms-voice-v2-event-destinations-sns-updating"></a>

You can use the [UpdateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_UpdateEventDestination.html) API to update an event destination\.

The procedure for updating an Amazon SNS event destination is similar to the process for creating an event destination\.

**To update an Amazon SNS event destination in the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 update-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event types eventTypes \
  > --sns-destination TopicArn=arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Update-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType eventTypes `
  >> -SnsDestination_TopicArn arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 update-event-destination ^
      --event-destination-name eventDestinationName ^
      --configuration-set-name configurationSet ^
      --matching-event types eventTypes ^
      --sns-destination TopicArn=arn:aws:sns:us-east-1:111122223333:snsTopic
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a name of the event destination that you want to modify\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\. You can associate the event destination with a different configuration set\.
  + Replace *eventTypes* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `TopicArn` with the Amazon Resource Name \(ARN\) of the Amazon SNS topic that you want to send events to\.

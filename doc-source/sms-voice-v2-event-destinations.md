# Managing event destinations<a name="sms-voice-v2-event-destinations"></a>

In the Amazon Pinpoint SMS and Voice API, version 2, an *event destination* is a location \(such as a CloudWatch Logs Group, a Kinesis Data Firehose stream, or an Amazon SNS topic\) that SMS and voice events are sent to\. To use event destinations, you first create the destination, and then associate it with a [configuration set](sms-voice-v2-configuration-sets.md)\. You can associate up to five event destinations with a single configuration set\. When you send a message, your call to the API includes a reference to the configuration set\. 

This section contains information about using the AWS CLI to manage event destinations in the Amazon Pinpoint SMS and Voice API, version 2\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Event types](sms-voice-v2-event-destinations-types.md)
+ [Creating event destinations](#sms-voice-v2-event-destinations-creating)
+ [Updating event destinations](#sms-voice-v2-event-destinations-updating)
+ [Deleting event destinations](#sms-voice-v2-event-destinations-deleting)
+ [Creating and updating CloudWatch Logs event destinations](sms-voice-v2-event-destinations-cwl.md)
+ [Creating and updating Kinesis Data Firehose event destinations](sms-voice-v2-event-destinations-akf.md)
+ [Creating and updating Amazon SNS event destinations](sms-voice-v2-event-destinations-sns.md)

## Creating event destinations<a name="sms-voice-v2-event-destinations-creating"></a>

The procedures for creating event destinations differ depending on the type of event destination you want to create\.
+ For information about creating event destinations that send events to a CloudWatch Logs group, see [Creating CloudWatch Logs event destinations](sms-voice-v2-event-destinations-cwl.md#sms-voice-v2-event-destinations-cwl-creating)\.
+ For information about creating event destinations that send events to a Kinesis Data Firehose stream, see [Creating Kinesis Data Firehose event destinations](sms-voice-v2-event-destinations-akf.md#sms-voice-v2-event-destinations-akf-creating)\.
+ For information about creating event destinations that send events to a Amazon SNS topic, see [Creating Amazon SNS event destinations](sms-voice-v2-event-destinations-sns.md#sms-voice-v2-event-destinations-sns-creating)\.

## Updating event destinations<a name="sms-voice-v2-event-destinations-updating"></a>

The procedures for updating event destinations also differ depending on the type of event destination you're updating\.
+ For information about updating event destinations that send events to a CloudWatch Logs group, see [Updating CloudWatch Logs event destinations](sms-voice-v2-event-destinations-cwl.md#sms-voice-v2-event-destinations-cwl-updating)\.
+ For information about updating event destinations that send events to a Kinesis Data Firehose stream, see [Updating Kinesis Data Firehose event destinations](sms-voice-v2-event-destinations-akf.md#sms-voice-v2-event-destinations-akf-updating)\.
+ For information about updating event destinations that send events to a Amazon SNS topic, see [Updating Amazon SNS event destinations](sms-voice-v2-event-destinations-sns.md#sms-voice-v2-event-destinations-sns-updating)\.

## Deleting event destinations<a name="sms-voice-v2-event-destinations-deleting"></a>

You can use the [DeleteEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DeleteEventDestination.html) API to delete an event destination\.

The process for deleting an event destination is the same regardless of the type of event destination that you want to delete\.

**To delete a configuration set**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 delete-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSetName
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSetName
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 delete-event-destination ^
   --event-destination-name eventDestinationName ^
   --configuration-set-name configurationSetName
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with the name or Amazon Resource Name \(ARN\) of the event destination that you want to delete\.
  + Replace *configurationSetName* with the name or ARN of the configuration set that the event destination is associated with\.
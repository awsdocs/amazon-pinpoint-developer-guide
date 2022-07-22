# Creating and updating CloudWatch Logs event destinations<a name="sms-voice-v2-event-destinations-cwl"></a>

Amazon CloudWatch Logs is an AWS service that you can use to monitor, store, and access log files\. When you create a CloudWatch Logs event destination, Amazon Pinpoint sends the types of events you specified in the event destination to a CloudWatch Logs group\. To learn more about CloudWatch Logs, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.

The examples in this section assume that you've already installed and configured the AWS Command Line Interface\. For more information about setting up the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

**Topics**
+ [Creating CloudWatch Logs event destinations](#sms-voice-v2-event-destinations-cwl-creating)
+ [Updating CloudWatch Logs event destinations](#sms-voice-v2-event-destinations-cwl-updating)

## Creating CloudWatch Logs event destinations<a name="sms-voice-v2-event-destinations-cwl-creating"></a>

Before you can create a CloudWatch Logs event destination, you must first create a CloudWatch Logs group\. For more inforation about creating log groups, see [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch Logs User Guide*\.

You also have to create an IAM role that allows the SMS and Voice API to write to the log group\. The following section contains information about the requirements for this role\.

### IAM policy for CloudWatch Logs<a name="sms-voice-v2-event-destinations-cwl-creating-role"></a>

Use the following example to create a policy for sending events to a CloudWatch Logs group\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-east-1:111122223333:log-group:*"
            ]
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

### Creating the event destination<a name="sms-voice-v2-event-destinations-cwl-creating-cli"></a>

After you create the IAM role and the CloudWatch Logs group, you can create the event destination\.

You can use the [CreateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreateEventDestination.html) API to create an event destination\.

**To create a CloudWatch Logs event destination using the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event-types ALL \
  > --cloud-watch-logs-destination IamRoleArn=arn:aws:iam::111122223333:role/CWLSMSRole,LogGroupArn=arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVEventDestination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType ALL `
  >> -CloudWatchLogsDestination_IamRoleArn arn:aws:iam::111122223333:role/CWLSMSRole `
  >> -CloudWatchLogsDestination_LogGroupArn arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-event-destination ^
   --event-destination-name eventDestinationName ^
   --configuration-set-name configurationSet ^
   --matching-event-types ALL ^
   --cloud-watch-logs-destination IamRoleArn=arn:aws:iam::111122223333:role/CWLSMSRole,LogGroupArn=arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a name that describes the event destination\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\.
  + Replace *ALL* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `IamRoleArn` with the Amazon Resource Name \(ARN\) of an IAM role that has the policies described in [IAM policy for CloudWatch Logs](#sms-voice-v2-event-destinations-cwl-creating-role)\.
  + Replace the value of `LogGroupArn` with the ARN of the CloudWatch Logs group that you want to send events to\. 

## Updating CloudWatch Logs event destinations<a name="sms-voice-v2-event-destinations-cwl-updating"></a>

You can use the [UpdateEventDestination](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_UpdateEventDestination.html) API to update an event destination\.

The procedure for updating a CloudWatch Logs event destination is similar to the process for creating an event destination\.

**To update an event destination in the AWS CLI**
+ At the command line, run the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 update-event-destination \
  > --event-destination-name eventDestinationName \
  > --configuration-set-name configurationSet \
  > --matching-event types eventTypes \
  > --cloud-watch-logs-destination IamRoleArn=arn:aws:iam::111122223333:role/CWLSMSRole,LogGroupArn=arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> aws pinpoint-sms-voice-v2 update-event-destination `
  >> -EventDestinationName eventDestinationName `
  >> -ConfigurationSetName configurationSet `
  >> -MatchingEventType eventTypes `
  >> -CloudWatchLogsDestination_IamRoleArn arn:aws:iam::111122223333:role/CWLSMSRole `
  >> -CloudWatchLogsDestination_LogGroupArn arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 update-event-destination ^
   --event-destination-name eventDestinationName ^
   --configuration-set-name configurationSet ^
   --matching-event types eventTypes ^
   --cloud-watch-logs-destination IamRoleArn=arn:aws:iam::111122223333:role/CWLSMSRole,LogGroupArn=arn:aws:logs:us-east-1:111122223333:log-group:MyCWLLogGroup
  ```

------

  In the preceding command, make the following changes:
  + Replace *eventDestinationName* with a name of the event destination that you want to modify\.
  + Replace *configurationSet* with the name of the configuration set that you want to associate the event destination with\. You can associate the event destination with a different configuration set\.
  + Replace *eventTypes* with one of the event types listed in [Event types](sms-voice-v2-event-destinations-types.md)\.
  + Replace the value of `IamRoleArn` with the Amazon Resource Name \(ARN\) of an IAM role that has the policies described in [IAM policy for CloudWatch Logs](#sms-voice-v2-event-destinations-cwl-creating-role)\.
  + Replace the value of `LogGroupArn` with the ARN of the CloudWatch Logs group that you want to send events to\. 
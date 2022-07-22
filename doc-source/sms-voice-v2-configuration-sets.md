# Managing configuration sets<a name="sms-voice-v2-configuration-sets"></a>

A *configuration set* is a set of rules that are applied when you send a message\. For example, a configuration set can specify a destination for events related to a message\. When SMS events occur \(such as delivery or failure events\), they are routed to the destination associated with the configuration set that you specified when you sent the message\. You're not required to use configuration sets when you send messages, but we recommend that you do\. If you don't specify a configuration set with an event destination, the API doesn't emit event records\. These event records are a useful way to determine how many messages you sent, how much you paid for each one, and whether or not the message was received by the recipient\.

This section contains information about using the AWS CLI to manage configuration sets in the Amazon Pinpoint SMS and Voice API, version 2\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Creating a configuration set](#sms-voice-v2-configuration-sets-creating)
+ [Listing configuration sets](#sms-voice-v2-configuration-sets-describe)
+ [Deleting configuration sets](#sms-voice-v2-configuration-sets-deleting)

## Creating a configuration set<a name="sms-voice-v2-configuration-sets-creating"></a>

You can use the [CreateConfigurationSet](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreateConfigurationSet.html) API to create a new configuration set\. After you create a configuration set, you can associate event destination with it\.

To create a configuration set, run the following command in the AWS CLI:

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint-sms-voice-v2 create-configuration-set \
> --configuration-set-name configurationSet
```

------
#### [ PowerShell ]

```
PS C:\> New-SMSVConfigurationSet `
>> -ConfigurationSetName configurationSet
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint-sms-voice-v2 create-configuration-set ^
 --configuration-set-name configurationSet
```

------

In the preceding command, replace *configurationSet* with the name of the configuration set that you want to create\.

You can add tags to the configuration set by specifying the optional `tags` parameter, as shown in the following example:

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint-sms-voice-v2 create-configuration-set \
> --configuration-set-name ConfigurationSet \
> --tags Key=key1,Value=value1 Key=key2,Value=value2
```

------
#### [ PowerShell ]

```
PS C:\> $tag = New-Object Amazon.PinpointSMSVoiceV2.Model.Tag
PS C:\> $tag.Key = "key1"
PS C:\> $tag.Value = "value1"
                        
PS C:\> New-SMSVConfigurationSet `
>> -ConfigurationSetName configurationSet `
>> -Tag $tag
```

------
#### [ Windows Command Prompt ]

```
C:\>aws pinpoint-sms-voice-v2 create-configuration-set ^
 --configuration-set-name ConfigurationSet ^
 --tags Key=key1,Value=value1 Key=key2,Value=value2
```

------

## Listing configuration sets<a name="sms-voice-v2-configuration-sets-describe"></a>

You can use the [DescribeConfigurationSets](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribeConfigurationSets.html) API to view information about the configuration sets in your Amazon Pinpoint account\.

**To view a list of the configuration sets in your account using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-configuration-sets
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVConfigurationSet
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-configuration-sets
  ```

------

If you want to view the details for a specific configuration set, or a group of configuration sets, use the `ConfigurationSetNames` parameter\.

**To view information about specific configuration sets using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-configuration-sets \ 
  > --configuration-set-names configurationSet
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVConfigurationSet `
  >> -ConfigurationSetName configurationSet
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-configuration-sets ^ 
   --configuration-set-names configurationSet
  ```

------

In the preceding command, replace *configurationSet* with the name of the configuration set that you want to find the details of\. You can also specify multiple configuration sets by separating the name of each configuration set with a space\.

## Deleting configuration sets<a name="sms-voice-v2-configuration-sets-deleting"></a>

You can use the [DeleteConfigurationSet](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DeleteConfigurationSet.html) API to delete a configuration set\.

**To delete a configuration set using the AWS CLI:**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 delete-configuration-set \
  > --configuration-set-name configurationSet
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVConfigurationSet `
  >> -ConfigurationSetName configurationSet
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 delete-configuration-set ^
   --configuration-set-name configurationSet
  ```

------

  In the preceding command, replace *configurationSet* with the name of the configuration set that you want to delete\.
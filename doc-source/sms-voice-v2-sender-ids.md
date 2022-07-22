# Managing sender IDs<a name="sms-voice-v2-sender-ids"></a>

You can use the Amazon Pinpoint SMS and Voice API version 2 to specify default *sender IDs* for a configuration set, and to add sender IDs to a pool\. A sender ID is an alphabetic string that appears on the recipient's device when they receive a message from you\. Sender IDs are not supported in all countries\. For a list of countries where Amazon Pinpoint supports sender IDs, see [Supported countries and regions](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-countries.html) in the *Amazon Pinpoint User Guide*\.

Sender IDs only support one\-way messaging—that is, your recipients can't reply to messages that you send using sender IDs\. In some countries, such as [India](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-senderid-india.html), you must register your use case and message templates in order to use sender IDs\.

This section contains information about using the AWS CLI to manage sender IDs\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Associating a sender ID with a pool](#sms-voice-v2-sender-ids-associate)
+ [Setting a default sender ID for a configuration set](#sms-voice-v2-sender-ids-setting)
+ [Disassociating a sender ID from a configuration set](#sms-voice-v2-sender-ids-unsetting)

## Associating a sender ID with a pool<a name="sms-voice-v2-sender-ids-associate"></a>

You can use the [AssociateOriginationIdentity](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_AssociateOriginationIdentity.html) API to add origination identities \(that is, phone numbers or sender IDs\) to an existing pool\.

Before you complete this step, you must first create a pool\. For more information, see [Creating a pool](sms-voice-v2-pools.md#sms-voice-v2-pools-creating)\.

**To add a sender ID to a pool using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 associate-origination-identity \
  > --pool-id poolId \
  > --origination-identity SENDER \
  > --iso-country-code IN
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Register-SMSVOriginationIdentity `
  >> -PoolId poolId `
  >> -OriginationIdentity SENDER `
  >> -IsoCountryCode IN
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 associate-origination-identity ^
   --pool-id poolId ^
   --origination-identity SENDER ^
   --iso-country-code IN
  ```

------

  In the preceding command, make the following changes:
  + Replace *poolId* with the ID or Amazon Resource Name \(ARN\) of the pool that you want to add the sender ID to\.
  + Replace *SENDER* with the sender ID that you want to add to the pool\.
  + Replace *IN* with the two\-letter ISO\-3166 alpha\-2 code for the country of the sender ID\.

## Setting a default sender ID for a configuration set<a name="sms-voice-v2-sender-ids-setting"></a>

You can use the [SetDefaultSenderId](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_SetDefaultSenderId.html) API to set the default sender ID for a configuration set\.

Before you can set a default sender ID, you must first create a configuration set\. For more information, see [Creating a configuration set](sms-voice-v2-configuration-sets.md#sms-voice-v2-configuration-sets-creating)\.

**To set a default sender ID using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 set-default-sender-id \ 
  > --configuration-set-name configurationSetName \
  > --sender-id senderId
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Set-SMSVDefaultSenderId `
  >> -ConfigurationSetName configurationSetName `
  >> -SenderId senderId
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 set-default-sender-id ^ 
   --configuration-set-name configurationSetName ^
   --sender-id senderId
  ```

------

  In the preceding example, make the following changes:
  + Replace *configurationSetName* with the name of the configuration set that you want to define a default sender ID for\.
  + Replace *senderId* with the value of the sender ID that you want to use\.

## Disassociating a sender ID from a configuration set<a name="sms-voice-v2-sender-ids-unsetting"></a>

You can use the [DeleteDefaultSenderId](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DeleteDefaultSenderId.html) API to disassociate a default sender ID from a configuration set\.

**To disassociate a default sender ID using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 delete-default-sender-id \ 
  > --configuration-set-name configurationSetName
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVDefaultSenderId ` 
  >> -ConfigurationSetName configurationSetName
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 delete-default-sender-id ^ 
   --configuration-set-name configurationSetName
  ```

------

  In the preceding example, replace *configurationSetName* with the name of a configuration set that contains a default sender ID\.
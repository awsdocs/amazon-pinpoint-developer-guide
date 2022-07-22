# Managing phone numbers<a name="sms-voice-v2-phone-numbers"></a>

You can use the Amazon Pinpoint SMS and Voice API version 2 to request and relinquish phone numbers from your Amazon Pinpoint account\. You can also use the API to view a list of all of the phone numbers in your account\.

This section contains information about using the AWS CLI to manage your phone numbers\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Requesting phone numbers](#sms-voice-v2-phone-numbers-requesting)
+ [Modifying phone number capabilities](#sms-voice-v2-phone-numbers-update)
+ [Listing phone numbers](#sms-voice-v2-phone-numbers-listing)
+ [Releasing phone numbers](#sms-voice-v2-phone-numbers-releasing)

## Requesting phone numbers<a name="sms-voice-v2-phone-numbers-requesting"></a>

You can use the [RequestPhoneNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_RequestPhoneNumber.html) API to add new phone numbers to your account\. Phone number availability and supported features vary by country\.

**To request a phone number**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws request-phone-number \
  > --iso-country-code XX \
  > --message-type TRANSACTIONAL \
  > --number-capabilities VOICE \
  > --number-type LONG_CODE \
  > --pool-id poolId \
  > --deletion-protection-enabled true \
  > --opt-out-list-name optOutListName \
  > --registration-id CO123EX
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVPhoneNumber `
  >> -IsoCountryCode XX `
  >> -MessageType TRANSACTIONAL `
  >> -NumberCapabilities VOICE `
  >> -NumberType LONG_CODE `
  >> -PoolId poolId `
  >> -DeletionProtectionEnabled $true `
  >> -OptOutListName optOutListName `
  >> -RegistrationId CO123EX
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws request-phone-number ^
   --iso-country-code XX ^
   --message-type TRANSACTIONAL ^
   --number-capabilities VOICE ^
   --number-type LONG_CODE ^
   --pool-id poolId ^
   --deletion-protection-enabled true ^
   --opt-out-list-name optOutListName ^
   --registration-id CO123EX
  ```

------

  In the preceding command, make the following changes:
  + Replace *XX* with the two\-letter ISO\-3166 alpha\-2 code for the country of the phone number \(such as `CA` for Canada\)\.
  + If you want to use the phone number to send promotional or marketing\-related content, replace *TRANSACTIONAL* with `PROMOTIONAL`\. Otherwise, use `TRANSACTIONAL`\.
  + If you want to request a phone number for sending SMS messages, replace *VOICE* with `SMS`\. You can also request a phone number that can be used to send both SMS and voice messages by specifying `SMS VOICE`\.
  + Replace *LONG\_CODE* with the type of phone number you want to request\. Acceptable values are `LONG_CODE`, `TOLL_FREE`, and `TEN_DLC`\.
  + Replace *poolId* with the ID or Amazon Resource Name \(ARN\) of the pool that you want to add the phone number to\. This parameter is optional\. If you don't want to add the phone number to a pool, omit this parameter\.
  + If you want to disable deletion protection for this phone number, omit the `DeletionProtectionEnabled` parameter, or set its value to `false`\. Deletion protection is disabled by default\. If deletion protection is enabled, you can't delete the phone number using the [ReleasePhoneNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_ReleasePhoneNumber.html) API, unless you [update](#sms-voice-v2-phone-numbers-update) the configuration of the phone number to disable this feature\.
  + Replace *optOutListName* with the name or ARN of the opt\-out list that you want to associate with the phone number\. This parameter is optional\. If you don't want to associate the phone number with an opt\-out list, omit this parameter\.
  + If you're requesting a phone number to use with a 10DLC campaign, replace *CO123EX* with the ID of the 10DLC campaign that you want to use\.
**Note**  
If you plan to use a 10DLC phone number, you must first register your company and campaign\. Currently, the only way to complete these registration processes is to use the Amazon Pinpoint console\. For more information about 10DLC registration, see [10DLC](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-10dlc.html) in the *Amazon Pinpoint User Guide*\.

If the number is successfully added to your account, you see output similar to the following:

```
{
    "PhoneNumberArn": "arn:aws:sms-voice:us-east-1:111122223333:phone-number/phone-615790209ea34aea8da9b729fexample",
    "PhoneNumberId": "phone-615790209ea34aea8da9b729fexample",
    "PhoneNumber": "+12045550123",
    "Status": "PENDING",
    "IsoCountryCode": "CA",
    "MessageType": "TRANSACTIONAL",
    "NumberCapabilities": [
        "SMS"
    ],
    "NumberType": "LONG_CODE",
    "MonthlyLeasingPrice": "1.00",
    "TwoWayEnabled": false,
    "SelfManagedOptOutsEnabled": false,
    "OptOutListName": "Default",
    "DeletionProtectionEnabled": false,
    "CreatedTimestamp": 1645568542.0
}
```

**Note**  
When you first purchase a phone number, the value of the `Status` attribute is `PENDING`\. When the phone number is ready to use, the value of `Status` changes to `ACTIVE`\.

If a phone number that meets the parameters you specified isn't available, the request fails with an error\.

## Modifying phone number capabilities<a name="sms-voice-v2-phone-numbers-update"></a>

After you request a phone number, you can use the [UpdatePhoneNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_UpdatePhoneNumber.html) API to change the settings of that phone number, or to enable additional features\. You can change several phone number settings, including the pool and opt\-out list that are associated with the phone number, as well as the deletion protection setting\.

An example of an additional feature that you can enable by updating a phone number is two\-way messaging\. Support for two\-way messaging varies depending on which country you plan to send messages to\. For a list of supported countries, see [Supported countries and regions](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-countries.html)\.

------
#### [ Linux, macOS, or Unix ]

```
$ aws update-phone-number \
> --phone-number-id phone-d2b0f5dd4fd14ebdb2a3b9128example \
> --deletion-protection-enabled true \
> --opt-out-list-name optOutListName \ 
> --self-managed-opt-outs-enabled true \
> --two-way-enabled true \
> --two-way-channel-arn arn:aws:sns:us-east-1:111122223333:MyTopic
```

------
#### [ PowerShell ]

```
PS C:\> Update-SMSVPhoneNumber `
>> -PhoneNumberId phone-d2b0f5dd4fd14ebdb2a3b9128example \
>> -DeletionProtectionEnabled $true \
>> -OptOutListName optOutListName \ 
>> -SelfManagedOptOutsEnabled $true \
>> -TwoWayEnabled $true \
>> -TwoWayChannelArn arn:aws:sns:us-east-1:111122223333:MyTopic
```

------
#### [ Windows Command Prompt ]

```
C:\> aws update-phone-number ^
 --phone-number-id phone-d2b0f5dd4fd14ebdb2a3b9128example ^
 --deletion-protection-enabled true ^
 --opt-out-list-name optOutListName ^
 --self-managed-opt-outs-enabled true ^
 --two-way-enabled true ^
 --two-way-channel-arn arn:aws:sns:us-east-1:111122223333:MyTopic
```

------

In the preceding command, do the following:
+ Replace *phone\-d2b0f5dd4fd14ebdb2a3b9128example* with the PhoneNumberID or the Amazon Resource Name \(ARN\) of the phone number that you want to update\. You can find both of these values by using the [DescribePhoneNumbers](#sms-voice-v2-phone-numbers-listing) operation\.
+ Replace *optOutListName* with the name of the [opt\-out list](sms-voice-v2-opt-out-lists.md) that you want to associate with this phone number\.
+ If you want to disable the deletion protection feature, change the value of the `DeletionProtectionEnabled` parameter to `false`\.
+ If you want to [self\-managed SMS opt\-outs](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-managing.html#settings-account-sms-self-managed-opt-out) feature, change the value of the `SelfManagedOptOutsEnabled` parameter to `false`\.
+ If you want to disable [two\-way SMS messaging](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-two-way.html) for this phone number, change the value of the `TwoWayEnabled` parameter to `false`\.
+ If you enable the two\-way messaging feature for the phone number, you must specify the ARN of an Amazon SNS topic\. Replace *arn:aws:sns:us\-east\-1:111122223333:MyTopic* with the ARN of the Amazon SNS topic that you want to use\. When you receive incoming messages, they are sent to the topic that you specify\.

The `PhoneNumberId` parameter is the only required parameter for this command\. You can omit any of the other parameters if you don't want to change the corresponding settings\.

## Listing phone numbers<a name="sms-voice-v2-phone-numbers-listing"></a>

You can use the [DescribePhoneNumbers](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribePhoneNumbers.html) to get more information about the origination phone numbers in your Amazon Pinpoint account\.

**To list all of the phone numbers in your account using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-phone-numbers
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVPhoneNumber
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-phone-numbers
  ```

------

The output of this command includes details about all of the phone numbers in your account\. You can also view information about specific phone numbers by including the `PhoneNumberId` parameter\.

**To view information about a specific phone number using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-phone-numbers \
  >  --phone-number-id phone-d2b0f5dd4fd14ebdb2a3b9128example
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVPhoneNumber `
  >>  -PhoneNumberId phone-d2b0f5dd4fd14ebdb2a3b9128example
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-phone-numbers ^
    --phone-number-id phone-d2b0f5dd4fd14ebdb2a3b9128example
  ```

------

In the preceding example, replace *phone\-d2b0f5dd4fd14ebdb2a3b9128example* with the PhoneNumberID or the Amazon Resource Name \(ARN\) of the phone number that you want to view more information about\.

You can also use the `filter` parameter to filter the list of phone numbers based on criteria that you define\. For example, you can filter by the country of the phone number, or by its capabilities \(that is, whether it supports SMS or voice messages, or both\)\.

**To view a filtered list of phone numbers using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-phone-numbers \
  > --filters Name=number-capability,Values=SMS \
  > --filters Name=iso-country-code,Values=CA
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> $filter1 = New-Object Amazon.PinpointSMSVoiceV2.Model.PhoneNumberFilter
  PS C:\> $filter2 = New-Object Amazon.PinpointSMSVoiceV2.Model.PhoneNumberFilter
  PS C:\> $filter1.Name = "number-capability"; $filter.Values = "SMS"
  PS C:\> $filter2.Name = "iso-country-code"; $filter.Values = "CA"
                          
  PS C:\> Get-SMSVPhoneNumber -Filter $filter1,$filter2
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-phone-numbers ^
   --filters Name=number-capability,Values=SMS ^
   --filters Name=iso-country-code,Values=CA
  ```

------

The filter `Name` can be any of the following values:
+ `status` – The current status of the phone number, such as `ACTIVE`\.
+ `iso-country-code` – The two\-character ISO\-3166 alpha\-2 code of the phone number's country\.
+ `message-type` – The type of messages that the phone number is used to send\. Possible values are `TRANSACTIONAL` or `PROMOTIONAL`\.
+ `number-capability` – The messaging channels that the phone number supports\. Possible values are `SMS` and `VOICE`\.
+ `number-type` – The type of phone number, such as `LONG_CODE`, `SHORT_CODE` or `TOLL_FREE`\.
+ `two-way-enabled` – A boolean that indicates whether or not two\-way SMS messaging is enabled\.
+ `self-managed-opt-outs-enabled` – A boolean that indicates whether or not self\-managed SMS opt\-outs are enabled\.
+ `opt-out-list-name` – The name of the opt\-out list associated with the phone number\.
+ `deletion-protection-enabled` – A boolean that indicates whether or not the phone number can be deleted using the `DeletePhoneNumber` operation\.

## Releasing phone numbers<a name="sms-voice-v2-phone-numbers-releasing"></a>

You can use the [ReleasePhoneNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_ReleasePhoneNumber.html) API to *release* origination phone numbers from your account\. When you release a phone number, that phone number is no longer available in your account, and you are no longer charged for it\.

**Important**  
Releasing a phone number can't be undone\. If you release a phone number, it isn't possible to get the same phone number back\.

**To release a phone number**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice release-phone-number \
  > --phone-number-id phoneNumberId
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVPhoneNumber -PhoneNumberId phoneNumberId
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice release-phone-number ^
   --phone-number-id phoneNumberId
  ```

------

  In the preceding command, replace *phoneNumberId* with the unique ID or Amazon Resource Name \(ARN\) of the phone number\.
**Tip**  
You can find both the ID and ARN of the phone number by completing the procedure in [Listing phone numbers](#sms-voice-v2-phone-numbers-listing)\.
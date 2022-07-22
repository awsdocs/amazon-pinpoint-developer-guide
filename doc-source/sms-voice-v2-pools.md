# Managing pools<a name="sms-voice-v2-pools"></a>

In the Amazon Pinpoint SMS and Voice API, version 2, you can use *pools* to create groups of origination phone numbers or sender IDs\. For example, by using pools, you can associate a list of opted\-out destination phone numbers with your origination phone numbers for a particular country\. By doing so, you can prevent messages from being sent to users who have already opted out of receiving messages from you\.

This section contains information about using the AWS CLI to manage pools in the Amazon Pinpoint SMS and Voice API, version 2\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Creating a pool](#sms-voice-v2-pools-creating)
+ [Adding origination identities to pools](#sms-voice-v2-pools-adding-ids)
+ [Listing pools](#sms-voice-v2-pools-describe)
+ [Listing the origination identities in a pool](#sms-voice-v2-pools-listing-ids)
+ [Deleting pools](#sms-voice-v2-pools-deleting)

## Creating a pool<a name="sms-voice-v2-pools-creating"></a>

You can use the [CreatePool](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreatePool.html) API to create new pools\.

When you create a new pool, you add an origination identity—either a phone number or a sender ID—to it\. Later, you can add more origination identities to it\. For more information about adding origination identities to an existing pool, see [Adding origination identities to pools](#sms-voice-v2-pools-adding-ids)\. You can also add a phone number to a pool when you use the `RequestPhoneNumber` API to purchase a phone number\. For more information, see [Requesting phone numbers](sms-voice-v2-phone-numbers.md#sms-voice-v2-phone-numbers-requesting)\.

**Note**  
The configuration of every origination identity that you add to a pool has to match the configuration of the first phone number that you specified when you create the pool\. For example, if you create a pool that contains a phone number that has two\-way messaging enabled, the other numbers that you add to the pool must also have two\-way messaging enabled\.

**To create a pool using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-pool \
  > --origination-identity originationIdentity \
  > --iso-country-code XX \
  > --message-type TRANSACTIONAL
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVPool `
  >> -OriginationIdentity originationIdentity `
  >> -IsoCountryCode XX `
  >> -MessageType TRANSACTIONAL
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-pool ^
   --origination-identity originationIdentity ^
   --iso-country-code XX ^
   --message-type TRANSACTIONAL
  ```

------

  In the preceding command, make the following changes:
  + Replace *originationIdentity* with the unique ID or Amazon Resource Name \(ARN\) of the phone number or sender ID that you want to add to the pool\.
**Tip**  
You can find both the ID and ARN of a phone number by using the [DescribePhoneNumbers](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribePhoneNumbers.html) operation\. You can find the ID and ARN of a sender ID by using the [DescribeSenderIds](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribeSenderIds.html) operation\.
  + Replace *XX* with the ISO\-3166 alpha\-2 identifier of the country for the pool\.
  + If you plan to use the pool to send marketing or promotional messages, replace *TRANSACTIONAL* with `PROMOTIONAL`\. Otherwise, use `TRANSACTIONAL`\.

## Adding origination identities to pools<a name="sms-voice-v2-pools-adding-ids"></a>

You can use the [AssociateOriginationIdentity](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_AssociateOriginationIdentity.html) API to add origination identities \(that is, phone numbers or sender IDs\) to an existing pool\.

The configuration of every origination identity that you add to a pool has to match the configuration of the first phone number that you specified when you created the pool\. For example, if you create a pool that contains a phone number that has two\-way messaging enabled, the other numbers that you add to the pool must also have two\-way messaging enabled\.

**To add an origination number to a pool using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 associate-origination-identity \
  > --pool-id poolId \
  > --origination-identity originationIdentity \
  > --iso-country-code US
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Register-SMSVOriginationIdentity `
  >> -PoolId poolId `
  >> -OriginationIdentity originationIdentity `
  >> -IsoCountryCode US
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 associate-origination-identity ^
   --pool-id poolId ^
   --origination-identity originationIdentity ^
   --iso-country-code US
  ```

------

  In the preceding command, make the following changes:
  + Replace *poolId* with the ID or Amazon Resource Name \(ARN\) of the pool that you want to add the origination identity to\.
  + Replace *originationIdentity* with the unique ID or Amazon Resource Name \(ARN\) of the phone number or sender ID that you want to add to the pool\.
**Tip**  
You can find both the ID and ARN of a phone number by using the [DescribePhoneNumbers](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribePhoneNumbers.html) operation\. You can find the ID and ARN of a sender ID by using the [DescribeSenderIds](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribeSenderIds.html) operation\.
  + Replace *\+12065550142* with the origination identity that you want to add to the pool\. This value can be a short code, a phone number, or a sender ID\.
  + Replace *US* with the two\-letter ISO\-3166 alpha\-2 code for the country of the origination identity\.

## Listing pools<a name="sms-voice-v2-pools-describe"></a>

You can use the [DescribePools](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribePools.html) API to view information about existing pools\.

This operation can provide a complete list of all of the pools in your Amazon Pinpoint account, information about a specific pool, or a list of pools that is filtered based on criteria that you define\.

**To retrieve a list of all of your pools using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-pools
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVPool
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-pools
  ```

------

To find information about specific pools, use the `PoolId` parameter\.

**To get information about specific pools using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-pools \
  > --pool-id poolId
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVPool -PoolId poolId
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-pools ^
   --pool-id poolId
  ```

------

To see a filtered list of pools, use the `Filters` parameter\. You can use the following filter values:
+ `status` – The current status of the pool, such as `ACTIVE`\.
+ `message-type` – The type of messages that the pool is used to send\. Possible values are `TRANSACTIONAL` or `PROMOTIONAL`\.
+ `two-way-enabled` – A boolean that indicates whether two\-way SMS messaging is enabled for numbers in the pool\.
+ `self-managed-opt-outs-enabled` – A boolean that indicates whether self\-managed SMS opt\-outs are enabled for numbers in the pool\.
+ `opt-out-list-name` – The name of the opt\-out list associated with the pool\.
+ `shared-routes-enabled` – A boolean that indicates whether shared routes are enabled for the pool\.
+ `deletion-protection-enabled` – A boolean that indicates whether or not the phone number can be deleted using the `DeletePhoneNumber` operation\.

For example, if you want to view a list of pools for transactional messages that support two\-way messaging, enter the following command at the command line:

------
#### [ Linux, macOS, or Unix ]

```
$ aws pinpoint-sms-voice-v2 describe-pools \
> --filters Name=message-type,Values=TRANSACTIONAL \
> --filters Name=two-way-enabled,Values=true
```

------
#### [ PowerShell ]

```
PS C:\> $filter1 = New-Object Amazon.PinpointSMSVoiceV2.Model.PoolFilter
PS C:\> $filter2 = New-Object Amazon.PinpointSMSVoiceV2.Model.PoolFilter
PS C:\> $filter1.Name = "message-type"; $filter.Values = "Transactional"
PS C:\> $filter2.Name = "two-way-enabled"; $filter.Values = "True"

PS C:\> Get-SMSVPool -Filter $filter1, $filter2
```

------
#### [ Windows Command Prompt ]

```
C:\> aws pinpoint-sms-voice-v2 describe-pools ^
 --filters Name=message-type,Values=TRANSACTIONAL ^
 --filters Name=two-way-enabled,Values=true
```

------

## Listing the origination identities in a pool<a name="sms-voice-v2-pools-listing-ids"></a>

You can use the [ListPoolOriginationIdentities](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_ListPoolOriginationIdentities.html) API to view information about all of the origination identities that have been added to a specific pool\.

**To view a list of origination IDs in a pool using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 list-pool-origination-identities \
  > --pool-id pool-78ec067f62f94d57bd3bab991example
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVPoolOriginationIdentityList `
  >> -PoolId pool-78ec067f62f94d57bd3bab991example
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-pools ^
   --pool-id pool-78ec067f62f94d57bd3bab991example
  ```

------

  , 

## Deleting pools<a name="sms-voice-v2-pools-deleting"></a>

You can use the [DeletePool](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DeletePool.html) API to delete pools\.

When you delete a pool, Amazon Pinpoint disassociates all origination identities from that pool, and then removes the pool itself\. However, the origination identities that were associated with the pool remain in your Amazon Pinpoint account\.

**To delete a pool using the AWS CLI**
+ To delete a pool, enter the following command at the command line:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 delete-pool \
  > --pool-id pool-78ec067f62f94d57bd3bab991example
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVPool -PoolId pool-78ec067f62f94d57bd3bab991example
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 delete-pool ^
   --pool-id pool-78ec067f62f94d57bd3bab991example
  ```

------

  In the preceding command, replace *pool\-78ec067f62f94d57bd3bab991example* with the unique ID or the Amazon Resource Name \(ARN\) of the pool\. You can find both of these values by using the [DescribePools](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribePools.html) operation\.
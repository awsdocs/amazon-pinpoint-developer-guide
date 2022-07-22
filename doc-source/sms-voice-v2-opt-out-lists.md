# Managing opt\-out lists<a name="sms-voice-v2-opt-out-lists"></a>

An *opt\-out list* is list of destination identities that should not have messages sent to them\. When you send SMS messages, destination identities are automatically added to the opt\-out list if they reply to your origination number with the keyword STOP \(unless you enable the self\-managed opt\-out option\)\. If you attempt to send a message to a destination number that is on an opt\-out list, and the opt\-out list is associated with the pool used to send the message, Amazon Pinpoint doesn't attempt to send the message\.

This section contains information about using the AWS CLI to manage opt\-out lists in the Amazon Pinpoint SMS and Voice API, version 2\. The procedures in this section assume that you've already configured the AWS CLI\. For more information, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Creating an opt\-out list](#sms-voice-v2-opt-out-lists-creating)
+ [Listing the opt\-out lists in your account](#sms-voice-v2-opt-out-lists-describing)
+ [Adding destination numbers to an opt\-out list](#sms-voice-v2-opt-out-lists-putting-destination-ids)
+ [Removing destination numbers from an opt\-out list](#sms-voice-v2-opt-out-lists-removing-destination-ids)

## Creating an opt\-out list<a name="sms-voice-v2-opt-out-lists-creating"></a>

You can use the [CreateOptOutList](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_CreateOptOutList.html) API to create a new opt\-out list\. After you create an opt\-out list, you can [add destination identities to it](#sms-voice-v2-opt-out-lists-putting-destination-ids)\.

**To create an opt\-out list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 create-opt-out-list \
  > --opt-out-list-name optOutListName
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> New-SMSVOptOutList -OptOutListName optOutListName
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 create-opt-out-list ^
   --opt-out-list-name optOutListName
  ```

------

  In the preceding example, replace *optOutListName* with a name that makes the opt\-out list easy to identify\.

  The AWS CLI returns the following information about the opt\-out list:

  ```
  {
      "OptOutListArn": "arn:aws:sms-voice:us-east-1:111122223333:opt-out-list/optOutListName",
      "OptOutListName": "optOutListName",
      "CreatedTimestamp": 1645568542.0
  }
  ```

## Listing the opt\-out lists in your account<a name="sms-voice-v2-opt-out-lists-describing"></a>

You can use the [DescribeOptOutLists](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DescribeOptOutLists.html) API to view information about the opt\-out lists in your Amazon Pinpoint account\.

**To view information about all of your opt\-out lists using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-opt-out-lists
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVOptOutList
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-opt-out-lists
  ```

------

You can also view information about specific opt\-out lists by using the `OptOutListNames` parameter\.

**To view information about specific opt\-out lists using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 describe-opt-out-lists \
  > --opt-out-list-names optOutListName
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Get-SMSVOptOutList -OptOutListName optOutListName
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 describe-opt-out-lists ^
   --opt-out-list-names optOutListName
  ```

------

  In the preceding command, replace *optOutListName* with the name or Amazon Resource Name \(ARN\) of the opt\-out list that you want to find more information about\. You can also specify multiple opt\-out lists by separating each list name with a space\.

  The AWS CLI returns the following information about all of the opt\-out lists in your account\.

## Adding destination numbers to an opt\-out list<a name="sms-voice-v2-opt-out-lists-putting-destination-ids"></a>

You can use the [PutOptedOutNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_PutOptedOutNumber.html) API to add a destination phone number to an opt\-out list\.

**To add a phone number to an opt\-out list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 put-opted-out-number \
  > --opt-out-list-name optOutListName \
  > --opted-out-number +12065550123
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Set-SMSVOptedOutNumber `
  >> -OptOutListName optOutListName `
  >> -OptedOutNumber +12065550123
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 put-opted-out-number ^
   --opt-out-list-name optOutListName ^
   --opted-out-number +12065550123
  ```

------

  In the preceding example, make the following changes:
  + Replace *optOutListName* with the name or Amazon Resource Name \(ARN\) of the opt\-out list that you want to add the destination identity to\.
  + Replace *\+12065550123* with phone number that you want to add to the opt\-out list\. The phone number must be formatted in E\.164 format\.

  The AWS CLI returns the following information about the opt\-out list:

  ```
  {
      "OptOutListArn": "arn:aws:sms-voice:us-east-1:111122223333:opt-out-list/optOutListName",
      "OptOutListName": "optOutListName",
      "OptedOutNumber": "+12065550123",
      "OptedOutTimestamp": 1645568542.0,
      "EndUserOptedOut": false
  }
  ```

## Removing destination numbers from an opt\-out list<a name="sms-voice-v2-opt-out-lists-removing-destination-ids"></a>

You can use the [DeleteOptedOutNumber](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_DeleteOptedOutNumber.html) API to remove a destination phone number from an opt\-out list\.

**Note**  
A destination phone number can only be removed from an opt\-out list once every 30 days\.

**To remove a phone number from an opt\-out list using the AWS CLI**
+ At the command line, enter the following command:

------
#### [ Linux, macOS, or Unix ]

  ```
  $ aws pinpoint-sms-voice-v2 delete-opted-out-number \
  > --opt-out-list-name optOutListName \
  > --opted-out-number +12065550123
  ```

------
#### [ PowerShell ]

  ```
  PS C:\> Remove-SMSVOptedOutNumber `
  >> -OptOutListName optOutListName `
  >> -OptedOutNumber +12065550123
  ```

------
#### [ Windows Command Prompt ]

  ```
  C:\> aws pinpoint-sms-voice-v2 delete-opted-out-number ^
   --opt-out-list-name optOutListName ^
   --opted-out-number +12065550123
  ```

------

  In the preceding example, make the following changes:
  + Replace *optOutListName* with the name or Amazon Resource Name \(ARN\) of the opt\-out list that you want to remove the destination identity from\.
  + Replace *\+12065550123* with phone number that you want to remove from the opt\-out list\. The phone number must be formatted in E\.164 format\.

  The AWS CLI returns the following information about the opt\-out list:

  ```
  {
      "OptOutListArn": "arn:aws:sms-voice:us-east-1:111122223333:opt-out-list/optOutListName",
      "OptOutListName": "optOutListName",
      "OptedOutNumber": "+12065550123",
      "OptedOutTimestamp": 1645568542.0,
      "EndUserOptedOut": false
  }
  ```
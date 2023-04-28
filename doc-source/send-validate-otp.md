# Sending and validating One\-Time Passwords \(OTPs\)<a name="send-validate-otp"></a>

Amazon Pinpoint includes a One\-Time Password \(OTP\) management feature\. You can use this feature to generate new one\-time passwords and send them to your recipients as SMS messages\. Your applications can then call the Amazon Pinpoint API to verify these passwords\.

**Note**  
To use this feature, your account must have production SMS access\. For more information, see [About the Amazon Pinpoint SMS sandbox](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-sandbox.html) in the *Amazon Pinpoint User Guide*\.  
In some countries and regions, you must obtain a dedicated phone number or origination ID before you can send SMS messages\. For example, when you send messages to the recipients in the United States, you must have a dedicated toll\-free number, 10DLC number, or short code\. When you send messages to recipients in India, you must have a registered sender ID, which includes a Principal Entity ID \(PEID\) and a Template ID\. These requirements still apply when you use the OTP feature\.  
To use this feature you need permissions to send and verify OTP messages, see [One\-time passwords](permissions-actions.md#permissions-actions-apiactions-otp)\. If you need help determining permissions see [Troubleshooting Amazon Pinpoint identity and access management](security_iam_troubleshoot.md)\.

## Sending an OTP message<a name="send-validate-otp-sending"></a>

You can use the `SendOtpMessages` operation in the Amazon Pinpoint API to send an OTP code to a user of your application\. When you use this API, Amazon Pinpoint generates a random code and sends it to your user as an SMS message\. Your request must include the following parameters:
+ `Channel` – The communication channel that the OTP code is sent through\. Currently, only SMS messages are supported, so the only acceptable value is SMS\.
+ `BrandName` – The name of the brand, company, or product that is associated with the OTP code\. This name can contain up to 20 characters\.
**Note**  
When Amazon Pinpoint sends the OTP message, the brand name is automatically inserted into the following message template:  

  ```
  This is your One Time Password: {{otp}} from {{brand}}
  ```
So, if you specify ExampleCorp as your brand name, and Amazon Pinpoint generates a one\-time password of 123456, it sends the following message to your user:  

  ```
  This is your One Time Password: 123456 from ExampleCorp
  ```
+ `CodeLength` – The number of digits that will be in the OTP code that's sent to the recipient\. OTP codes can contain between 5 and 8 digits, inclusive\.
+ `ValidityPeriod` – The amount of time, in minutes, that the OTP code will be valid\. The validity period can be between 5 and 60 minutes, inclusive\.
+ `AllowedAttempts` – The number of times the recipient can unsuccessfully attempt to verify the OTP\. If the number of attempts exceeds this value, the OTP automatically becomes invalid\. The maximum number of allowed attempts is 5\.
+ `Language` – The language, in IETF BCP\-47 format, to use when sending the message\. Acceptable values are:
  + `de-DE` – German
  + `en-GB` – English \(UK\)
  + `en-US` – English \(US\)
  + `es-419` – Spanish \(Latin America\)
  + `es-ES` – Spanish
  + `fr-CA` – French \(Canada\)
  + `fr-FR` – French
  + `it-IT` – Italian
  + `ja-JP` – Japanese
  + `ko-KR` – Korean
  + `pt-BR` – Portuguese \(Brazil\)
  + `zh-CN` – Chinese \(Simplified\)
  + `zh-TW` – Chinese \(Traditional\)
+ `OriginationIdentity` – The originating identity \(such as a long code, short code, or sender ID\) that is used to send the OTP code\. If you use a long code or toll\-free number to send the OTP, the phone number must be in E\.164 format\.
+ `DestinationIdentity` – The phone number, in E\.164 format, that the OTP code was sent to\. 
+ `ReferenceId` – A unique reference ID for the request\. The reference ID exactly match the reference ID that you provide when you verify the OTP\. The reference ID can contain between 1 and 48 characters, inclusive\.
+ `EntityId` – An Entity ID that is registered with a regulatory agency\. This parameter is currently only used when sending messages to recipients in India\. If you aren't sending to recipients in India, you can omit this parameter\.
+ `TemplateId` – A Template ID that is registered with a regulatory agency\. This parameter is currently only used when sending messages to recipients in India\. If you aren't sending to recipients in India, you can omit this parameter\.
**Note**  
For more information about the requirements for sending messages to recipients in India, see [Special requirements for sending SMS messages to recipients in India](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-senderid-india.html) in the *Amazon Pinpoint User Guide*\.

To ensure that your Amazon Pinpoint account is properly configured to send OTP messages, you can use the AWS CLI to send a test message\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

------
#### [ Linux, macOS, or Unix ]

To send a test OTP message using the AWS CLI, run the [send\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/send-otp-message.html) command in the terminal:

```
aws pinpoint send-otp-message \
--application-id 7353f53e6885409fa32d07cedexample \
--send-otp-message-request '{
  "Channel": "SMS",
  "BrandName": "ExampleCorp",
  "CodeLength": 5,
  "ValidityPeriod": 20,
  "AllowedAttempts": 5,
  "OriginationIdentity": "+18555550142",
  "DestinationIdentity": "+12065550007",
  "ReferenceId": "SampleReferenceId"
}'
```

------
#### [ PowerShell ]

To send a test OTP message using the AWS CLI, run the [send\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/send-otp-message.html) command in PowerShell:

```
aws pinpoint send-otp-message `
--application-id 7353f53e6885409fa32d07cedexample `
--send-otp-message-request '{ `
  "Channel": "SMS", `
  "BrandName": "ExampleCorp", `
  "CodeLength": 5, `
  "ValidityPeriod": 20, `
  "AllowedAttempts": 5, `
  "OriginationIdentity": "+18555550142", `
  "DestinationIdentity": "+12065550007", `
  "ReferenceId": "SampleReferenceId"}'
```

------
#### [ Windows command prompt ]

To send a test OTP message using the AWS CLI, run the [send\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/send-otp-message.html) command in the Windows command prompt:

```
aws pinpoint send-otp-message ^
--application-id 7353f53e6885409fa32d07cedexample ^
--send-otp-message-request '{ ^
  "Channel": "SMS", ^
  "BrandName": "ExampleCorp", ^
  "CodeLength": 5, ^
  "ValidityPeriod": 20, ^
  "AllowedAttempts": 5, ^
  "OriginationIdentity": "+18555550142", ^
  "DestinationIdentity": "+12065550007", ^
  "ReferenceId": "SampleReferenceId" ^
}'
```

------

### `SendOtpMessage` Response<a name="send-validate-otp-sending-response"></a>

When you successfully send an OTP message, you receive a response that resembles the following example:

```
{
    "MessageResponse": {
        "ApplicationId": "7353f53e6885409fa32d07cedexample",
        "RequestId": "255d15ea-75fe-4040-b919-096f2example",
        "Result": {
            "+12065550007": {
                "DeliveryStatus": "SUCCESSFUL",
                "MessageId": "nvrmgq9kq4en96qgp0tlqli3og1at6aexample",
                "StatusCode": 200,
                "StatusMessage": "MessageId: nvrmgq9kq4en96qgp0tlqli3og1at6aexample"
            }
        }
    }
}
```

## Validating an OTP message<a name="send-validate-otp-validating"></a>

To verify an OTP code, call the `VerifyOtpMessages` API\. Your request must include the following parameters:
+ `DestinationIdentity` – The phone number, in E\.164 format, that the OTP code was sent to\.
+ `ReferenceId` – The reference ID that you used when you sent the OTP code to the recipient\. The reference ID must be an exact match\.
+ `Otp` – The OTP code that you are validating\.

You can use the AWS CLI to test the validation process\. For more information about installing and configuring the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

------
#### [ Linux, macOS, or Unix ]

To verify an OTP using the AWS CLI, run the [verify\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/verify-otp-message.html) command in the terminal:

```
aws pinpoint verify-otp-message \
--application-id 7353f53e6885409fa32d07cedexample \
--verify-otp-message-request-parameters '{
  "DestinationIdentity": "+12065550007",
  "ReferenceId": "SampleReferenceId",
  "Otp": "012345"
}'
```

------
#### [ PowerShell ]

To verify an OTP using the AWS CLI, run the [verify\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/verify-otp-message.html) command in PowerShell:

```
aws pinpoint verify-otp-message \`
--application-id 7353f53e6885409fa32d07cedexample \`
--verify-otp-message-request-parameters '{`
  "DestinationIdentity": "+12065550007",`
  "ReferenceId": "SampleReferenceId",`
  "Otp": "012345"}'
```

------
#### [ Windows command prompt ]

To verify an OTP using the AWS CLI, run the [verify\-otp\-message](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/verify-otp-message.html) command in the Windows command prompt:

```
aws pinpoint verify-otp-message \^
--application-id 7353f53e6885409fa32d07cedexample \^
--verify-otp-message-request-parameters '{^
  "DestinationIdentity": "+12065550007",^
  "ReferenceId": "SampleReferenceId",^
  "Otp": "012345"}'
```

------

### `VerifyOtpMessage` Response<a name="send-validate-otp-validating-response"></a>

When you send a request to the `VerifyOTPMessage` API, it returns a `VerificationResponse` object, which contains a single property, `Valid`\. If the reference ID, phone number, and OTP all match the values that Amazon Pinpoint expects, and if the OTP hasn't expired, the value of `Valid` is `true`; otherwise, it is `false`\. The following is an example of response for a successful OTP verification:

```
{
    "VerificationResponse": {
        "Valid": true
    }
}
```

## Code examples<a name="send-validate-otp-examples"></a>

This section contains code examples that show how to use the SDK for Python \(Boto3\) to send and verify OTP codes\. 

### Generating a reference ID<a name="send-validate-otp-examples-refid"></a>

The following function generates a unique reference ID for each recipient, based on the recipient's phone number, the product or brand that the recipient is receiving an OTP for, and the source of the request \(which could be the name of a page in a site or app, for example\)\. When you verify the OTP code, you must pass an identical reference ID in order for the validation to succeed\. Both the sending and validation code examples use this utility function\. 

This function isn't required, but it is a useful way to scope the OTP sending and verification process to a specific transaction in a way that can be easily re\-submitted during the verification step\. You can use any reference ID you want—this is just a basic example\. However, the other code examples in this section rely on this function\.

```
# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier:  Apache-2.0

import hashlib
 
def generate_ref_id(destinationNumber,brandName,source):
    refId = brandName + source + destinationNumber
    return hashlib.md5(refId.encode()).hexdigest()
```

### Sending OTP codes<a name="send-validate-otp-examples-send"></a>

The following code example shows you how to use the SDK for Python \(Boto3\) to send an OTP code\.

```
# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier:  Apache-2.0

import boto3
from botocore.exceptions import ClientError
from generate_ref_id import generate_ref_id

### Some variables that are unlikely to change from request to request. ###

# The AWS Region that you want to use to send the message.
region = "us-east-1"

# The phone number or short code to send the message from.
originationNumber = "+18555550142"

# The project/application ID to use when you send the message.
appId = "7353f53e6885409fa32d07cedexample"

# The number of times the user can unsuccessfully enter the OTP code before it becomes invalid.
allowedAttempts = 3

# Function that sends the OTP as an SMS message.
def send_otp(destinationNumber,codeLength,validityPeriod,brandName,source,language):
    client = boto3.client('pinpoint',region_name=region)
    try:
        response = client.send_otp_message(
            ApplicationId=appId,
            SendOTPMessageRequestParameters={
                'Channel': 'SMS',
                'BrandName': brandName,
                'CodeLength': codeLength,
                'ValidityPeriod': validityPeriod,
                'AllowedAttempts': allowedAttempts,
                'Language': language,
                'OriginationIdentity': originationNumber,
                'DestinationIdentity': destinationNumber,
                'ReferenceId': generate_ref_id(destinationNumber,brandName,source)
            }
        )

    except ClientError as e:
        print(e.response)
    else:
        print(response)

# Send a message to +14255550142 that contains a 6-digit OTP that is valid for 15 minutes. The
# message will include the brand name "ExampleCorp", and the request originated from a part of your
# site or application called "CreateAccount". The US English message template should be used to
# send the message.
send_otp("+14255550142",6,15,"ExampleCorp","CreateAccount","en-US")
```

### Validating OTP codes<a name="send-validate-otp-examples-validate"></a>

The following code example shows you how to use the SDK for Python \(Boto3\) to verify an OTP code that you've already sent\. In order for the validation step to succeed, your request must include a reference ID that exactly matches the reference ID that was used to send the message\.

```
# Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier:  Apache-2.0

import boto3
from botocore.exceptions import ClientError
from generate_ref_id import generate_ref_id

# The AWS Region that you want to use to send the message.
region = "us-east-1"

# The project/application ID to use when you send the message.
appId = "7353f53e6885409fa32d07cedexample"

# Function that verifies the OTP code.
def verify_otp(destinationNumber,otp,brandName,source):
    client = boto3.client('pinpoint',region_name=region)
    try:
        response = client.verify_otp_message(
            ApplicationId=appId,
            VerifyOTPMessageRequestParameters={
                'DestinationIdentity': destinationNumber,
                'ReferenceId': generate_ref_id(destinationNumber,brandName,source),
                'Otp': otp
            }
        )

    except ClientError as e:
        print(e.response)
    else:
        print(response)

# Verify the OTP 012345, which was sent to +14255550142. The brand name ("ExampleCorp") and the
# source name ("CreateAccount") are used to generate the correct reference ID.
verify_otp("+14255550142","012345","ExampleCorp","CreateAccount")
```
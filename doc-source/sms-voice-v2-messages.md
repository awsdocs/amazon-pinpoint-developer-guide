# Sending messages using the SMS and Voice API<a name="sms-voice-v2-messages"></a>

You can use the Amazon Pinpoint SMS and Voice API version 2 to send *transactional* messages directly from your apps\. Transactional messages are messages that you send to specific recipients, as opposed to messages that you send via campaigns or journeys\.

This section includes code examples for sending both [SMS messages](#sms-voice-v2-messages-sms) and [voice messages](#sms-voice-v2-messages-voice)\.

**Topics**
+ [Sending SMS Messages](#sms-voice-v2-messages-sms)
+ [Sending Voice Messages](#sms-voice-v2-messages-voice)

## Sending SMS Messages<a name="sms-voice-v2-messages-sms"></a>

You can use the following code example to send an SMS message using the AWS SDK for Python \(Boto3\)\.

```
import boto3
from botocore.exceptions import ClientError


def send_sms_message(sms_voice_v2_client, configuration_set, context_keys,
                     country_parameters, destination_number, dry_run, keyword,
                     max_price, message_body, message_type, origination_number,
                     ttl):
    try:
        response = sms_voice_v2_client.send_text_message(
            ConfigurationSetName=configuration_set,
            Context=context_keys,
            DestinationCountryParameters=country_parameters,
            DestinationPhoneNumber=destination_number,
            DryRun=dry_run,
            Keyword=keyword,
            MaxPrice=max_price,
            MessageBody=message_body,
            MessageType=message_type,
            OriginationIdentity=origination_number,
            TimeToLive=ttl
        )

    except ClientError as e:
        print(e.response)
    else:
        return response['MessageId']


def main():
    configuration_set = "MyConfigurationSet"
    context_keys = {"key1": "value1"}
    country_parameters = {
        "IN_TEMPLATE_ID": "TEMPLATE01234",
        "IN_ENTITY_ID": "ENTITY98765"
    }
    destination_number = "+14258918757"
    dry_run = False
    keyword = "MyKeyword"
    max_price = "2.00"
    message_body = ("This is a test message sent from Amazon Pinpoint "
                    "using the AWS SDK for Python (Boto3). ")
    message_type = "TRANSACTIONAL"
    origination_number = "+18449831743"
    ttl = 120

    print(
        f"Sending text message to {destination_number}.")

    message_id = send_sms_message(
        boto3.client('pinpoint-sms-voice-v2'), configuration_set, context_keys,
        country_parameters, destination_number, dry_run, keyword, max_price,
        message_body, message_type, origination_number, ttl)

    print(f"Message sent!\nMessage ID: {message_id}")


if __name__ == '__main__':
    main()
```

In the preceding code example, make the following changes in the `main()` function:
+ Change the value of `configuration_set` to the name or Amazon Resource Name \(ARN\) of the configuration set that you want to use to send this message\.
+ Change the value of `context_keys` to the keys and values that you want to use when sending this message\. These keys appear in the event records associated with this message\.
+ If you use a registered sender ID to send messages to customers in India, change the value of `country_parameters` to match the registered Entity ID and Template ID that you received when you registered your sender ID\.
**Important**  
If you don't use a registered sender ID to send messages to customers in India, omit this parameter completely\. If you do, you must also remove the corresponding line in the `send_sms_message` function\.
+ Change the value of `destination_number` to the phone number that you want to send the message to\.
+ If you want to execute this operation without sending any messages, change the value of `dry_run` to `True`\.
+ Change the value of `max_price` to the maximum amount of money that you want to spend, in US Dollars, to send each message part this message\. A message part contains up to 140 bytes of information\. For more information, see [SMS character limits](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-limitations-characters.html) in the *Amazon Pinpoint User Guide*\.
+ Change the value of `message_body` to include the message that you want to send\. The maximum length of a message depends on which characters the message contains\. For more information about SMS character encoding, see [SMS character limits](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-managing.html#settings-account-sms-self-managed-opt-out)in the *Amazon Pinpoint User Guide*\.
+ Change the value of `message_type` to represent the appropriate message category\. Valid values are TRANSACTIONAL \(for messages that are critical or time\-sensitive\) and PROMOTIONAL \(for messages that aren't critical or time\-sensitive\)\.
+ Change the value of `origination_number` to the phone number that you want to use to send the message\. The phone number must be in E\.164 format\.
+ Change the value of `ttl` to the amount of time, in seconds, that Amazon Pinpoint should attempt to deliver the message\. You can set the TTL value up to 259200 seconds \(72 hours\)\.

## Sending Voice Messages<a name="sms-voice-v2-messages-voice"></a>

You can use the following code example to send a voice message using the AWS SDK for Python \(Boto3\)\.

```
import boto3
from botocore.exceptions import ClientError


def send_voice_message(sms_voice_v2_client, configuration_set, context_keys,
                       destination_number, dry_run, max_price, message_body,
                       message_type, origination_number, ttl, voice_id):
    try:
        response = sms_voice_v2_client.send_voice_message(
            ConfigurationSetName=configuration_set,
            Context=context_keys,
            DestinationPhoneNumber=destination_number,
            DryRun=dry_run,
            MaxPricePerMinute=max_price,
            MessageBody=message_body,
            MessageBodyTextType=message_type,
            OriginationIdentity=origination_number,
            TimeToLive=ttl,
            VoiceId=voice_id
        )

    except ClientError as e:
        print(e.response)
    else:
        return response['MessageId']


def main():
    configuration_set = "MyConfigurationSet"
    context_keys = {"key1":"value1"}
    destination_number = "+12065550123"
    dry_run = False
    max_price = "2.00"
    message_body = (
        "<speak>"
        "This is a test message sent from <emphasis>Amazon Pinpoint</emphasis>"
        "using the <break strength='weak'/> AWS SDK for Python (Boto3). "
        "<amazon:effect phonation='soft'>Thank you for listening."
        "</amazon:effect>"
        "</speak>")
    message_type = "SSML"
    origination_number = "+18445550142"
    ttl = 120
    voice_id = "MATTHEW"

    print(
        f"Sending voice message from {origination_number} to {destination_number}.")

    message_id = send_voice_message(
        boto3.client('pinpoint-sms-voice-v2'), configuration_set, context_keys,
        destination_number, dry_run, max_price, message_body, message_type,
        origination_number, ttl, voice_id)

    print(f"Message sent!\nMessage ID: {message_id}")


if __name__ == '__main__':
    main()
```

In the preceding code example, make the following changes in the `main()` function:
+ Change the value of `configuration_set` to the name or Amazon Resource Name \(ARN\) of the configuration set that you want to use to send this message\.
+ Change the value of `context_keys` to the keys and values that you want to use when sending this message\. These keys appear in the event records associated with this message\.
+ Change the value of `destination_number` to the phone number that you want to send the message to\.
+ Change the value of `max_price` to the maximum amount of money that you want to spend per minute sending this message\.
+ Change the value of `message_body` to include the message that you want to send\. Your message can contain up to 6,000 characters\.
+ If you want to use a plain text script rather than an SSML\-formatted one, change the value of `message_type` to `TEXT`\.
+ Change the value of `origination_number` to the phone number that you want to use to send the message\. The phone number must be in E\.164 format\.
+ If you want to execute this operation without sending any messages, change the value of `dry_run` to `True`\.
+ Change the value of `ttl` to the amount of time, in seconds, that Amazon Pinpoint should attempt to deliver the message\. You can set the TTL value up to 259200 seconds \(72 hours\)\.
+ Replace `MATTHEW` with the name of the Amazon Polly voice that you want to use to send the message\. For a complete list of supported voices, see [SendVoiceMessage](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/API_SendVoiceMessage.html#pinpoint-SendVoiceMessage-request-VoiceId) in the *SMS and Voice, version 2 API Reference*\. If you don't specify a voice, your message is sent using the "MATTHEW" voice\.
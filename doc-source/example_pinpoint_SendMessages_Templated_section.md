# Send templated email and text messages with Amazon Pinpoint using an AWS SDK<a name="example_pinpoint_SendMessages_Templated_section"></a>

The following code example shows how to send templated email and text messages with Amazon Pinpoint\.

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
Send an email message with an existing email template\.  

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


def send_templated_email_message(
        pinpoint_client, project_id, sender, to_addresses, template_name,
        template_version):
    """
    Sends an email message with HTML and plain text versions.

    :param pinpoint_client: A Boto3 Pinpoint client.
    :param project_id: The Amazon Pinpoint project ID to use when you send this message.
    :param sender: The "From" address. This address must be verified in
                   Amazon Pinpoint in the AWS Region you're using to send email.
    :param to_addresses: The addresses on the "To" line. If your Amazon Pinpoint
                         account is in the sandbox, these addresses must be verified.
    :param template_name: The name of the email template to use when sending the message.
    :param template_version: The version number of the message template.

    :return: A dict of to_addresses and their message IDs.
    """
    try:
        response = pinpoint_client.send_messages(
            ApplicationId=project_id,
            MessageRequest={
                'Addresses': {
                    to_address: {
                        'ChannelType': 'EMAIL'
                    } for to_address in to_addresses
                },
                'MessageConfiguration': {
                    'EmailMessage': {'FromAddress': sender}
                },
                'TemplateConfiguration': {
                    'EmailTemplate': {
                        'Name': template_name,
                        'Version': template_version
                    }
                }
            }
        )
    except ClientError:
        logger.exception("Couldn't send email.")
        raise
    else:
        return {
            to_address: message['MessageId'] for
            to_address, message in response['MessageResponse']['Result'].items()
        }


def main():
    project_id = "296b04b342374fceb661bf494example"
    sender = "sender@example.com"
    to_addresses = ["recipient@example.com"]
    template_name = "My_Email_Template"
    template_version = "1"

    print("Sending email.")
    message_ids = send_templated_email_message(
        boto3.client('pinpoint'), project_id, sender, to_addresses, template_name,
        template_version)
    print(f"Message sent! Message IDs: {message_ids}")


if __name__ == '__main__':
    main()
```
Send a text message with an existing SMS template\.  

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


def send_templated_sms_message(
        pinpoint_client,
        project_id,
        destination_number,
        message_type,
        origination_number,
        template_name,
        template_version):
    """
    Sends an SMS message to a specific phone number using a pre-defined template.

    :param pinpoint_client: A Boto3 Pinpoint client.
    :param project_id: An Amazon Pinpoint project (application) ID.
    :param destination_number: The phone number to send the message to.
    :param message_type: The type of SMS message (promotional or transactional).
    :param origination_number: The phone number that the message is sent from.
    :param template_name: The name of the SMS template to use when sending the message.
    :param template_version: The version number of the message template.

    :return The ID of the message.
    """
    try:
        response = pinpoint_client.send_messages(
            ApplicationId=project_id,
            MessageRequest={
                'Addresses': {
                    destination_number: {
                        'ChannelType': 'SMS'
                    }
                },
                'MessageConfiguration': {
                    'SMSMessage': {
                        'MessageType': message_type,
                        'OriginationNumber': origination_number
                    }
                },
                'TemplateConfiguration': {
                    'SMSTemplate': {
                        'Name': template_name,
                        'Version': template_version
                    }
                }
            }
        )

    except ClientError:
        logger.exception("Couldn't send message.")
        raise
    else:
        return response['MessageResponse']['Result'][destination_number]['MessageId']


def main():
    region = "us-east-1"
    origination_number = "+18555550001"
    destination_number = "+14255550142"
    project_id = "7353f53e6885409fa32d07cedexample"
    message_type = "TRANSACTIONAL"
    template_name = "My_SMS_Template"
    template_version = "1"
    message_id = send_templated_sms_message(
        boto3.client('pinpoint', region_name=region), project_id,
        destination_number, message_type, origination_number, template_name,
        template_version)
    print(f"Message sent! Message ID: {message_id}.")


if __name__ == '__main__':
    main()
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/pinpoint#code-examples)\. 
+  For API details, see [SendMessages](https://docs.aws.amazon.com/goto/boto3/pinpoint-2016-12-01/SendMessages) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
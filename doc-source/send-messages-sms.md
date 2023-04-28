# Send SMS messages<a name="send-messages-sms"></a>

You can use the Amazon Pinpoint API to send SMS messages \(text messages\) to specific phone numbers or endpoint IDs\. This section contains complete code examples that you can use to send SMS messages through the Amazon Pinpoint API by using an AWS SDK\.

------
#### [ C\# ]

Use this example to send an SMS message by using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. This example assumes that you've already installed and configured the AWS SDK for \.NET\. For more information, see [Getting started ](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Configuring AWS credentials](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

```
using System;
using System.Collections.Generic;
using Amazon;
using Amazon.Pinpoint;
using Amazon.Pinpoint.Model;

namespace SendMessage
{
    class MainClass
    {
        // The AWS Region that you want to use to send the message. For a list of
        // AWS Regions where the Amazon Pinpoint API is available, see
        // https://docs.aws.amazon.com/pinpoint/latest/apireference/
        private static readonly string region = "us-east-1";

        // The phone number or short code to send the message from. The phone number
        // or short code that you specify has to be associated with your Amazon Pinpoint
        // account. For best results, specify long codes in E.164 format.
        private static readonly string originationNumber = "+12065550199";

        // The recipient's phone number.  For best results, you should specify the
        // phone number in E.164 format.
        private static readonly string destinationNumber = "+14255550142";

        // The content of the SMS message.
        private static readonly string message = "This message was sent through Amazon Pinpoint"
                + "using the AWS SDK for .NET. Reply STOP to opt out.";

        // The Pinpoint project/application ID to use when you send this message.
        // Make sure that the SMS channel is enabled for the project or application
        // that you choose.
        private static readonly string appId = "ce796be37f32f178af652b26eexample";

        // The type of SMS message that you want to send. If you plan to send
        // time-sensitive content, specify TRANSACTIONAL. If you plan to send
        // marketing-related content, specify PROMOTIONAL.
        private static readonly string messageType = "TRANSACTIONAL";

        // The registered keyword associated with the originating short code.
        private static readonly string registeredKeyword = "myKeyword";

        // The sender ID to use when sending the message. Support for sender ID
        // varies by country or region. For more information, see
        // https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-countries.html
        private static readonly string senderId = "mySenderId";

        public static void Main(string[] args)
        {
            using (AmazonPinpointClient client = new AmazonPinpointClient(RegionEndpoint.GetBySystemName(region)))
            {
                SendMessagesRequest sendRequest = new SendMessagesRequest
                {
                    ApplicationId = appId,
                    MessageRequest = new MessageRequest
                    {
                        Addresses = new Dictionary<string, AddressConfiguration>
                        {
                            {
                                destinationNumber,
                                new AddressConfiguration
                                {
                                    ChannelType = "SMS"
                                }
                            }
                        },
                        MessageConfiguration = new DirectMessageConfiguration
                        {
                            SMSMessage = new SMSMessage
                            {
                                Body = message,
                                MessageType = messageType,
                                OriginationNumber = originationNumber,
                                SenderId = senderId,
                                Keyword = registeredKeyword
                            }
                        }
                    }
                };
                try
                {
                    Console.WriteLine("Sending message...");
                    SendMessagesResponse response = client.SendMessages(sendRequest);
                    Console.WriteLine("Message sent!");
                }
                catch (Exception ex)
                {
                    Console.WriteLine("The message wasn't sent. Error message: " + ex.Message);
                }
            }
        }
    }
}
```

------
#### [ Java ]

Use this example to send an SMS message by using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This example assumes that you've already installed and configured the SDK for Java\. For more information, see [Getting started](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/getting-started.html) in the *AWS SDK for Java Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Set default credentials and Region](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html#setup-credentials) in the *AWS SDK for Java Developer Guide*\.

```
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpoint.PinpointClient;
import software.amazon.awssdk.services.pinpoint.model.DirectMessageConfiguration;
import software.amazon.awssdk.services.pinpoint.model.SMSMessage;
import software.amazon.awssdk.services.pinpoint.model.AddressConfiguration;
import software.amazon.awssdk.services.pinpoint.model.ChannelType;
import software.amazon.awssdk.services.pinpoint.model.MessageRequest;
import software.amazon.awssdk.services.pinpoint.model.SendMessagesRequest;
import software.amazon.awssdk.services.pinpoint.model.SendMessagesResponse;
import software.amazon.awssdk.services.pinpoint.model.MessageResponse;
import software.amazon.awssdk.services.pinpoint.model.PinpointException;
import java.util.HashMap;
import java.util.Map;
```

```
    public static void sendSMSMessage(PinpointClient pinpoint, String message, String appId, String originationNumber, String destinationNumber) {

        try {
            Map<String, AddressConfiguration> addressMap = new HashMap<String, AddressConfiguration>();
            AddressConfiguration addConfig = AddressConfiguration.builder()
                .channelType(ChannelType.SMS)
                .build();

            addressMap.put(destinationNumber, addConfig);
            SMSMessage smsMessage = SMSMessage.builder()
                .body(message)
                .messageType(messageType)
                .originationNumber(originationNumber)
                .senderId(senderId)
                .keyword(registeredKeyword)
                .build();

            // Create a DirectMessageConfiguration object.
            DirectMessageConfiguration direct = DirectMessageConfiguration.builder()
                .smsMessage(smsMessage)
                .build();

            MessageRequest msgReq = MessageRequest.builder()
                .addresses(addressMap)
                .messageConfiguration(direct)
                .build();

            // create a  SendMessagesRequest object
            SendMessagesRequest request = SendMessagesRequest.builder()
                .applicationId(appId)
                .messageRequest(msgReq)
                .build();

            SendMessagesResponse response= pinpoint.sendMessages(request);
            MessageResponse msg1 = response.messageResponse();
            Map map1 = msg1.result();

            //Write out the result of sendMessage.
            map1.forEach((k, v) -> System.out.println((k + ":" + v)));

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

For the full SDK example, see [SendMessage\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/SendMessage.java/) on [GitHub](https://github.com/)\.

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send an SMS message by using the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)\. This example assumes that you've already installed and configured the SDK for JavaScript in Node\.js\. For more information, see [Getting started](https://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-intro.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Setting credentials](https://docs.aws.amazon.com/sdk-for-javascript/latest/developer-guide/setting-credentials.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

```
'use strict';

var AWS = require('aws-sdk');

// The AWS Region that you want to use to send the message. For a list of
// AWS Regions where the Amazon Pinpoint API is available, see
// https://docs.aws.amazon.com/pinpoint/latest/apireference/.
var aws_region = "us-east-1";

// The phone number or short code to send the message from. The phone number
// or short code that you specify has to be associated with your Amazon Pinpoint
// account. For best results, specify long codes in E.164 format.
var originationNumber = "+12065550199";

// The recipient's phone number.  For best results, you should specify the
// phone number in E.164 format.
var destinationNumber = "+14255550142";

// The content of the SMS message.
var message = "This message was sent through Amazon Pinpoint "
            + "using the AWS SDK for JavaScript in Node.js. Reply STOP to "
            + "opt out.";

// The Amazon Pinpoint project/application ID to use when you send this message.
// Make sure that the SMS channel is enabled for the project or application
// that you choose.
var applicationId = "ce796be37f32f178af652b26eexample";

// The type of SMS message that you want to send. If you plan to send
// time-sensitive content, specify TRANSACTIONAL. If you plan to send
// marketing-related content, specify PROMOTIONAL.
var messageType = "TRANSACTIONAL";

// The registered keyword associated with the originating short code.
var registeredKeyword = "myKeyword";

// The sender ID to use when sending the message. Support for sender ID
// varies by country or region. For more information, see
// https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-countries.html
var senderId = "MySenderID";

// Specify that you're using a shared credentials file, and optionally specify
// the profile that you want to use.
var credentials = new AWS.SharedIniFileCredentials({profile: 'default'});
AWS.config.credentials = credentials;

// Specify the region.
AWS.config.update({region:aws_region});

//Create a new Pinpoint object.
var pinpoint = new AWS.Pinpoint();

// Specify the parameters to pass to the API.
var params = {
  ApplicationId: applicationId,
  MessageRequest: {
    Addresses: {
      [destinationNumber]: {
        ChannelType: 'SMS'
      }
    },
    MessageConfiguration: {
      SMSMessage: {
        Body: message,
        Keyword: registeredKeyword,
        MessageType: messageType,
        OriginationNumber: originationNumber,
        SenderId: senderId,
      }
    }
  }
};

//Try to send the message.
pinpoint.sendMessages(params, function(err, data) {
  // If something goes wrong, print an error message.
  if(err) {
    console.log(err.message);
  // Otherwise, show the unique ID for the message.
  } else {
    console.log("Message sent! " 
        + data['MessageResponse']['Result'][destinationNumber]['StatusMessage']);
  }
});
```

------
#### [ Python ]

Use this example to send an SMS message by using the [AWS SDK for Python \(Boto3\)](https://aws.amazon.com/sdk-for-python)\. This example assumes that you've already installed and configured the SDK for Python\. For more information, see [Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) in * AWS SDK for Python \(Boto3\) Getting Started*\.

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


def send_sms_message(
        pinpoint_client, app_id, origination_number, destination_number, message,
        message_type):
    """
    Sends an SMS message with Amazon Pinpoint.

    :param pinpoint_client: A Boto3 Pinpoint client.
    :param app_id: The Amazon Pinpoint project/application ID to use when you send
                   this message. The SMS channel must be enabled for the project or
                   application.
    :param destination_number: The recipient's phone number in E.164 format.
    :param origination_number: The phone number to send the message from. This phone
                               number must be associated with your Amazon Pinpoint
                               account and be in E.164 format.
    :param message: The content of the SMS message.
    :param message_type: The type of SMS message that you want to send. If you send
                         time-sensitive content, specify TRANSACTIONAL. If you send
                         marketing-related content, specify PROMOTIONAL.
    :return: The ID of the message.
    """
    try:
        response = pinpoint_client.send_messages(
            ApplicationId=app_id,
            MessageRequest={
                'Addresses': {destination_number: {'ChannelType': 'SMS'}},
                'MessageConfiguration': {
                    'SMSMessage': {
                        'Body': message,
                        'MessageType': message_type,
                        'OriginationNumber': origination_number}}})
    except ClientError:
        logger.exception("Couldn't send message.")
        raise
    else:
        return response['MessageResponse']['Result'][destination_number]['MessageId']


def main():
    app_id = "ce796be37f32f178af652b26eexample"
    origination_number = "+12065550199"
    destination_number = "+14255550142"
    message = (
        "This is a sample message sent from Amazon Pinpoint by using the AWS SDK for "
        "Python (Boto 3).")
    message_type = "TRANSACTIONAL"

    print("Sending SMS message.")
    message_id = send_sms_message(
        boto3.client('pinpoint'), app_id, origination_number, destination_number,
        message, message_type)
    print(f"Message sent! Message ID: {message_id}.")


if __name__ == '__main__':
    main()
```

You can also use message templates to send SMS messages, as shown in the following example:

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

These examples assume that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Credentials](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html) in the *AWS SDK for Python \(Boto3\) API Reference*\.

------
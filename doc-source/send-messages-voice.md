# Send voice messages<a name="send-messages-voice"></a>

You can use the Amazon Pinpoint API to send voice messages to specific phone numbers\. This section contains complete code examples that you can use to send voice messages through the Amazon Pinpoint SMS and Voice API by using an AWS SDK\.

------
#### [ Java ]

Use this example to send a voice message by using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This example assumes that you've already installed and configured the SDK for Java\. For more information, see [Getting started ](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/getting-started.html) in the AWS SDK for Java Developer Guide\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing user\. For more information, see [Set up AWS credentials and Region for development](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\.

```
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.core.client.config.ClientOverrideConfiguration;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpointsmsvoice.PinpointSmsVoiceClient;
import software.amazon.awssdk.services.pinpointsmsvoice.model.SSMLMessageType;
import software.amazon.awssdk.services.pinpointsmsvoice.model.VoiceMessageContent;
import software.amazon.awssdk.services.pinpointsmsvoice.model.SendVoiceMessageRequest;
import software.amazon.awssdk.services.pinpointsmsvoice.model.PinpointSmsVoiceException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
```

```
    public static void sendVoiceMsg(PinpointSmsVoiceClient client, String originationNumber, String destinationNumber ) {

        try {
            SSMLMessageType ssmlMessageType = SSMLMessageType.builder()
                .languageCode(languageCode)
                .text(ssmlMessage)
                .voiceId(voiceName)
                .build();

            VoiceMessageContent content = VoiceMessageContent.builder()
                .ssmlMessage(ssmlMessageType)
                .build();

            SendVoiceMessageRequest voiceMessageRequest = SendVoiceMessageRequest.builder()
                .destinationPhoneNumber(destinationNumber)
                .originationPhoneNumber(originationNumber)
                .content(content)
                .build();

            client.sendVoiceMessage(voiceMessageRequest);
            System.out.println("The message was sent successfully.");

        } catch (PinpointSmsVoiceException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

For the full SDK example, see [SendVoiceMessage\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/SendVoiceMessage.java/) on [GitHub](https://github.com/)\.

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send a voice message by using the AWS SDK for JavaScript in Node\.js\. This example assumes that you've already installed and configured the SDK for JavaScript in Node\.js\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing user\. For more information, see [Setting credentials](https://docs.aws.amazon.com/sdk-for-javascript/latest/developer-guide/setting-credentials.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

```
'use strict'

var AWS = require('aws-sdk');

// The AWS Region that you want to use to send the voice message. For a list of
// AWS Regions where the Amazon Pinpoint SMS and Voice API is available, see
// https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/
var aws_region = "us-east-1";

// The phone number that the message is sent from. The phone number that you
// specify has to be associated with your Amazon Pinpoint account. For best results, you
// should specify the phone number in E.164 format.
var originationNumber = "+12065550110";

// The recipient's phone number. For best results, you should specify the phone
// number in E.164 format.
var destinationNumber = "+12065550142";

// The language to use when sending the message. For a list of supported
// languages, see https://docs.aws.amazon.com/polly/latest/dg/SupportedLanguage.html
var languageCode = "en-US";

// The Amazon Polly voice that you want to use to send the message. For a list
// of voices, see https://docs.aws.amazon.com/polly/latest/dg/voicelist.html
var voiceId = "Matthew";

// The content of the message. This example uses SSML to customize and control
// certain aspects of the message, such as the volume or the speech rate.
// The message can't contain any line breaks.
var ssmlMessage = "<speak>"
    + "This is a test message sent from <emphasis>Amazon Pinpoint</emphasis> "
    + "using the <break strength='weak'/>AWS SDK for JavaScript in Node.js. "
    + "<amazon:effect phonation='soft'>Thank you for listening."
    + "</amazon:effect>"
    + "</speak>";

// The phone number that you want to appear on the recipient's device. The phone
// number that you specify has to be associated with your Amazon Pinpoint account.
var callerId = "+12065550199";

// The configuration set that you want to use to send the message.
var configurationSet = "ConfigSet";

// Specify that you're using a shared credentials file, and optionally specify
// the profile that you want to use.
var credentials = new AWS.SharedIniFileCredentials({profile: 'default'});
AWS.config.credentials = credentials;

// Specify the region.
AWS.config.update({region:aws_region});

//Create a new Pinpoint object.
var pinpointsmsvoice = new AWS.PinpointSMSVoice();

var params = {
  CallerId: callerId,
  ConfigurationSetName: configurationSet,
  Content: {
    SSMLMessage: {
      LanguageCode: languageCode,
      Text: ssmlMessage,
      VoiceId: voiceId
    }
  },
  DestinationPhoneNumber: destinationNumber,
  OriginationPhoneNumber: originationNumber
};

//Try to send the message.
pinpointsmsvoice.sendVoiceMessage(params, function(err, data) {
  // If something goes wrong, print an error message.
  if(err) {
    console.log(err.message);
  // Otherwise, show the unique ID for the message.
  } else {
    console.log("Message sent! Message ID: " + data['MessageId']);
  }
});
```

------
#### [ Python ]

Use this example to send a voice message by using the AWS SDK for Python \(Boto3\)\. This example assumes that you've already installed and configured the SDK for Python \(Boto3\)\. 

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing user\. For more information, see [Credentials](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html) in the *AWS SDK for Python \(Boto3\) API Reference*\.

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


def send_voice_message(
        sms_voice_client, origination_number, caller_id, destination_number,
        language_code, voice_id, ssml_message):
    """
    Sends a voice message using speech synthesis provided by Amazon Polly.

    :param sms_voice_client: A Boto3 PinpointSMSVoice client.
    :param origination_number: The phone number that the message is sent from.
                               The phone number must be associated with your Amazon
                               Pinpoint account and be in E.164 format.
    :param caller_id: The phone number that you want to appear on the recipient's
                      device. The phone number must be associated with your Amazon
                      Pinpoint account and be in E.164 format.
    :param destination_number: The recipient's phone number. Specify the phone
                               number in E.164 format.
    :param language_code: The language to use when sending the message.
    :param voice_id: The Amazon Polly voice that you want to use to send the message.
    :param ssml_message: The content of the message. This example uses SSML to control
                         certain aspects of the message, such as the volume and the
                         speech rate. The message must not contain line breaks.
    :return: The ID of the message.
    """
    try:
        response = sms_voice_client.send_voice_message(
            DestinationPhoneNumber=destination_number,
            OriginationPhoneNumber=origination_number,
            CallerId=caller_id,
            Content={
                'SSMLMessage': {
                    'LanguageCode': language_code,
                    'VoiceId': voice_id,
                    'Text': ssml_message}})
    except ClientError:
        logger.exception(
            "Couldn't send message from %s to %s.", origination_number, destination_number)
        raise
    else:
        return response['MessageId']


def main():
    origination_number = "+12065550110"
    caller_id = "+12065550199"
    destination_number = "+12065550142"
    language_code = "en-US"
    voice_id = "Matthew"
    ssml_message = (
        "<speak>"
        "This is a test message sent from <emphasis>Amazon Pinpoint</emphasis> "
        "using the <break strength='weak'/>AWS SDK for Python (Boto3). "
        "<amazon:effect phonation='soft'>Thank you for listening."
        "</amazon:effect>"
        "</speak>")
    print(f"Sending voice message from {origination_number} to {destination_number}.")
    message_id = send_voice_message(
        boto3.client('pinpoint-sms-voice'), origination_number, caller_id,
        destination_number, language_code, voice_id, ssml_message)
    print(f"Message sent!\nMessage ID: {message_id}")


if __name__ == '__main__':
    main()
```

------
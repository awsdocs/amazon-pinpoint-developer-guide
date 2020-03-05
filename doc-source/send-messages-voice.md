# Send Voice Messages<a name="send-messages-voice"></a>

You can use the Amazon Pinpoint API to send voice messages to specific phone numbers\. This section contains complete code examples that you can use to send voice messages through the Amazon Pinpoint SMS and Voice API by using an AWS SDK\.

------
#### [ Java ]

Use this example to send a voice message by using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This example assumes that you've already installed and configured the SDK for Java\. For more information, see [Getting Started ](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/getting-started.html) in the AWS SDK for Java Developer Guide\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\.

```
package com.amazonaws.samples;

import java.io.IOException;

import com.amazonaws.services.pinpointsmsvoice.AmazonPinpointSMSVoice;
import com.amazonaws.services.pinpointsmsvoice.AmazonPinpointSMSVoiceClientBuilder;
import com.amazonaws.services.pinpointsmsvoice.model.SSMLMessageType;
import com.amazonaws.services.pinpointsmsvoice.model.SendVoiceMessageRequest;
import com.amazonaws.services.pinpointsmsvoice.model.VoiceMessageContent;

public class SendMessage {
    
    // The AWS Region that you want to use to send the voice message. For a list of
    // AWS Regions where the Amazon Pinpoint SMS and Voice API is available, see
    // https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/
    static final String region = "us-east-1";
	
    // The phone number that the message is sent from. The phone number that you
    // specify has to be associated with your Amazon Pinpoint account. For best 
    // results, you should specify the phone number in E.164 format.
    static final String originationNumber = "+12065550110";
    
    // The recipient's phone number.  For best results, you should specify the
    // phone number in E.164 format.
    static final String destinationNumber = "+12065550142";
    
    // The Amazon Polly voice that you want to use to send the message. For a list
    // of voices, see https://docs.aws.amazon.com/polly/latest/dg/voicelist.html
    static final String voiceName = "Matthew";
    
    // The language to use when sending the message. For a list of supported
    // languages, see https://docs.aws.amazon.com/polly/latest/dg/SupportedLanguage.html
    static final String languageCode = "en-US";
    
    // The content of the message. This example uses SSML to customize and control
    // certain aspects of the message, such as by adding pauses and changing 
    // phonation. The message can't contain any line breaks.
    static final String ssmlMessage = "<speak>This is a test message sent from "
                                    + "<emphasis>Amazon Pinpoint</emphasis> "
                                    + "using the <break strength='weak'/>AWS "
                                    + "SDK for Java. "
                                    + "<amazon:effect phonation='soft'>Thank "
                                    + "you for listening.</amazon:effect></speak>";
    
    // The phone number that you want to appear on the recipient's device. The 
    // phone number that you specify has to be associated with your Amazon Pinpoint
    // account.
    static final String callerId = "+12065550199";
    
    // The configuration set that you want to use to send the message.
    static final String configurationSet = "ConfigSet";
    
    public static void main(String[] args) throws IOException {
        try {
            AmazonPinpointSMSVoice client = AmazonPinpointSMSVoiceClientBuilder.standard()
            	.withRegion(region).build();
            SendVoiceMessageRequest request = new SendVoiceMessageRequest()
            	.withCallerId(callerId)
            	.withDestinationPhoneNumber(destinationNumber)
            	.withOriginationPhoneNumber(originationNumber)
            	.withConfigurationSetName(configurationSet)
            	.withContent(new VoiceMessageContent()
            		.withSSMLMessage(new SSMLMessageType()
            			.withLanguageCode(languageCode)
            			.withVoiceId(voiceName)
            			.withText(ssmlMessage)
            		)
            	); 
            client.sendVoiceMessage(request);
            System.out.println("The message was sent successfully.");
        } catch (Exception ex) {
            System.out.println("The message wasn't sent. Error message: " + ex.getMessage());
        }
    }
}
```

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send a voice message by using the AWS SDK for JavaScript in Node\.js\. This example assumes that you've already installed and configured the SDK for JavaScript in Node\.js\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Setting Credentials](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-credentials.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

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

Use this example to send a voice message by using the AWS SDK for Python \(Boto 3\)\. This example assumes that you've already installed and configured the SDK for Python \(Boto 3\)\. 

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Credentials](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html) in the *AWS SDK for Python \(Boto 3\) API Reference*\.

```
import boto3
from botocore.exceptions import ClientError

# The AWS Region that you want to use to send the voice message. For a list of
# AWS Regions where the Amazon Pinpoint SMS and Voice API is available, see
# https://docs.aws.amazon.com/pinpoint-sms-voice/latest/APIReference/
region = "us-east-1"

# The phone number that the message is sent from. The phone number that you
# specify has to be associated with your Amazon Pinpoint account. For best results, you
# should specify the phone number in E.164 format.
originationNumber = "+12065550110"

# The recipient's phone number. For best results, you should specify the phone
# number in E.164 format.
destinationNumber = "+12065550142"

# The language to use when sending the message. For a list of supported
# languages, see https://docs.aws.amazon.com/polly/latest/dg/SupportedLanguage.html
languageCode = "en-US"

# The Amazon Polly voice that you want to use to send the message. For a list
# of voices, see https://docs.aws.amazon.com/polly/latest/dg/voicelist.html
voiceId = "Matthew"

# The content of the message. This example uses SSML to customize and control
# certain aspects of the message, such as the volume or the speech rate.
# The message can't contain any line breaks.
ssmlMessage = ("<speak>"
               "This is a test message sent from <emphasis>Amazon Pinpoint</emphasis> "
               "using the <break strength='weak'/>AWS SDK for Python. "
               "<amazon:effect phonation='soft'>Thank you for listening."
               "</amazon:effect>"
               "</speak>")

# The phone number that you want to appear on the recipient's device. The phone
# number that you specify has to be associated with your Amazon Pinpoint account.
callerId = "+12065550199"

# The configuration set that you want to use to send the message.
configurationSet = "ConfigSet"

# Create a new SMS and Voice client and specify an AWS Region.
client = boto3.client('sms-voice',region_name=region)

try:
    response = client.send_voice_message(
        DestinationPhoneNumber = destinationNumber,
        OriginationPhoneNumber = originationNumber,
        CallerId = callerId,
        ConfigurationSetName = configurationSet,
        Content={
            'SSMLMessage':{
                'LanguageCode': languageCode,
                'VoiceId': voiceId,
                'Text': ssmlMessage
            }
        }
    )
# Display an error message if something goes wrong.
except ClientError as e:
    print(e.response['Error']['Message'])
# If the message is sent successfully, show the message ID.
else:
    print("Message sent!"),
    print("Message ID: " + response['MessageId'])
```

------
# Send email by using the Amazon Pinpoint API<a name="send-messages-sdk"></a>

This section contains complete code examples that you can use to send email through the Amazon Pinpoint API by using an AWS SDK\.

------
#### [ C\# ]

Use this example to send email by using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. This example assumes that you've already installed and configured the AWS SDK for \.NET\. For more information, see [Getting started with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Configuring AWS credentials](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

This code example was tested using the AWS SDK for \.NET version 3\.3\.29\.13 and \.NET Core runtime version 2\.1\.2\.

```
using System;
using System.Collections.Generic;
using Amazon;
using Amazon.Pinpoint;
using Amazon.Pinpoint.Model;

namespace PinpointEmailSendMessageAPI
{
    class MainClass
    {
        // The AWS Region that you want to use to send the email. For a list of
        // AWS Regions where the Amazon Pinpoint API is available, see 
        // https://docs.aws.amazon.com/pinpoint/latest/apireference/
        static string region = "us-west-2";

        // The "From" address. This address has to be verified in Amazon Pinpoint 
        // in the region you're using to send email.
        static string senderAddress = "sender@example.com";

        // The address on the "To" line. If your Amazon Pinpoint account is in
        // the sandbox, this address also has to be verified. 
        static string toAddress = "recipient@example.com";

        // The Amazon Pinpoint project/application ID to use when you send this message.
        // Make sure that the SMS channel is enabled for the project or application
        // that you choose.
        static string appId = "ce796be37f32f178af652b26eexample";

        // The subject line of the email.
        static string subject = "Amazon Pinpoint Email test";

        // The body of the email for recipients whose email clients don't 
        // support HTML content.
        static string textBody = @"Amazon Pinpoint Email Test (.NET)
---------------------------------
This email was sent using the Amazon Pinpoint API using the AWS SDK for .NET.";

        // The body of the email for recipients whose email clients support
        // HTML content.
        static string htmlBody = @"<html>
<head></head>
<body>
  <h1>Amazon Pinpoint Email Test (AWS SDK for .NET)</h1>
  <p>This email was sent using the 
    <a href='https://aws.amazon.com/pinpoint/'>Amazon Pinpoint</a> API 
    using the <a href='https://aws.amazon.com/sdk-for-net/'>
      AWS SDK for .NET</a>.</p>
</body>
</html>";

        // The character encoding the you want to use for the subject line and
        // message body of the email.
        static string charset = "UTF-8";

        public static void Main(string[] args)
        {
            using (var client = new AmazonPinpointClient(RegionEndpoint.GetBySystemName(region)))
            {
                var sendRequest = new SendMessagesRequest
                {
                    ApplicationId = appId,
                    MessageRequest = new MessageRequest
                    {
                        Addresses = new Dictionary<string, AddressConfiguration>
                        {
                            {
                                toAddress,
                                new AddressConfiguration
                                {
                                    ChannelType = "EMAIL"
                                }
                            }
                        },
                        MessageConfiguration = new DirectMessageConfiguration
                        {
                            EmailMessage = new EmailMessage
                            {
                                FromAddress = senderAddress,
                                SimpleEmail = new SimpleEmail
                                {
                                    HtmlPart = new SimpleEmailPart
                                    {
                                        Charset = charset,
                                        Data = htmlBody
                                    },
                                    TextPart = new SimpleEmailPart
                                    {
                                        Charset = charset,
                                        Data = textBody
                                    },
                                    Subject = new SimpleEmailPart
                                    {
                                        Charset = charset,
                                        Data = subject
                                    }
                                }
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

Use this example to send email by using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This example assumes that you've already installed and configured the AWS SDK for Java 2\.x\. For more information, see [Getting started](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/getting-started.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Set default credentials and Region](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html#setup-credentials) in the *AWS SDK for Java Developer Guide*\.

This code example was tested using the AWS SDK for Java version 2\.3\.1 and OpenJDK version 11\.0\.1\.

```
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpoint.PinpointClient;
import software.amazon.awssdk.services.pinpoint.model.AddressConfiguration;
import software.amazon.awssdk.services.pinpoint.model.ChannelType;
import software.amazon.awssdk.services.pinpoint.model.SimpleEmailPart;
import software.amazon.awssdk.services.pinpoint.model.SimpleEmail;
import software.amazon.awssdk.services.pinpoint.model.EmailMessage;
import software.amazon.awssdk.services.pinpoint.model.DirectMessageConfiguration;
import software.amazon.awssdk.services.pinpoint.model.MessageRequest;
import software.amazon.awssdk.services.pinpoint.model.SendMessagesRequest;
import software.amazon.awssdk.services.pinpoint.model.PinpointException;
import java.util.HashMap;
import java.util.Map;
```

```
    public static void sendEmail(PinpointClient pinpoint,
                                 String subject,
                                 String appId,
                                 String senderAddress,
                                 String toAddress) {

        try {

            Map<String,AddressConfiguration> addressMap = new HashMap<String,AddressConfiguration>();
            AddressConfiguration configuration =  AddressConfiguration.builder()
                    .channelType(ChannelType.EMAIL)
                    .build();

            addressMap.put(toAddress, configuration);
            SimpleEmailPart emailPart = SimpleEmailPart.builder()
                    .data(htmlBody)
                    .charset(charset)
                    .build() ;

            SimpleEmailPart subjectPart = SimpleEmailPart.builder()
                    .data(subject)
                    .charset(charset)
                    .build() ;

            SimpleEmail simpleEmail = SimpleEmail.builder()
                    .htmlPart(emailPart)
                    .subject(subjectPart)
                    .build();

            EmailMessage emailMessage =  EmailMessage.builder()
                    .body(htmlBody)
                    .fromAddress(senderAddress)
                    .simpleEmail(simpleEmail)
                    .build();

            DirectMessageConfiguration directMessageConfiguration = DirectMessageConfiguration.builder()
                    .emailMessage(emailMessage)
                    .build();

            MessageRequest messageRequest = MessageRequest.builder()
                    .addresses(addressMap)
                    .messageConfiguration(directMessageConfiguration)
                    .build();

            SendMessagesRequest messagesRequest = SendMessagesRequest.builder()
                    .applicationId(appId)
                    .messageRequest(messageRequest)
                    .build();

            pinpoint.sendMessages(messagesRequest);


        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

For the full SDK example, see [SendEmailMessage\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/SendEmailMessage.java/) on [GitHub](https://github.com/)\.

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send email by using the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)\. This example assumes that you've already installed and configured the SDK for JavaScript in Node\.js\. For more information, see [Getting started](https://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-intro.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Setting credentials](https://docs.aws.amazon.com/sdk-for-javascript/latest/developer-guide/setting-credentials.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

This code example was tested using the SDK for JavaScript in Node\.js version 2\.388\.0 and Node\.js version 11\.7\.0\.

```
'use strict';

const AWS = require('aws-sdk');

// The AWS Region that you want to use to send the email. For a list of
// AWS Regions where the Amazon Pinpoint API is available, see
// https://docs.aws.amazon.com/pinpoint/latest/apireference/
const aws_region = "us-west-2"

// The "From" address. This address has to be verified in Amazon Pinpoint
// in the region that you use to send email.
const senderAddress = "sender@example.com";

// The address on the "To" line. If your Amazon Pinpoint account is in
// the sandbox, this address also has to be verified.
var toAddress = "recipient@example.com";

// The Amazon Pinpoint project/application ID to use when you send this message.
// Make sure that the SMS channel is enabled for the project or application
// that you choose.
const appId = "ce796be37f32f178af652b26eexample";

// The subject line of the email.
var subject = "Amazon Pinpoint (AWS SDK for JavaScript in Node.js)";

// The email body for recipients with non-HTML email clients.
var body_text = `Amazon Pinpoint Test (SDK for JavaScript in Node.js)
----------------------------------------------------
This email was sent with Amazon Pinpoint using the AWS SDK for JavaScript in Node.js.
For more information, see https:\/\/aws.amazon.com/sdk-for-node-js/`;

// The body of the email for recipients whose email clients support HTML content.
var body_html = `<html>
<head></head>
<body>
  <h1>Amazon Pinpoint Test (SDK for JavaScript in Node.js)</h1>
  <p>This email was sent with
    <a href='https://aws.amazon.com/pinpoint/'>the Amazon Pinpoint API</a> using the
    <a href='https://aws.amazon.com/sdk-for-node-js/'>
      AWS SDK for JavaScript in Node.js</a>.</p>
</body>
</html>`;

// The character encoding the you want to use for the subject line and
// message body of the email.
var charset = "UTF-8";

// Specify that you're using a shared credentials file.
var credentials = new AWS.SharedIniFileCredentials({profile: 'default'});
AWS.config.credentials = credentials;

// Specify the region.
AWS.config.update({region:aws_region});

//Create a new Pinpoint object.
var pinpoint = new AWS.Pinpoint();

// Specify the parameters to pass to the API.
var params = {
  ApplicationId: appId,
  MessageRequest: {
    Addresses: {
      [toAddress]:{
        ChannelType: 'EMAIL'
      }
    },
    MessageConfiguration: {
      EmailMessage: {
        FromAddress: senderAddress,
        SimpleEmail: {
          Subject: {
            Charset: charset,
            Data: subject
          },
          HtmlPart: {
            Charset: charset,
            Data: body_html
          },
          TextPart: {
            Charset: charset,
            Data: body_text
          }
        }
      }
    }
  }
};

//Try to send the email.
pinpoint.sendMessages(params, function(err, data) {
  // If something goes wrong, print an error message.
  if(err) {
    console.log(err.message);
  } else {
    console.log("Email sent! Message ID: ", data['MessageResponse']['Result'][toAddress]['MessageId']);
  }
});
```

------
#### [ Python ]

Use this example to send email by using the [AWS SDK for Python \(Boto3\)](https://aws.amazon.com/sdk-for-python/)\. This example assumes that you've already installed and configured the SDK for Python \(Boto3\)\. For more information, see [Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) in the *AWS SDK for Python \(Boto3\) API Reference*\.

```
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


def send_email_message(
        pinpoint_client, app_id, sender, to_addresses, char_set, subject,
        html_message, text_message):
    """
    Sends an email message with HTML and plain text versions.
    
    :param pinpoint_client: A Boto3 Pinpoint client.
    :param app_id: The Amazon Pinpoint project ID to use when you send this message.
    :param sender: The "From" address. This address must be verified in
                   Amazon Pinpoint in the AWS Region you're using to send email.
    :param to_addresses: The addresses on the "To" line. If your Amazon Pinpoint account
                         is in the sandbox, these addresses must be verified.
    :param char_set: The character encoding to use for the subject line and message
                     body of the email.
    :param subject: The subject line of the email.
    :param html_message: The body of the email for recipients whose email clients can
                         display HTML content.
    :param text_message: The body of the email for recipients whose email clients
                         don't support HTML content.
    :return: A dict of to_addresses and their message IDs.
    """
    try:
        response = pinpoint_client.send_messages(
            ApplicationId=app_id,
            MessageRequest={
                'Addresses': {
                    to_address: {'ChannelType': 'EMAIL'} for to_address in to_addresses
                },
                'MessageConfiguration': {
                    'EmailMessage': {
                        'FromAddress': sender,
                        'SimpleEmail': {
                            'Subject': {'Charset': char_set, 'Data': subject},
                            'HtmlPart': {'Charset': char_set, 'Data': html_message},
                            'TextPart': {'Charset': char_set, 'Data': text_message}}}}})
    except ClientError:
        logger.exception("Couldn't send email.")
        raise
    else:
        return {
            to_address: message['MessageId'] for
            to_address, message in response['MessageResponse']['Result'].items()
        }


def main():
    app_id = "ce796be37f32f178af652b26eexample"
    sender = "sender@example.com"
    to_address = "recipient@example.com"
    char_set = "UTF-8"
    subject = "Amazon Pinpoint Test (SDK for Python (Boto3))"
    text_message = """Amazon Pinpoint Test (SDK for Python)
    -------------------------------------
    This email was sent with Amazon Pinpoint using the AWS SDK for Python (Boto3).
    For more information, see https://aws.amazon.com/sdk-for-python/
                """
    html_message = """<html>
    <head></head>
    <body>
      <h1>Amazon Pinpoint Test (SDK for Python (Boto3)</h1>
      <p>This email was sent with
        <a href='https://aws.amazon.com/pinpoint/'>Amazon Pinpoint</a> using the
        <a href='https://aws.amazon.com/sdk-for-python/'>
          AWS SDK for Python (Boto3)</a>.</p>
    </body>
    </html>
                """

    print("Sending email.")
    message_ids = send_email_message(
        boto3.client('pinpoint'), app_id, sender, [to_address], char_set, subject,
        html_message, text_message)
    print(f"Message sent! Message IDs: {message_ids}")


if __name__ == '__main__':
    main()
```

You can also use message templates to send email messages, as shown in the following example:

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

These examples assume that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Credentials](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html) in the *AWS SDK for Python \(Boto3\) API Reference*\.

------
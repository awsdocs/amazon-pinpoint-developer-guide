# Send Email by Using the Amazon Pinpoint API<a name="send-messages-sdk"></a>

This section contains complete code examples that you can use to send email through the Amazon Pinpoint API by using an AWS SDK\.

------
#### [ C\# ]

Use this example to send email by using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. This example assumes that you've already installed and configured the AWS SDK for \.NET\. For more information, see [Getting Started with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

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

Use this example to send email by using the [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. This example assumes that you've already installed and configured the AWS SDK for Java 2\.x\. For more information, see [Getting Started](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/getting-started.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\.

This code example was tested using the AWS SDK for Java version 2\.3\.1 and OpenJDK version 11\.0\.1\.

```
package com.amazonaws.samples;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.AddressConfiguration;
import com.amazonaws.services.pinpoint.model.ChannelType;
import com.amazonaws.services.pinpoint.model.DirectMessageConfiguration;
import com.amazonaws.services.pinpoint.model.EmailMessage;
import com.amazonaws.services.pinpoint.model.MessageRequest;
import com.amazonaws.services.pinpoint.model.SendMessagesRequest;
import com.amazonaws.services.pinpoint.model.SimpleEmail;
import com.amazonaws.services.pinpoint.model.SimpleEmailPart;

public class SendMessages {

    // The AWS Region that you want to use to send the message. For a list of
    // AWS Regions where the Amazon Pinpoint API is available, see
    // https://docs.aws.amazon.com/pinpoint/latest/apireference/
    public static String region = "us-west-2";
    
    // The "From" address. This address has to be verified in Amazon 
    // Pinpoint in the region you're using to send email.
    public static String senderAddress = "sender@example.com";

    // The address on the "To" line. If your Amazon Pinpoint account is in
    // the sandbox, this address also has to be verified. 
    public static String toAddress = "recipient@example.com";

    // The Amazon Pinpoint project/application ID to use when you send this message.
    // Make sure that the SMS channel is enabled for the project or application
    // that you choose.
    public static String appId = "ce796be37f32f178af652b26eexample";

    // The subject line of the email.
    public static String subject = "Amazon Pinpoint test";

    // The email body for recipients with non-HTML email clients.
    static final String textBody = "Amazon Pinpoint Test (SDK for Java 2.x)\r\n"
            + "---------------------------------\r\n"
            + "This email was sent using the Amazon Pinpoint "
            + "API using the AWS SDK for Java 2.x.";
    
    // The body of the email for recipients whose email clients support
    // HTML content.
    static final String htmlBody = "<h1>Amazon Pinpoint test (AWS SDK for Java 2.x)</h1>"
            + "<p>This email was sent through the <a href='https://aws.amazon.com/pinpoint/'>"
            + "Amazon Pinpoint</a> Email API using the "
            + "<a href='https://aws.amazon.com/sdk-for-java/'>AWS SDK for Java 2.x</a>";

    // The character encoding the you want to use for the subject line and
    // message body of the email.
    public static String charset = "UTF-8";
     
    public static void main(String[] args) throws IOException {
          
        try {               
            Map<String,AddressConfiguration> addressMap = 
                new HashMap<String,AddressConfiguration>();
               
            addressMap.put(toAddress, new AddressConfiguration()
                .withChannelType(ChannelType.EMAIL));
               
            AmazonPinpoint client = AmazonPinpointClientBuilder.standard()
                .withRegion(region).build();
               
            SendMessagesRequest request = (new SendMessagesRequest()
                .withApplicationId(appId)
                .withMessageRequest(new MessageRequest()
                    .withAddresses(addressMap)           
                    .withMessageConfiguration(new DirectMessageConfiguration()
                        .withEmailMessage(new EmailMessage()
                            .withSimpleEmail(new SimpleEmail()
                                .withHtmlPart(new SimpleEmailPart()
                                    .withCharset(charset)
                                    .withData(htmlBody)
                                )
                                .withTextPart(new SimpleEmailPart()
                                    .withCharset(charset)
                                    .withData(textBody)
                                )
                                .withSubject(new SimpleEmailPart()
                                    .withCharset(charset)
                                    .withData(subject)
                                )
                            )
                        )
                    )
                )
            );
            System.out.println("Sending message...");               
            client.sendMessages(request);
            System.out.println("Message sent!");
    } catch (Exception ex) {
        System.out.println("The message wasn't sent. Error message: " 
                + ex.getMessage());
        }
    }
}
```

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send email by using the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)\. This example assumes that you've already installed and configured the SDK for JavaScript in Node\.js\. For more information, see [Getting Started](https://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-intro.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Setting Credentials](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-credentials.html) in the *AWS SDK for JavaScript in Node\.js Developer Guide*\.

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

Use this example to send email by using the [AWS SDK for Python \(Boto 3\)](https://aws.amazon.com/sdk-for-python/)\. This example assumes that you've already installed and configured the SDK for Python \(Boto 3\)\. For more information, see [Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) in the *AWS SDK for Python \(Boto 3\) API Reference*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Credentials](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html) in the *AWS SDK for Python \(Boto 3\) API Reference*\.

This code example was tested using the SDK for Python \(Boto 3\) version 1\.9\.62 and Python version 3\.6\.7\.

```
import boto3
from botocore.exceptions import ClientError

# The AWS Region that you want to use to send the email. For a list of
# AWS Regions where the Amazon Pinpoint API is available, see
# https://docs.aws.amazon.com/pinpoint/latest/apireference/
AWS_REGION = "us-west-2"

# The "From" address. This address has to be verified in
# Amazon Pinpoint in the region you're using to send email.
SENDER = "Mary Major <sender@example.com>"

# The addresses on the "To" line. If your Amazon Pinpoint account is in
# the sandbox, these addresses also have to be verified.
TOADDRESS = "recipient@example.com"

# The Amazon Pinpoint project/application ID to use when you send this message.
# Make sure that the email channel is enabled for the project or application
# that you choose.
APPID = "ce796be37f32f178af652b26eexample"

# The subject line of the email.
SUBJECT = "Amazon Pinpoint Test (SDK for Python (Boto 3))"

# The body of the email for recipients whose email clients don't support HTML
# content.
BODY_TEXT = """Amazon Pinpoint Test (SDK for Python)
-------------------------------------
This email was sent with Amazon Pinpoint using the AWS SDK for Python (Boto 3).
For more information, see https:#aws.amazon.com/sdk-for-python/
            """

# The body of the email for recipients whose email clients can display HTML
# content.
BODY_HTML = """<html>
<head></head>
<body>
  <h1>Amazon Pinpoint Test (SDK for Python)</h1>
  <p>This email was sent with
    <a href='https:#aws.amazon.com/pinpoint/'>Amazon Pinpoint</a> using the
    <a href='https:#aws.amazon.com/sdk-for-python/'>
      AWS SDK for Python (Boto 3)</a>.</p>
</body>
</html>
            """

# The character encoding that you want to use for the subject line and message
# body of the email.
CHARSET = "UTF-8"

# Create a new client and specify a region.
client = boto3.client('pinpoint',region_name=AWS_REGION)
try:
    response = client.send_messages(
        ApplicationId=APPID,
        MessageRequest={
            'Addresses': {
                TOADDRESS: {
                     'ChannelType': 'EMAIL'
                }
            },
            'MessageConfiguration': {
                'EmailMessage': {
                    'FromAddress': SENDER,
                    'SimpleEmail': {
                        'Subject': {
                            'Charset': CHARSET,
                            'Data': SUBJECT
                        },
                        'HtmlPart': {
                            'Charset': CHARSET,
                            'Data': BODY_HTML
                        },
                        'TextPart': {
                            'Charset': CHARSET,
                            'Data': BODY_TEXT
                        }
                    }
                }
            }
        }
    )
except ClientError as e:
    print(e.response['Error']['Message'])
else:
    print("Message sent! Message ID: "
            + response['MessageResponse']['Result'][TOADDRESS]['MessageId'])
```

------
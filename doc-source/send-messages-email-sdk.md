# Send Email by Using the Amazon Pinpoint Email API<a name="send-messages-email-sdk"></a>

This section contains complete code examples that you can use to send email through the Amazon Pinpoint Email API by using an AWS SDK\.

------
#### [ C\# ]

Use this example to send email by using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. This example assumes that you've already installed and configured the AWS SDK for \.NET\. For more information, see [Getting Started with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-setup.html) in the *AWS SDK for \.NET Developer Guide*\.

This example assumes that you're using a shared credentials file to specify the Access Key and Secret Access Key for an existing IAM user\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

This code example was tested using the AWS SDK for \.NET version 3\.3\.29\.13 and \.NET Core runtime version 2\.1\.2\.

```
using System;
using System.Collections.Generic;
using Amazon;
using Amazon.PinpointEmail;
using Amazon.PinpointEmail.Model;

namespace PinpointEmailSDK
{
    class MainClass
    {
        // The AWS Region that you want to use to send the email. For a list of
        // AWS Regions where the Amazon Pinpoint Email API is available, see
        // https://docs.aws.amazon.com/pinpoint-email/latest/APIReference
        static string region = "us-west-2";

        // The "From" address. This address has to be verified in Amazon
        // Pinpoint in the region you're using to send email.
        static string senderAddress = "sender@example.com";

        // The address on the "To" line. If your Amazon Pinpoint account is in
        // the sandbox, this address also has to be verified.
        static string toAddress = "recipient@example.com";

        // CC and BCC addresses. If your account is in the sandbox, these
        // addresses have to be verified.
        static string ccAddress = "cc-recipient@example.com";
        static string bccAddress = "bcc-recipient@example.com";

        // The configuration set that you want to use to send the email.
        static string configSet = "ConfigSet";

        // The subject line of the email.
        static string subject = "Amazon Pinpoint Email API test";

        // The body of the email for recipients whose email clients don't
        // support HTML content.
        static string textBody = "Amazon Pinpoint Email Test (.NET)\r\n"
                               + "This email was sent using the Amazon Pinpoint Email"
                               + "API using the AWS SDK for .NET.";

        // The body of the email for recipients whose email clients support
        // HTML content.
        static string htmlBody = @"<html>
<head></head>
<body>
  <h1>Amazon Pinpoint Email API Test (AWS SDK for .NET)</h1>
  <p>This email was sent using the
    <a href='https://aws.amazon.com/pinpoint/'>Amazon Pinpoint</a> Email API
    using the <a href='https://aws.amazon.com/sdk-for-net/'>
      AWS SDK for .NET</a>.</p>
</body>
</html>";

        // The message tags that you want to apply to the email.
        static MessageTag tag0 = new MessageTag
        {
            Name = "key0",
            Value = "value0"
        };
        static MessageTag tag1 = new MessageTag
        {
            Name = "key1",
            Value = "value1"
        };

        // The character encoding the you want to use for the subject line and
        // message body of the email.
        static string charset = "UTF-8";

        public static void Main(string[] args)
        {
            using (var client = new AmazonPinpointEmailClient(RegionEndpoint.GetBySystemName(region)))
            {
                // Create a request to send the message. The request contains
                // all of the message attributes and content that were defined
                // earlier.
                var sendRequest = new SendEmailRequest
                {
                    FromEmailAddress = senderAddress,

                    // An object that contains all of the destinations that you
                    // want to send the message to. You can send a message to up
                    // to 50 recipients in a single call to the API.
                    Destination = new Destination
                    {
                        // You can include multiple To, CC, and BCC addresses.
                        ToAddresses = new List<string>
                        {
                            toAddress,
                            //additional recipients here
                        },
                        CcAddresses = new List<string>
                        {
                            ccAddress,
                            //additional recipients here
                        },
                        BccAddresses = new List<string>
                        {
                            bccAddress,
                            //additional recipients here
                        }
                    },

                    // The body of the email message.
                    Content = new EmailContent
                    {
                        // Create a new Simple email message. If you need to
                        // include attachments, you should send a RawMessage
                        // instead.
                        Simple = new Message
                        {
                            Subject = new Content
                            {
                                Charset = charset,
                                Data = subject
                            },
                            Body = new Body
                            {
                                // The text-only body of the message.
                                Text = new Content
                                {
                                    Charset = charset,
                                    Data = textBody
                                },
                                // The HTML body of the message.
                                Html = new Content
                                {
                                    Charset = charset,
                                    Data = htmlBody
                                }
                            }
                        }
                    },

                    // The configuration set that you want to use when you send
                    // this message.
                    ConfigurationSetName = configSet,

                    // A list of tags that you want to apply to this message.
                    EmailTags = new List<MessageTag>
                    {
                        tag0,
                        tag1
                    }
                };

                // Send the email, and provide a confirmation message if it's
                // sent successfully.
                try
                {
                    Console.WriteLine("Sending email using the Amazon Pinpoint Email API...");
                    var response = client.SendEmail(sendRequest);
                    Console.WriteLine("Email sent!");
                }

                // If the message can't be sent, return the error message that's
                // provided by the Amazon Pinpoint Email API.
                catch (Exception ex)
                {
                    Console.WriteLine("The email wasn't sent. ");
                    Console.WriteLine("Error message: " + ex.Message);
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

```
package com.amazonaws.samples;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;

import com.amazonaws.services.pinpointemail.AmazonPinpointEmail;
import com.amazonaws.services.pinpointemail.AmazonPinpointEmailClientBuilder;
import com.amazonaws.services.pinpointemail.model.Body;
import com.amazonaws.services.pinpointemail.model.Content;
import com.amazonaws.services.pinpointemail.model.Destination;
import com.amazonaws.services.pinpointemail.model.EmailContent;
import com.amazonaws.services.pinpointemail.model.Message;
import com.amazonaws.services.pinpointemail.model.MessageTag;
import com.amazonaws.services.pinpointemail.model.SendEmailRequest;

public class SendMessage {

    // The AWS Region that you want to use to send the email. For a list of
    // AWS Regions where the Amazon Pinpoint Email API is available, see 
    // https://docs.aws.amazon.com/pinpoint-email/latest/APIReference
    static final String region = "us-west-2";
    
    // The "From" address. This address has to be verified in Amazon 
    // Pinpoint in the region you're using to send email.
    static final String senderAddress = "sender@example.com";
    
    // The address on the "To" line. If your Amazon Pinpoint account is in
    // the sandbox, this address also has to be verified. 
    static final String toAddress = "recipient@example.com";
    
    // CC and BCC addresses. If your account is in the sandbox, these 
    // addresses have to be verified. 
    // In this example, there are several CC recipients. Each address is separated
    // with a comma. We convert this string to an ArrayList later in the example.
    static final String ccAddress = "cc_recipient0@example.com, cc_recipient1@example.com";
    static final String bccAddress = "bcc_recipient@example.com";
    
    // The configuration set that you want to use to send the email.
    static final String configurationSet = "ConfigSet";
    
    // The subject line of the email.
    static final String subject = "Amazon Pinpoint Email API test";
    
    // The email body for recipients with non-HTML email clients.
    static final String textBody = "Amazon Pinpoint Email Test (Java)\r\n"
            + "This email was sent using the Amazon Pinpoint "
            + "Email API using the AWS SDK for Java.";
    
    // The body of the email for recipients whose email clients support
    // HTML content.
    static final String htmlBody = "<h1>Amazon Pinpoint test (AWS SDK for Java)</h1>"
            + "<p>This email was sent through the <a href='https://aws.amazon.com/pinpoint/'>"
            + "Amazon Pinpoint</a> Email API using the "
            + "<a href='https://aws.amazon.com/sdk-for-java/'>AWS SDK for Java</a>";
        
    // The message tags that you want to apply to the email.
    static final String tagKey0 = "key0";
    static final String tagValue0 = "value0";
    static final String tagKey1 = "key1";
    static final String tagValue1 = "value1";
        
    // The character encoding the you want to use for the subject line and
    // message body of the email.
    static final String charset = "UTF-8";
    
    public static void main(String[] args) throws IOException {
    
        // Convert comma-separated lists of To, CC, and BCC Addresses into collections.
        // This works even if the string only contains a single email address.
        Collection<String> toAddresses = 
                new ArrayList<String>(Arrays.asList(toAddress.split("\\s*,\\s*")));
        Collection<String> ccAddresses = 
                new ArrayList<String>(Arrays.asList(ccAddress.split("\\s*,\\s*")));
        Collection<String> bccAddresses = 
                new ArrayList<String>(Arrays.asList(bccAddress.split("\\s*,\\s*")));
    
        try {
            // Create a new email client
            AmazonPinpointEmail client = AmazonPinpointEmailClientBuilder.standard()
                    .withRegion(region).build();
                    
            // Combine all of the components of the email to create a request.
            SendEmailRequest request = new SendEmailRequest()
                    .withFromEmailAddress(senderAddress)
                    .withConfigurationSetName(configurationSet)
                    .withDestination(new Destination()
                        .withToAddresses(toAddresses)
                        .withCcAddresses(ccAddresses)
                        .withBccAddresses(bccAddresses)
                    )
                    .withContent(new EmailContent()
                        .withSimple(new Message()
                            .withSubject(new Content()
                                .withCharset(charset)
                                .withData(subject)
                            )
                            .withBody(new Body()
                                .withHtml(new Content()
                                    .withCharset(charset)
                                    .withData(htmlBody)
                                )
                                .withText(new Content()
                                    .withCharset(charset)
                                    .withData(textBody)
                                )
                            )
                        )
                    )
                    .withEmailTags(new MessageTag()
                        .withName(tagKey0)
                        .withValue(tagValue0)
                    )
                    .withEmailTags(new MessageTag()
                        .withName(tagKey1)
                        .withValue(tagValue1)
                    );
            client.sendEmail(request);
            System.out.println("Email sent!");
            System.out.println(request);
        } catch (Exception ex) {
            System.out.println("The email wasn't sent. Error message: " 
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

var AWS = require('aws-sdk');

// The AWS Region that you want to use to send the email. For a list of
// AWS Regions where the Amazon Pinpoint Email API is available, see
// https://docs.aws.amazon.com//pinpoint-email/latest/APIReference
var aws_region = "us-west-2";

// The "From" address. This address has to be verified in Amazon Pinpoint
// in the region that you use to send email.
var senderAddress = "Mary Major <sender@example.com>";

// The address on the "To" line. If your Amazon Pinpoint account is in
// the sandbox, this address also has to be verified.
// Note: All recipient addresses in this example are in arrays, which makes it
// easier to specify multiple recipients. Alternatively, you can make these
// variables strings, and then modify the To/Cc/BccAddresses attributes in the
// params variable so that it passes an array for each recipient type.
var toAddresses = [ "recipient@example.com" ];

// CC and BCC addresses. If your account is in the sandbox, these
// addresses have to be verified.
var ccAddresses = [ "cc_recipient1@example.com","cc_recipient2@example.com" ];
var bccAddresses = [ "bcc_recipient@example.com" ];

// The configuration set that you want to use to send the email.
var configuration_set = "ConfigSet";

// The subject line of the email.
var subject = "Amazon Pinpoint Test (AWS SDK for JavaScript in Node.js)";

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
    <a href='https://aws.amazon.com//pinpoint/'>the Amazon Pinpoint Email API</a> using the
    <a href='https://aws.amazon.com//sdk-for-node-js/'>
      AWS SDK for JavaScript in Node.js</a>.</p>
</body>
</html>`;

// The message tags that you want to apply to the email.
var tag0 = { 'Name':'key0', 'Value':'value0' };
var tag1 = { 'Name':'key1', 'Value':'value1' };

// The character encoding the you want to use for the subject line and
// message body of the email.
var charset = "UTF-8";

// Specify that you're using a shared credentials file, and specify which
// profile to use in the shared credentials file.
var credentials = new AWS.SharedIniFileCredentials({profile: 'default'});
AWS.config.credentials = credentials;

// Specify the region.
AWS.config.update({region:aws_region});

//Create a new PinpointEmail object.
var pinpointEmail = new AWS.PinpointEmail();

// Specify the parameters to pass to the API.
var params = {
  FromEmailAddress: senderAddress,
  Destination: {
    ToAddresses: toAddresses,
    CcAddresses: ccAddresses,
    BccAddresses: bccAddresses
  },
  Content: {
    Simple: {
      Body: {
        Html: {
          Data: body_html,
          Charset: charset
        },
        Text: {
          Data: body_text,
          Charset: charset
        }
      },
      Subject: {
        Data: subject,
        Charset: charset
      }
    }
  },
  ConfigurationSetName: configuration_set,
  EmailTags: [
    tag0,
    tag1
  ]
};

//Try to send the email.
pinpointEmail.sendEmail(params, function(err, data) {
  // If something goes wrong, print an error message.
  if(err) {
    console.log(err.message);
  } else {
    console.log("Email sent! Message ID: ", data.MessageId);
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
# AWS Regions where the Amazon Pinpoint Email API is available, see
# https://docs.aws.amazon.com/pinpoint-email/latest/APIReference
AWS_REGION = "us-west-2"

# The "From" address. This address has to be verified in
# Amazon Pinpoint in the region you're using to send email.
SENDER = "Mary Major <sender@example.com>"

# The addresses on the "To" line. If your Amazon Pinpoint account is in
# the sandbox, these addresses also have to be verified.
TOADDRESSES = ["recipient@example.com"]

# CC and BCC addresses. If your account is in the sandbox, these
# addresses have to be verified.
CCADDRESSES = ["cc_recipient1@example.com", "cc_recipient2@example.com"]
BCCADDRESSES = ["bcc_recipient@example.com"]

# The configuration set that you want to use to send the email.
CONFIGURATION_SET = "ConfigSet"

# The subject line of the email.
SUBJECT = "Amazon Pinpoint Test (SDK for Python)"

# The body of the email for recipients whose email clients don't support HTML
# content.
BODY_TEXT = """Amazon Pinpoint Test (SDK for Python)
-------------------------------------
This email was sent with Amazon Pinpoint using the AWS SDK for Python.
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
      AWS SDK for Python</a>.</p>
</body>
</html>
            """

# The message tags that you want to apply to the email.
TAG0 = {'Name': 'key0', 'Value': 'value0'}
TAG1 = {'Name': 'key1', 'Value': 'value1'}

# The character encoding that you want to use for the subject line and message
# body of the email.
CHARSET = "UTF-8"

# Create a new Pinpoint resource and specify a region.
client = boto3.client('pinpoint-email', region_name=AWS_REGION)

# Send the email.
try:
    # Create a request to send the email. The request contains all of the
    # message attributes and content that were defined earlier.
    response = client.send_email(

        FromEmailAddress=SENDER,

        # An object that contains all of the email addresses that you want to
        # send the message to. You can send a message to up to 50 recipients in
        # a single call to the API.
        Destination={
            'ToAddresses': TOADDRESSES,
            'CcAddresses': CCADDRESSES,
            'BccAddresses': BCCADDRESSES
        },
        # The body of the email message.
        Content={
            # Create a new Simple message. If you need to include attachments,
            # you should send a RawMessage instead.
            'Simple': {
                'Subject': {
                    'Charset': CHARSET,
                    'Data': SUBJECT,
                },
                'Body': {
                    'Html': {
                        'Charset': CHARSET,
                        'Data': BODY_HTML
                    },
                    'Text': {
                        'Charset': CHARSET,
                        'Data': BODY_TEXT,
                    }
                }
            }
        },
        # The configuration set that you want to use when you send this message.
        ConfigurationSetName=CONFIGURATION_SET,
        EmailTags=[
            TAG0,
            TAG1
        ]
    )
# Display an error if something goes wrong.
except ClientError as e:
    print("The message wasn't sent. Error message: \"" + e.response['Error']['Message'] + "\"")
else:
    print("Email sent!")
    print("Message ID: " + response['MessageId'])
```

------
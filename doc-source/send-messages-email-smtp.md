# Send email by using the Amazon Pinpoint SMTP interface<a name="send-messages-email-smtp"></a>

This section contains complete code examples that you can use to send email from your apps by using the Amazon Pinpoint SMTP interface\. These examples use standard email\-sending libraries whenever possible\.

These examples assume that you've already created an Amazon Pinpoint SMTP user name and password\. For more information, see [Obtaining SMTP credentials](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-email-send-smtp.html#channels-email-send-smtp-credentials) in the *Amazon Pinpoint User Guide*\.

------
#### [ C\# ]

Use this example to send email by using classes in the \.NET [System\.Net\.Mail](https://docs.microsoft.com/en-us/dotnet/api/system.net.mail?view=netframework-4.7.2) namespace\.

```
using System;
using System.Net;
using System.Net.Mail;

namespace PinpointEmailSMTP
{
    class MainClass
    {
        // If you're using Amazon Pinpoint in a region other than US West (Oregon), 
        // replace email-smtp.us-west-2.amazonaws.com with the Amazon Pinpoint SMTP  
        // endpoint in the appropriate AWS Region.
        static string smtpEndpoint = "email-smtp.us-west-2.amazonaws.com";

        // The port to use when connecting to the SMTP server.
        static int port = 587;

        // Replace sender@example.com with your "From" address. 
        // This address must be verified with Amazon Pinpoint.
        static string senderName = "Mary Major"; 
        static string senderAddress = "sender@example.com";

        // Replace recipient@example.com with a "To" address. If your account 
        // is still in the sandbox, this address must be verified.
        static string toAddress = "recipient@example.com";
    
        // CC and BCC addresses. If your account is in the sandbox, these 
        // addresses have to be verified.
        static string ccAddress = "cc-recipient@example.com"; 
        static string bccAddress = "bcc-recipient@example.com";

        // Replace smtp_username with your Amazon Pinpoint SMTP user name.
        static string smtpUsername = "AKIAIOSFODNN7EXAMPLE";

        // Replace smtp_password with your Amazon Pinpoint SMTP password.
        static string smtpPassword = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";

        // (Optional) the name of a configuration set to use for this message.
        static string configurationSet = "ConfigSet";
        
        // The subject line of the email
        static string subject =
            "Amazon Pinpoint test (SMTP interface accessed using C#)";

        // The body of the email for recipients whose email clients don't 
        // support HTML content.
        static AlternateView textBody = AlternateView.
            CreateAlternateViewFromString("Amazon Pinpoint Email Test (.NET)\r\n"
                    + "This email was sent using the Amazon Pinpoint SMTP "
                    + "interface.", null, "text/plain");

        // The body of the email for recipients whose email clients support
        // HTML content.
        static AlternateView htmlBody = AlternateView.
            CreateAlternateViewFromString("<html><head></head><body>"
                    + "<h1>Amazon Pinpoint SMTP Interface Test</h1><p>This "
                    + "email was sent using the "
                    + "<a href='https://aws.amazon.com/pinpoint/'>Amazon Pinpoint"
                    + "</a> SMTP interface.</p></body></html>", null, "text/html");

        // The message tags that you want to apply to the email.
        static string tag0 = "key0=value0";
        static string tag1 = "key1=value1";

        public static void Main(string[] args)
        {
            // Create a new MailMessage object
            MailMessage message = new MailMessage();
            
            // Add sender and recipient email addresses to the message
            message.From = new MailAddress(senderAddress,senderName);
            message.To.Add(new MailAddress(toAddress));
            message.CC.Add(new MailAddress(ccAddress));
            message.Bcc.Add(new MailAddress(bccAddress));
            
            // Add the subject line, text body, and HTML body to the message
            message.Subject = subject;
            message.AlternateViews.Add(textBody);
            message.AlternateViews.Add(htmlBody);
            
            // Add optional headers for configuration set and message tags to the message
            message.Headers.Add("X-SES-CONFIGURATION-SET", configurationSet);
            message.Headers.Add("X-SES-MESSAGE-TAGS", tag0);
            message.Headers.Add("X-SES-MESSAGE-TAGS", tag1);

            using (var client = new System.Net.Mail.SmtpClient(smtpEndpoint, port))
            {
                // Create a Credentials object for connecting to the SMTP server
                client.Credentials =
                    new NetworkCredential(smtpUsername, smtpPassword);

                client.EnableSsl = true;
                
                // Send the message
                try
                {
                    Console.WriteLine("Attempting to send email...");
                    client.Send(message);
                    Console.WriteLine("Email sent!");
                }
                // Show an error message if the message can't be sent
                catch (Exception ex)
                {
                    Console.WriteLine("The email wasn't sent.");
                    Console.WriteLine("Error message: " + ex.Message);
                }
            }
        }
    }
}
```

------
#### [ Java ]

Use this example to send email by using the [JavaMail API](https://javaee.github.io/javamail/)\. 

```
import java.util.Properties;

import javax.mail.Message;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class SendEmail {

    // If you're using Amazon Pinpoint in a region other than US West (Oregon),
    // replace email-smtp.us-west-2.amazonaws.com with the Amazon Pinpoint SMTP
    // endpoint in the appropriate AWS Region.
    static final String smtpEndpoint = "email-smtp.us-west-2.amazonaws.com";

    // The port to use when connecting to the SMTP server.
    static final int port = 587;

    // Replace sender@example.com with your "From" address.
    // This address must be verified with Amazon Pinpoint.
    static final String senderName= "Mary Major";
    static final String senderAddress = "sender@example.com";

    // Replace recipient@example.com with a "To" address. If your account
    // is still in the sandbox, this address must be verified. To specify
    // multiple addresses, separate each address with a comma.
    static final String toAddresses = "recipient@example.com";

    // CC and BCC addresses. If your account is in the sandbox, these
    // addresses have to be verified. To specify multiple addresses, separate
    // each address with a comma.
    static final String ccAddresses = "cc-recipient0@example.com,cc-recipient1@example.com";
    static final String bccAddresses = "bcc-recipient@example.com";

    // Replace smtp_username with your Amazon Pinpoint SMTP user name.
    static final String smtpUsername = "AKIAIOSFODNN7EXAMPLE";

    // Replace smtp_password with your Amazon Pinpoint SMTP password.
    static final String smtpPassword = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";

    // (Optional) the name of a configuration set to use for this message.
    static final String configurationSet = "ConfigSet";

    // The subject line of the email
    static final String subject = "Amazon Pinpoint test (SMTP interface accessed using Java)";

    // The body of the email for recipients whose email clients don't
    // support HTML content.
    static final String htmlBody = String.join(
    	    System.getProperty("line.separator"),
    	    "<h1>Amazon Pinpoint SMTP Email Test</h1>",
    	    "<p>This email was sent with Amazon Pinpoint using the ",
    	    "<a href='https://github.com/javaee/javamail'>Javamail Package</a>",
    	    " for <a href='https://www.java.com'>Java</a>."
    );

    // The message tags that you want to apply to the email.
    static final String tag0 = "key0=value0";
    static final String tag1 = "key1=value1";

    public static void main(String[] args) throws Exception {

        // Create a Properties object to contain connection configuration information.
    	Properties props = System.getProperties();
    	props.put("mail.transport.protocol", "smtp");
    	props.put("mail.smtp.port", port);
    	props.put("mail.smtp.starttls.enable", "true");
    	props.put("mail.smtp.auth", "true");

        // Create a Session object to represent a mail session with the specified properties.
    	Session session = Session.getDefaultInstance(props);

        // Create a message with the specified information.
        MimeMessage msg = new MimeMessage(session);
        msg.setFrom(new InternetAddress(senderAddress,senderName));
        msg.setRecipients(Message.RecipientType.TO, InternetAddress.parse(toAddresses));
        msg.setRecipients(Message.RecipientType.CC, InternetAddress.parse(ccAddresses));
        msg.setRecipients(Message.RecipientType.BCC, InternetAddress.parse(bccAddresses));

        msg.setSubject(subject);
        msg.setContent(htmlBody,"text/html");

        // Add headers for configuration set and message tags to the message.
        msg.setHeader("X-SES-CONFIGURATION-SET", configurationSet);
        msg.setHeader("X-SES-MESSAGE-TAGS", tag0);
        msg.setHeader("X-SES-MESSAGE-TAGS", tag1);

        // Create a transport.
        Transport transport = session.getTransport();

        // Send the message.
        try {
            System.out.println("Sending...");

            // Connect to Amazon Pinpoint using the SMTP username and password you specified above.
            transport.connect(smtpEndpoint, smtpUsername, smtpPassword);

            // Send the email.
            transport.sendMessage(msg, msg.getAllRecipients());
            System.out.println("Email sent!");
        }
        catch (Exception ex) {
            System.out.println("The email wasn't sent. Error message: "
                + ex.getMessage());
        }
        finally {
            // Close the connection to the SMTP server.
            transport.close();
        }
    }
}
```

------
#### [ JavaScript \(Node\.js\) ]

Use this example to send email by using the [Nodemailer](https://nodemailer.com) module for Node\.js\.

```
/* 
This code uses callbacks to handle asynchronous function responses.
It currently demonstrates using an async-await pattern. 
AWS supports both the async-await and promises patterns.
For more information, see the following: 
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/calling-services-asynchronously.html
https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-handler.html 
*/

"use strict";
const nodemailer = require("nodemailer");

// If you're using Amazon Pinpoint in a region other than US West (Oregon),
// replace email-smtp.us-west-2.amazonaws.com with the Amazon Pinpoint SMTP
// endpoint in the appropriate AWS Region.
const smtpEndpoint = "email-smtp.us-west-2.amazonaws.com";

// The port to use when connecting to the SMTP server.
const port = 587;

// Replace sender@example.com with your "From" address.
// This address must be verified with Amazon Pinpoint.
const senderAddress = "Mary Major <sender@example.com>";

// Replace recipient@example.com with a "To" address. If your account
// is still in the sandbox, this address must be verified. To specify
// multiple addresses, separate each address with a comma.
var toAddresses = "recipient@example.com";

// CC and BCC addresses. If your account is in the sandbox, these
// addresses have to be verified. To specify multiple addresses, separate
// each address with a comma.
var ccAddresses = "cc-recipient0@example.com,cc-recipient1@example.com";
var bccAddresses = "bcc-recipient@example.com";

// Replace smtp_username with your Amazon Pinpoint SMTP user name.
const smtpUsername = "AKIAIOSFODNN7EXAMPLE";

// Replace smtp_password with your Amazon Pinpoint SMTP password.
const smtpPassword = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";

// (Optional) the name of a configuration set to use for this message.
var configurationSet = "ConfigSet";

// The subject line of the email
var subject = "Amazon Pinpoint test (Nodemailer)";

// The email body for recipients with non-HTML email clients.
var body_text = `Amazon Pinpoint Test (Nodemailer)
---------------------------------
This email was sent through the Amazon Pinpoint SMTP interface using Nodemailer.
`;

// The body of the email for recipients whose email clients support HTML content.
var body_html = `<html>
<head></head>
<body>
  <h1>Amazon Pinpoint Test (Nodemailer)</h1>
  <p>This email was sent with <a href='https://aws.amazon.com/pinpoint/'>Amazon Pinpoint</a>
        using <a href='https://nodemailer.com'>Nodemailer</a> for Node.js.</p>
</body>
</html>`;

// The message tags that you want to apply to the email.
var tag0 = "key0=value0";
var tag1 = "key1=value1";

async function main(){

  // Create the SMTP transport.
  let transporter = nodemailer.createTransport({
    host: smtpEndpoint,
    port: port,
    secure: false, // true for 465, false for other ports
    auth: {
      user: smtpUsername,
      pass: smtpPassword
    }
  });

  // Specify the fields in the email.
  let mailOptions = {
    from: senderAddress,
    to: toAddresses,
    subject: subject,
    cc: ccAddresses,
    bcc: bccAddresses,
    text: body_text,
    html: body_html,
    // Custom headers for configuration set and message tags.
    headers: {
      'X-SES-CONFIGURATION-SET': configurationSet,
      'X-SES-MESSAGE-TAGS': tag0,
      'X-SES-MESSAGE-TAGS': tag1
    }
  };

  // Send the email.
  let info = await transporter.sendMail(mailOptions)

  console.log("Message sent! Message ID: ", info.messageId);
}

main().catch(console.error);
```

------
#### [ Python ]

Use this example to send email by using the Python [email](https://docs.python.org/3/library/email.html) and [smtplib](https://docs.python.org/3/library/smtplib.html) libraries\.

```
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import logging
import smtplib

logger = logging.getLogger(__name__)


def send_smtp_message(
        smtp_server, smtp_username, smtp_password, sender, to_address, cc_address,
        subject, html_message, text_message):
    """
    Sends an email by using an Amazon Pinpoint SMTP server.

    :param smtp_server: An smtplib SMTP session.
    :param smtp_username: The username to use to connect to the SMTP server.
    :param smtp_password: The password to use to connect to the SMTP server.
    :param sender: The "From" address. This address must be verified.
    :param to_address: The "To" address. If your account is still in the sandbox,
                       this address must be verified.
    :param cc_address: The "CC" address. If your account is still in the sandbox,
                       this address must be verified.
    :param subject: The subject line of the email.
    :param html_message: The HTML body of the email.
    :param text_message: The email body for recipients with non-HTML email clients.
    """
    # Create message container. The correct MIME type is multipart/alternative.
    msg = MIMEMultipart('alternative')
    msg['From'] = sender
    msg['To'] = to_address
    msg['Cc'] = cc_address
    msg['Subject'] = subject
    msg.attach(MIMEText(html_message, 'html'))
    msg.attach(MIMEText(text_message, 'plain'))

    smtp_server.ehlo()
    smtp_server.starttls()
    # smtplib docs recommend calling ehlo() before and after starttls()
    smtp_server.ehlo()
    smtp_server.login(smtp_username, smtp_password)
    # Uncomment the next line to send SMTP server responses to stdout.
    # smtp_server.set_debuglevel(1)
    smtp_server.sendmail(sender, to_address, msg.as_string())


def main():
    # If you're using Amazon Pinpoint in an AWS Region other than US West (Oregon),
    # replace email-smtp.us-west-2.amazonaws.com with the Amazon Pinpoint SMTP
    # endpoint in the appropriate AWS Region.
    host = "email-smtp.us-west-2.amazonaws.com"
    port = 587
    sender = 'sender@example.com'
    to_address = 'recipient@example.com'
    cc_address = "cc_recipient@example.com"
    subject = 'Amazon Pinpoint Test (Python smtplib)'
    text_message = (
        "Amazon Pinpoint Test\r\n"
        "This email was sent through the Amazon Pinpoint SMTP "
        "interface using the Python smtplib package.")
    html_message = """<html>
    <head></head>
    <body>
      <h1>Amazon Pinpoint SMTP Email Test</h1>
      <p>This email was sent with Amazon Pinpoint using the
        <a href='https://www.python.org/'>Python</a>
        <a href='https://docs.python.org/3/library/smtplib.html'>
        smtplib</a> library.</p>
    </body>
    </html>
                """

    # Replace smtp_username and smtp_password with your Amazon Pinpoint SMTP user name
    # and password.
    smtp_username = "AKIAIOSFODNN7EXAMPLE"
    smtp_password = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

    print("Sending email through SMTP server.")
    try:
        with smtplib.SMTP(host, port) as smtp_server:
            send_smtp_message(
                smtp_server, smtp_username, smtp_password, sender, to_address,
                cc_address, subject, html_message, text_message)
    except Exception:
        logger.exception("Couldn't send message.")
        raise
    else:
        print("Email sent!")


if __name__ == '__main__':
    main()
```

------
# Email Events<a name="event-streams-data-email"></a>

When you send email messages, Amazon Pinpoint streams data that provides additional information about the following types of events:
+ Sends
+ Deliveries
+ Bounces
+ Complaints
+ Opens
+ Clicks
+ Rejections
+ Unsubscribes

The event types in the preceding list are explained in detail in [Email Event Attributes](#event-streams-data-email-attributes)\.

## Sample Events<a name="event-streams-data-email-send-example"></a>

**Email Send**  
The JSON object for an email send event contains the data shown in the following example\.

```
{
  "event_type": "_email.send",
  "event_timestamp": 1564618621380,
  "arrival_timestamp": 1564618622025,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "9a311b17-6f8e-4093-be61-4d0bbexample"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "feedback": "received"
  },
  "awsAccountId": "123456789012",
  "facets": {
    "email_channel": {
      "mail_event": {
        "mail": {
          "message_id": "0200000073rnbmd1-mbvdg3uo-q8ia-m3ku-ibd3-ms77kexample-000000",
          "message_send_timestamp": 1564618621380,
          "from_address": "sender@example.com",
          "destination": ["recipient@example.com"],
          "headers_truncated": false,
          "headers": [{
            "name": "From",
            "value": "sender@example.com"
          }, {
            "name": "To",
            "value": "recipient@example.com"
          }, {
            "name": "Subject",
            "value": "Amazon Pinpoint Test"
          }, {
            "name": "MIME-Version",
            "value": "1.0"
          }, {
            "name": "Content-Type",
            "value": "multipart/alternative;  boundary=\"----=_Part_314159_271828\""
          }],
          "common_headers": {
            "from": "sender@example.com",
            "to": ["recipient@example.com"],
            "subject": "Amazon Pinpoint Test"
          }
        },
        "send": {}
      }
    }
  }
}
```

**Email Delivered**  
The JSON object for an email send event contains the data shown in the following example\.

```
{
  "event_type": "_email.delivered",
  "event_timestamp": 1564618621380,
  "arrival_timestamp": 1564618622690,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "e9a3000d-daa2-40dc-ac47-1cd34example"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "feedback": "delivered"
  },
  "awsAccountId": "123456789012",
  "facets": {
    "email_channel": {
      "mail_event": {
        "mail": {
          "message_id": "0200000073rnbmd1-mbvdg3uo-q8ia-m3ku-ibd3-ms77kexample-000000",
          "message_send_timestamp": 1564618621380,
          "from_address": "sender@example.com",
          "destination": ["recipient@example.com"],
          "headers_truncated": false,
          "headers": [{
            "name": "From",
            "value": "sender@example.com"
          }, {
            "name": "From",
            "value": "recipient@example.com"
          }, {
            "name": "Subject",
            "value": "Amazon Pinpoint Test"
          }, {
            "name": "MIME-Version",
            "value": "1.0"
          }, {
            "name": "Content-Type",
            "value": "multipart/alternative;  boundary=\"----=_Part_314159_271828\""
          }],
          "common_headers": {
            "from": "sender@example.com",
            "to": ["recipient@example.com"],
            "subject": "Amazon Pinpoint Test"
          }
        },
        "delivery": {
          "smtp_response": "250 ok:  Message 82080542 accepted",
          "reporting_mta": "a8-53.smtp-out.amazonses.com",
          "recipients": ["recipient@example.com"],
          "processing_time_millis": 1310
        }
      }
    }
  }
}
```

**Email Click**  
The JSON object for an email click event contains the data shown in the following example\.

```
{
  "event_type": "_email.click",
  "event_timestamp": 1564618621380,
  "arrival_timestamp": 1564618713751,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "49c1413e-a69c-46dc-b1c4-6470eexample"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "feedback": "https://aws.amazon.com/pinpoint/"
  },
  "awsAccountId": "123456789012",
  "facets": {
    "email_channel": {
      "mail_event": {
        "mail": {
          "message_id": "0200000073rnbmd1-mbvdg3uo-q8ia-m3ku-ibd3-ms77kexample-000000",
          "message_send_timestamp": 1564618621380,
          "from_address": "sender@example.com",
          "destination": ["recipient@example.com"],
          "headers_truncated": false,
          "headers": [{
            "name": "From",
            "value": "sender@example.com"
          }, {
            "name": "From",
            "value": "recipient@example.com"
          }, {
            "name": "Subject",
            "value": "Amazon Pinpoint Test"
          }, {
            "name": "MIME-Version",
            "value": "1.0"
          }, {
            "name": "Content-Type",
            "value": "multipart/alternative;  boundary=\"----=_Part_314159_271828\""
          }, {
            "name": "Message-ID",
            "value": "null"
          }],
          "common_headers": {
            "from": "sender@example.com",
            "to": ["recipient@example.com"],
            "subject": "Amazon Pinpoint Test"
          }
        },
        "click": {
          "ip_address": "72.21.198.67",
          "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1.2 Safari/605.1.15",
          "link": "https://aws.amazon.com/pinpoint/"
        }
      }
    }
  }
}
```

**Email Open**  
The JSON object for an email open event contains the data shown in the following example\.

```
{
  "event_type": "_email.open",
  "event_timestamp": 1564618621380,
  "arrival_timestamp": 1564618712316,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "8dc1f651-b3ec-46fc-9b67-2a050example"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "feedback": "opened"
  },
  "awsAccountId": "123456789012",
  "facets": {
    "email_channel": {
      "mail_event": {
        "mail": {
          "message_id": "0200000073rnbmd1-mbvdg3uo-q8ia-m3ku-ibd3-ms77kexample-000000",
          "message_send_timestamp": 1564618621380,
          "from_address": "sender@example.com",
          "destination": ["recipient@example.com"],
          "headers_truncated": false,
          "headers": [{
            "name": "From",
            "value": "sender@example.com"
          }, {
            "name": "From",
            "value": "recipient@example.com"
          }, {
            "name": "Subject",
            "value": "Amazon Pinpoint Test"
          }, {
            "name": "MIME-Version",
            "value": "1.0"
          }, {
            "name": "Content-Type",
            "value": "multipart/alternative;  boundary=\"----=_Part_314159_271828\""
          }, {
            "name": "Message-ID",
            "value": "null"
          }],
          "common_headers": {
            "from": "sender@example.com",
            "to": ["recipient@example.com"],
            "subject": "Amazon Pinpoint Test"
          }
        },
        "open": {
          "ip_address": "72.21.198.67",
          "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/605.1.15 (KHTML, like Gecko)"
        }
      }
    }
  }
}
```

## Email Event Attributes<a name="event-streams-data-email-attributes"></a>

This section defines the attributes that are included in the event stream data that Amazon Pinpoint generates when you send email\.


| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. Possible values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-email.html)  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. See the *Application* table for more information\. | 
| client | The app client installed on the device that reports the event\. See the *Client* table for more information\. | 
| device | The device that reports the event\. See the *Device* table for more information\. In events that are generated when you send a transactional message, this object is empty\. | 
| session | For email events, this object is empty\. | 
| attributes |  Attributes that are associated with the event\. For events that are reported by one of your apps, this object can include custom attributes that are defined by the app\. For events that are created when you send a campaign, this object contains attributes that are associated with the campaign\. For events that are generated when you send transactional messages, this object contains information that's related to the message itself\. See the *Attributes* table for more information\.  | 
| client\_context | For email events, this object contains an object called custom, which contains an attribute called legacy\_identifier\. The value of legacy\_identifier is equal to the ID of the project that the email was sent from\. | 
| facets |  Additional information about the message, such as the email headers\. See the *Facets* table for more information\.  | 
| awsAccountId |  The ID of the AWS account that was used to send the message\.  | 

### Application<a name="event-streams-data-email-attributes-application"></a>

Includes information about the Amazon Pinpoint project that the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| sdk |  The SDK that was used to report the event\. If you send a transactional email message calling the Amazon Pinpoint API directly, or by using the Amazon Pinpoint console, this object is empty\.  | 

### Attributes<a name="event-streams-data-email-attributes-attrs"></a>

Includes information about the campaign that produced the event\.


| Attribute | Description | 
| --- | --- | 
| feedback | For `_email.click` events, the value of this attribute is the URL of the link that the recipient clicked to generate the event\. For other events, the value represents the event type \(such as `received`, `opened`, or `clicked`\)\. | 
| treatment\_id |  If the message was sent using an A/B test campaign, this value represents the treatment number of the message\. For standard campaigns and transactional email messages, this value is 0\.  | 
| campaign\_activity\_id | The unique ID that Amazon Pinpoint generates when the event occurs\. | 
| campaign\_id |  The unique ID of the campaign that the message was sent from\.  | 

### Client<a name="event-streams-data-email-attributes-client"></a>

Includes information about the endpoint that was targeted by the campaign\.


| Attribute | Description | 
| --- | --- | 
| client\_id | The email address of the endpoint that reported the event\. | 

### Facets<a name="event-streams-data-email-attributes-facets"></a>


| Attribute | Description | 
| --- | --- | 
| email\_channel | Contains an object called `mail_event`, which contains two objects: `mail`, and an object that corresponds with the event type\. | 

### Mail<a name="event-streams-data-email-attributes-mail"></a>

Includes information about the content of the email, as well as metadata related to the message itself\.


| Attribute | Description | 
| --- | --- | 
| message\_id | The unique ID of the email message\. Amazon Pinpoint automatically generates this ID when it accepts the email\. | 
| message\_send\_timestamp | The date and time when the message was sent, shown in the format specified in [RFC 822](https://tools.ietf.org/html/rfc822)\. | 
| from\_address | The email address that the message was sent from\. | 
| destination | An array that contains the email addresses that the message was sent to\. | 
| headers\_truncated | A boolean that indicates whether the email headers were truncated\. | 
| headers |  An object that contains several name\-value pairs that correspond to the headers in the email\. This object typically contains information about the following headers: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-email.html)  | 
| common\_headers | Contains information about several common email headers, such as the date when the email was sent, and the to, from, and subject lines\. | 
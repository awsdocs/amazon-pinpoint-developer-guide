# SMS events<a name="event-streams-data-sms"></a>

If the SMS channel is enabled for a project, Amazon Pinpoint can stream event data about SMS message deliveries for the project\.

## Example<a name="event-streams-data-sms-example"></a>

The JSON object for an SMS event contains the data shown in the following example\.

```
{
  "event_type": "_SMS.SUCCESS",
  "event_timestamp": 1553104954322,
  "arrival_timestamp": 1553104954064,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "123456789012"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "sender_request_id": "565d4425-4b3a-11e9-b0a5-example",
    "campaign_activity_id": "cbcfc3c5e3bd48a8ae2b9cb41example",
    "origination_phone_number": "+12065550142",
    "destination_phone_number": "+14255550199",
    "record_status": "DELIVERED",
    "iso_country_code": "US",
    "treatment_id": "0",
    "number_of_message_parts": "1",
    "message_id": "1111-2222-3333",
    "message_type": "Transactional",
    "campaign_id": "52dc44b35c4742c98c5935269example"
  },
  "metrics": {
    "price_in_millicents_usd": 645.0
  },
  "awsAccountId": "123456789012"
}
```

## SMS event attributes<a name="event-streams-data-sms-attributes"></a>

This section defines the attributes that are included in the event stream data that Amazon Pinpoint generates when you send SMS messages\.


**Event**  

| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-sms.html)  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. For more information, see the [Application](#event-streams-data-sms-attributes-application) table\. | 
| client | Information about the app client installed on the device that reported the event\. For more information, see the [Client](#event-streams-data-sms-attributes-client) table\. | 
| device | Information about the device that reported the event\. For more information, see the [Device](#event-streams-data-sms-attributes-device) table\. For SMS events, this object is empty\. | 
| session | For SMS events, this object is empty\. | 
| attributes |  Attributes that are associated with the event\. For events that are reported by one of your apps, this object can include custom attributes that are defined by the app\. For events that are created when you send a campaign, this object contains attributes that are associated with the campaign\. For events that are generated when you send transactional messages, this object contains information that's related to the message itself\. For more information, see the [Attributes](#event-streams-data-sms-attributes-attrs) table\.  | 
| metrics |  Additional metrics that are associated with the event\. For more information, see the [Metrics](#event-streams-data-sms-attributes-metrics) table\.  | 
| awsAccountId |  The ID of the AWS account that was used to send the message\.  | 

### Application<a name="event-streams-data-sms-attributes-application"></a>

Includes information about the Amazon Pinpoint project that the event is associated with and, if applicable, the SDK that was used to report the event\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| sdk |  The SDK that was used to report the event\. If you send a transactional SMS message by calling the Amazon Pinpoint API directly or by using the Amazon Pinpoint console, this object is empty\.  | 

### Attributes<a name="event-streams-data-sms-attributes-attrs"></a>

Includes information about the attributes that are associated with the event\.


| Attribute | Description | 
| --- | --- | 
| sender\_request\_id | A unique ID that's associated with the request to send the SMS message\. | 
| campaign\_activity\_id | The unique ID of the activity within the campaign\. | 
| origination\_phone\_number |  The phone number that the message was sent from\.  | 
| destination\_phone\_number |  The phone number that you attempted to send the message to\.  | 
| record\_status |  Additional information about the status of the message\. Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-sms.html)  | 
| iso\_country\_code |  The country that's associated with the recipient's phone number, shown in ISO 3166\-1 alpha\-2 format\.  | 
| treatment\_id |  The ID of the message treatment, if the message was sent in an A/B campaign\.  | 
| treatment\_id |  If the message was sent using an A/B test campaign, this value represents the treatment number of the message\. For transactional SMS messages, this value is 0\.  | 
| number\_of\_message\_parts |  The number of message parts that Amazon Pinpoint created in order to send the message\. Generally, SMS messages can contain only 160 GSM\-7 characters or 67 non\-GSM characters, although these limits can vary by country \. If you send a message that exceeds these limits, Amazon Pinpoint automatically splits the message into smaller parts\. We bill you based on the number of message parts that you send\.  | 
| message\_id |  The unique ID that Amazon Pinpoint generates when it accepts the message\.  | 
| message\_type |  The type of message\. Possible values are **Promotional** and **Transactional**\. You specify this value when you create a campaign, or when you send transactional messages by using the [SendMessages](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-messages.html#SendMessages) operation in the Amazon Pinpoint API\.  | 
| campaign\_id |  The unique ID of the Amazon Pinpoint campaign that sent the message\.  | 

### Client<a name="event-streams-data-sms-attributes-client"></a>

Includes information about the app client that's installed on the device that reported the event\.


| Attribute | Description | 
| --- | --- | 
| client\_id |  For events that are generated by apps, this value is the unique ID of the app client that's installed on the device\. This ID is automatically generated by the AWS Mobile SDK for iOS and the AWS Mobile SDK for Android\. For events that are generated when you send campaigns and transactional messages, this value is equal to the ID of the endpoint that you sent the message to\.  | 
| cognito\_id | The unique ID assigned to the app client in the Amazon Cognito identity pool used by your app\. | 

### Device<a name="event-streams-data-sms-attributes-device"></a>

Includes information about the device that reported the event\.


| Attribute | Description | 
| --- | --- | 
| locale | The device locale\. | 
| make | The device make, such as Apple or Samsung\. | 
| model | The device model, such as iPhone\. | 
| platform | The device platform, such as ios or android\. | 

### Metrics<a name="event-streams-data-sms-attributes-metrics"></a>

Includes information about metrics that are associated with the event\.


| Attribute | Description | 
| --- | --- | 
| price\_in\_millicents\_usd | The amount that we charged you to send the message\. This price is shown in thousandths of a United States cent\. For example, if the value of this attribute is `645`, then we charged you 0\.645¢ to send the message \(645 / 1000 = 0\.645¢ = $0\.00645\)\.  This property doesn't appear for messages with an `event_type` of **\_SMS\.BUFFERED**\.   | 
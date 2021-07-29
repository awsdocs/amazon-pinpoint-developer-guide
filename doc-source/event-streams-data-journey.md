# Journey events<a name="event-streams-data-journey"></a>

If you publish a journey, Amazon Pinpoint can stream event data about the journey\. This includes event data for any email, SMS, push, or custom messages that you send from the journey\. 

See the following for information about the data that Amazon Pinpoint streams:
+ For email messages, see [Email events](event-streams-data-email.md)\. 
+ For SMS messages, see [SMS events](https://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-sms.html)\. 

## Sample event<a name="event-streams-data-journey-example"></a>

The JSON object for a journey event contains the data shown in the following sample\.

```
{
   "event_type":"_journey.send",
   "event_timestamp":1572989078843,
   "arrival_timestamp":1572989078843,
   "event_version":"3.1",
   "application":{
      "app_id":"a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
      "sdk":{

      }
   },
   "client":{
      "client_id":"d8dcf7c5-e81a-48ae-8313-f540cexample"
   },
   "device":{
      "platform":{

      }
   },
   "session":{

   },
   "attributes":{
      "journey_run_id":"edc9a0b577164d1daf72ebd15example",
      "journey_send_status":"SUCCESS",
      "journey_id":"546401670c5547b08811ac6a9example",
      "journey_activity_id":"0yKexample",
      "journey_activity_type": "EMAIL"
   },
   "client_context":{
      "custom":{
         "endpoint":"{\"ChannelType\":\"EMAIL\",\"EndpointStatus\":\"ACTIVE\",\"OptOut\":\"NONE\",\"Demographic\":{\"Timezone\":\"America/Los_Angeles\"}}"
      }
   },
   "awsAccountId":"123456789012"
}
```

## Journey event attributes<a name="event-streams-data-journey-attributes"></a>

This section defines the attributes that are included in the event stream data that Amazon Pinpoint generates for a journey\.


| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. For journey events, the value for this attribute is always `_journey.send`, which indicates that Amazon Pinpoint executed the journey\.  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. For more information, see the [Application](#event-streams-data-journey-attributes-application) table\. | 
| client | Information about the endpoint that's associated with the event\. For more information, see the [Client](#event-streams-data-journey-attributes-client) table\. | 
| device | Information about the device that reported the event\. For journeys, this object is empty\. | 
| session | Information about the session that generated the event\. For journeys, this object is empty\. | 
| attributes |  Attributes that are associated with the journey and journey activity that generated the event\. For more information, see the [Attributes](#event-streams-data-journey-attributes-attrs) table\.  | 
| client\_context | Contains a custom object, which contains an endpoint property\. The endpoint property contains the contents of the endpoint record for the endpoint that's associated with the event\. | 
| awsAccountId |  The ID of the AWS account that was used to execute the journey\.  | 

### Application<a name="event-streams-data-journey-attributes-application"></a>

Includes information about the Amazon Pinpoint project that's associated with the event\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| sdk |  The SDK that was used to report the event\.   | 

### Client<a name="event-streams-data-journey-attributes-client"></a>

Includes information about the endpoint that's associated with the event\.


| Attribute | Description | 
| --- | --- | 
| client\_id | The ID of the endpoint\. | 

### Attributes<a name="event-streams-data-journey-attributes-attrs"></a>

Includes information about the journey that generated the event\.


| Attribute | Description | 
| --- | --- | 
| journey\_run\_id |  The unique ID of the journey run that generated the event\. Amazon Pinpoint generates and assigns this ID automatically to each new run of a journey\.  | 
| journey\_send\_status | Indicates the delivery status of the message that's associated with the event\. Possible values include:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-journey.html) | 
| journey\_id | The unique ID of the journey that generated the event\. | 
| journey\_activity\_id | The unique ID of the journey activity that generated the event\. | 
| journey\_activity\_type | The event's journey activity type\. This can be **EMAIL**, **SMS**, **PUSH**, or **CUSTOM**\.  **VOICE **is not a supported journey activity type\. | 
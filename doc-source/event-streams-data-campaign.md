# Campaign events<a name="event-streams-data-campaign"></a>

If you use Amazon Pinpoint to send campaigns through any channel, Amazon Pinpoint can stream event data about those campaigns\. This includes event data for any email or SMS messages that you send from a campaign\. For detailed information about the data that Amazon Pinpoint streams for those types of messages, see [Email events](event-streams-data-email.md) and [SMS events](event-streams-data-sms.md)\. 

## Sample event<a name="event-streams-data-campaign-example"></a>

The JSON object for a campaign event contains the data shown in the following sample\.

```
{
  "event_type": "_campaign.send",
  "event_timestamp": 1562109497426,
  "arrival_timestamp": 1562109497494,
  "event_version": "3.1",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "sdk": {}
  },
  "client": {
    "client_id": "d8dcf7c5-e81a-48ae-8313-f540cexample"
  },
  "device": {
    "platform": {}
  },
  "session": {},
  "attributes": {
    "treatment_id": "0",
    "campaign_activity_id": "5473285727f04865bc673e527example",
    "delivery_type": "GCM",
    "campaign_id": "4f8d6097c2e8400fa3081d875example",
    "campaign_send_status": "SUCCESS"
  },
  "client_context": {
    "custom": {
      "endpoint": "{\"ChannelType\":\"GCM\",\"EndpointStatus\":\"ACTIVE\",
          ↳\"OptOut\":\"NONE\",\"RequestId\":\"ec229696-9d1e-11e9-8bf1-85d0aexample\",
          ↳\"EffectiveDate\":\"2019-07-02T23:12:54.836Z\",\"User\":{}}"
    }
  },
  "awsAccountId": "123456789012"
}
```

## Campaign event attributes<a name="event-streams-data-campaign-attributes"></a>

This section defines the attributes that are included in the campaign event stream\.


| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-campaign.html)  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. For more information, see the [Application](#event-streams-data-campaign-attributes-application) table\. | 
| client | Information about the endpoint that the event is associated with\. For more information, see the [Client](#event-streams-data-campaign-attributes-client) table\. | 
| device | Information about the device that reported the event\. For campaign and transactional messages, this object is empty\. | 
| session | Information about the session that generated the event\. For campaigns, this object is empty\. | 
| attributes |  Attributes that are associated with the event\. For events that are reported by one of your apps, this object can include custom attributes that are defined by the app\. For events that are created when you send a campaign, this object contains attributes that are associated with the campaign\. For events that are generated when you send transactional messages, this object contains information that's related to the message itself\. For more information, see the [Attributes](#event-streams-data-campaign-attributes-attrs) table\.  | 
| client\_context | Contains a custom object, which contains an endpoint property\. The endpoint property contains the contents of the endpoint record for the endpoint that the campaign was sent to\. | 
| awsAccountId |  The ID of the AWS account that was used to send the message\.  | 

### Application<a name="event-streams-data-campaign-attributes-application"></a>

Includes information about the Amazon Pinpoint project that the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| sdk |  The SDK that was used to report the event\.   | 

### Attributes<a name="event-streams-data-campaign-attributes-attrs"></a>

Includes information about the campaign that produced the event\.


| Attribute | Description | 
| --- | --- | 
| treatment\_id |  If the message was sent using an A/B test campaign, this value represents the treatment number of the message\. For standard campaigns, this value is `0`\.  | 
| campaign\_activity\_id | The unique ID that Amazon Pinpoint generates when the event occurs\. | 
| delivery\_type | The delivery method for the campaign\. This attribute should not be confused with the ChannelType field specified under the endpoint property of client\_context\. ChannelType is typically based on the endpoint that the message is being sent to\. For channels that support only one endpoint type — for example, the email channel — the delivery\_type and ChannelType fields have the same value, EMAIL\. However, this may not be true for channels that support different endpoint types, such as custom channels\. Since a custom channel can be used for different endpoints — EMAIL, SMS, CUSTOM, etc\. — the delivery\_type identifies a custom delivery event, CUSTOM, while the ChannelType specifies the type of the endpoint that the campaign was sent to — EMAIL, SMS, CUSTOM, etc\. For more information on creating custom channels, see [Creating custom channels in Amazon Pinpoint](channels-custom.md)\. Possible values are:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-campaign.html) | 
| campaign\_id |  The unique ID of the campaign that the message was sent from\.  | 
| campaign\_send\_status | Indicates the status of the campaign for the target endpoint\. Possible values include:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-campaign.html)  | 

### Client<a name="event-streams-data-campaign-attributes-client"></a>

Includes information about the endpoint that was targeted by the campaign\.


| Attribute | Description | 
| --- | --- | 
| client\_id | The ID of the endpoint that the campaign was sent to\. | 
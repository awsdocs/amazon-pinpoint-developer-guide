# App Events<a name="event-streams-data-app"></a>

After you integrate your app with Amazon Pinpoint, Amazon Pinpoint streams events about user activity and campaign message deliveries\.

## Example<a name="event-streams-data-app-example"></a>

The JSON object for an app event contains the data shown in the following example\.

```
{
  "event_type": "_session.stop",
  "event_timestamp": 1487973802507,
  "arrival_timestamp": 1487973803515,
  "event_version": "3.0",
  "application": {
    "app_id": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6",
    "cognito_identity_pool_id": "us-east-1:a1b2c3d4-e5f6-g7h8-i9j0-k1l2m3n4o5p6",
    "package_name": "main.page",
    "sdk": {
      "name": "aws-sdk-mobile-analytics-js",
      "version": "0.9.1:2.4.8"
    },
    "title": "title",
    "version_name": "1.0",
    "version_code": "1"
  },
  "client": {
    "client_id": "m3n4o5p6-a1b2-c3d4-e5f6-g7h8i9j0k1l2",
    "cognito_id": "us-east-1:i9j0k1l2-m3n4-o5p6-a1b2-c3d4e5f6g7h8"
  },
  "device": {
    "locale": {
      "code": "en_US",
      "country": "US",
      "language": "en"
    },
    "make": "generic web browser",
    "model": "Unknown",
    "platform": {
      "name": "android",
      "version": "10.10"
    }
  },
  "session": {
    "session_id": "f549dea9-1090-945d-c3d1-e4967example",
    "start_timestamp": 1487973202531,
    "stop_timestamp": 1487973802507
  },
  "attributes": {},
  "metrics": {}
}
```

## App Event Attributes<a name="event-streams-data-app-attributes"></a>

This section defines the attributes that are included in the app event stream\.


| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. Possible values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-app.html)  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. See [Application](#event-streams-data-app-attributes-application) table for more information\. | 
| client | Information about the client that's installed on the device that reports the event\. See the [Client](#event-streams-data-app-attributes-client) table for more information\. | 
| device | The device that reports the event\. See the [Device](#event-streams-data-app-attributes-device) table for more information\. | 
| session | Information about the session that generated the event\. See the [Session](#event-streams-data-app-attributes-session) table for more information\. | 
| attributes |  Attributes that are associated with the event\. For events that are reported by your apps, this object includes custom attributes that you define\.  | 
| metrics | Metrics that are related to the event\. You can optionally configure your apps to send custom metrics to Amazon Pinpoint\. | 

### Application<a name="event-streams-data-app-attributes-application"></a>

Includes information about the Amazon Pinpoint project that the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| cognito\_identity\_pool\_id |  The ID of the Amazon Cognito Identity Pool that the endpoint is associated with\.  | 
| package\_name |  The name of your application package, such as `com.example.my_app`\.  | 
| sdk |  Information about the SDK that was used to report the event\. See the [SDK](#event-streams-data-app-attributes-application-sdk) table for more information\.  | 
| title |  The name of your app\.  | 
| version\_name |  The name of the version of the app, such as `V2.5`\.  | 
| version\_code |  The version number of the app, such as `3`\.  | 

#### SDK<a name="event-streams-data-app-attributes-application-sdk"></a>


| Attribute | Description | 
| --- | --- | 
| name | The name of the SDK that was used to report the event\. | 
| version | The version of the SDK\. | 

### Client<a name="event-streams-data-app-attributes-client"></a>

Includes information about the endpoint that the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| client\_id | The endpoint ID of the endpoint that the campaign was sent to\. | 
| cognito\_id | The Amazon Cognito ID token that's associated with the endpoint\. | 

### Device<a name="event-streams-data-app-attributes-device"></a>

Includes information about the device that the endpoint that generated the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| locale |  Contains information about the language and region settings for the device\. See the [Locale](#event-streams-data-app-attributes-device-locale) table for more information\.  | 
| make | The manufacturer of the device that's associated with the endpoint\. | 
| model | The model identifier of the device that's associated with the endpoint\. | 
| platform |  Contains information about the operating system of the user's device\. See the [Platform](#event-streams-data-app-attributes-device-platform) table for more information\.  | 

#### Locale<a name="event-streams-data-app-attributes-device-locale"></a>


| Attribute | Description | 
| --- | --- | 
| code | The locale identifier that's associated with the device\. | 
| country | The country or region that's associated with the device's locale\. | 
| language | The language that's associated with the device's locale\. | 

#### Platform<a name="event-streams-data-app-attributes-device-platform"></a>


| Attribute | Description | 
| --- | --- | 
| name | The name of the operating system on the user's device\. | 
| version | The version of the device's operating system\. | 

### Session<a name="event-streams-data-app-attributes-session"></a>

Includes information about the session that generated the event\.


| Attribute | Description | 
| --- | --- | 
| session\_id | A unique ID that identifies the session\. | 
| start\_timestamp | The date and time when the session began, shown as Unix time in milliseconds\. | 
| stop\_timestamp | The date and time when the session ended, shown as Unix time in milliseconds\. | 
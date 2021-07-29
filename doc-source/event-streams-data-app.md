# App events<a name="event-streams-data-app"></a>

After you integrate your application \(app\) with Amazon Pinpoint, Amazon Pinpoint can stream event data about user activity, custom events, and message deliveries for the app\.

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

## App event attributes<a name="event-streams-data-app-attributes"></a>

This section defines the attributes that are included in the app event stream\.


| Attribute | Description | 
| --- | --- | 
| event\_type |  The type of event\. Possible values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/event-streams-data-app.html)  | 
| event\_timestamp | The time when the event was reported, shown as Unix time in milliseconds\. | 
| arrival\_timestamp | The time when the event was received by Amazon Pinpoint, shown as Unix time in milliseconds\. | 
| event\_version |  The version of the event JSON schema\.  Check this version in your event\-processing application so that you know when to update the application in response to a schema update\.   | 
| application | Information about the Amazon Pinpoint project that's associated with the event\. For more information, see the [Application](#event-streams-data-app-attributes-application) table\. | 
| client | Information about the endpoint that reported the event\. For more information, see the [Client](#event-streams-data-app-attributes-client) table\. | 
| device | Information about the device that reported the event\. For more information, see the [Device](#event-streams-data-app-attributes-device) table\. | 
| session | Information about the session that generated the event\. For more information, see the [Session](#event-streams-data-app-attributes-session) table\. | 
| attributes |  Attributes that are associated with the event\. For events that are reported by your apps, this object includes custom attributes that you define\.  | 
| metrics | Metrics that are related to the event\. You can optionally configure your apps to send custom metrics to Amazon Pinpoint\. | 

### Application<a name="event-streams-data-app-attributes-application"></a>

Includes information about the Amazon Pinpoint project that the event is associated with\.


| Attribute | Description | 
| --- | --- | 
| app\_id |  The unique ID of the Amazon Pinpoint project that reported the event\.  | 
| cognito\_identity\_pool\_id |  The ID of the Amazon Cognito Identity Pool that the endpoint is associated with\.  | 
| package\_name |  The name of the app package, such as `com.example.my_app`\.  | 
| sdk |  Information about the SDK that was used to report the event\. For more information, see the [SDK](#event-streams-data-app-attributes-application-sdk) table\.  | 
| title |  The name of the app\.  | 
| version\_name |  The name of the version of the app, such as `V2.5`\.  | 
| version\_code |  The version number of the app, such as `3`\.  | 

#### SDK<a name="event-streams-data-app-attributes-application-sdk"></a>

Includes information about the SDK that was used to report the event\.


| Attribute | Description | 
| --- | --- | 
| name | The name of the SDK that was used to report the event\. | 
| version | The version of the SDK\. | 

### Client<a name="event-streams-data-app-attributes-client"></a>

Includes information about the endpoint that generated the event\.


| Attribute | Description | 
| --- | --- | 
| client\_id | The ID of the endpoint\. | 
| cognito\_id | The Amazon Cognito ID token that's associated with the endpoint\. | 

### Device<a name="event-streams-data-app-attributes-device"></a>

Includes information about the device of the endpoint that generated the event\.


| Attribute | Description | 
| --- | --- | 
| locale |  Information about the language and region settings for the endpoint's device\. For more information, see the [Locale](#event-streams-data-app-attributes-device-locale) table\.  | 
| make | The manufacturer of the endpoint's device\. | 
| model | The model identifier of the endpoint's device\. | 
| platform |  Information about the operating system on the endpoint's device\. For more information, see the [Platform](#event-streams-data-app-attributes-device-platform) table\.  | 

#### Locale<a name="event-streams-data-app-attributes-device-locale"></a>

Includes information about the language and region settings for the endpoint's device\.


| Attribute | Description | 
| --- | --- | 
| code | The locale identifier that's associated with the device\. | 
| country | The country or region that's associated with the device's locale\. | 
| language | The language that's associated with the device's locale\. | 

#### Platform<a name="event-streams-data-app-attributes-device-platform"></a>

Includes information about the operating system on the endpoint's device\.


| Attribute | Description | 
| --- | --- | 
| name | The name of the operating system on the device\. | 
| version | The version of the operating system on the device\. | 

### Session<a name="event-streams-data-app-attributes-session"></a>

Includes information about the session that generated the event\.


| Attribute | Description | 
| --- | --- | 
| session\_id | A unique ID that identifies the session\. | 
| start\_timestamp | The date and time when the session began, shown as Unix time in milliseconds\. | 
| stop\_timestamp | The date and time when the session ended, shown as Unix time in milliseconds\. | 
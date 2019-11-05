# Streaming Amazon Pinpoint Events to Kinesis<a name="analytics-streaming"></a>

The Kinesis platform offers services that you can use to load and analyze streaming data on AWS\. You can configure Amazon Pinpoint to send events to Amazon Kinesis Data Firehose or Amazon Kinesis Data Streams\. By streaming your events, you enable more flexible options for analysis and storage\. For more information, and for instructions on how to set up event streaming in the Amazon Pinpoint console, see [Streaming App and Campaign Events with Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/analytics-streaming.html) in the *Amazon Pinpoint User Guide*\.

## **Setting up Event Streaming**<a name="analytics-streaming-setup"></a>

The following examples demonstrate how to configure Amazon Pinpoint to automatically send the event data from an app to a Kinesis stream or Kinesis Data Firehose delivery stream\.

**Prerequisites**  
These examples require the following input:  
The app ID of an app that is integrated with Amazon Pinpoint and reporting events\. For information about how to integrate, see [Integrating Amazon Pinpoint with Your Application](integrate.md)\.
The ARN of a Kinesis stream or Kinesis Data Firehose delivery stream in your AWS account\. For information about creating these resources, see [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/amazon-kinesis-streams.html) in the *Amazon Kinesis Data Streams Developer Guide* or [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.
The ARN of an AWS Identity and Access Management \(IAM\) role that authorizes Amazon Pinpoint to send data to the stream\. For information about creating a role, see [IAM Role for Streaming Events to Kinesis](permissions-streams.md)\.

### AWS CLI<a name="analytics-streaming-setup-cli"></a>

The following AWS CLI example uses the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/put-event-stream.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/put-event-stream.html) command\. This command configures Amazon Pinpoint to send app and campaign events to an Kinesis stream:

```
aws pinpoint put-event-stream --application-id application-id --write-event-stream DestinationStreamArn=stream-arn,RoleArn=role-arn
```

### AWS SDK for Java<a name="analytics-streaming-setup-java"></a>

The following Java example configures Amazon Pinpoint to send app and campaign events to a Kinesis stream:

```
public PutEventStreamResult createEventStream(AmazonPinpoint pinClient, String appId,
                                                         String streamArn, String roleArn) {
    WriteEventStream stream = new WriteEventStream()
            .withDestinationStreamArn(streamArn)
            .withRoleArn(roleArn);

    PutEventStreamRequest request = new PutEventStreamRequest()
            .withApplicationId(appId)
            .withWriteEventStream(stream);

    return pinClient.putEventStream(request);
}
```

This example constructs a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/WriteEventStream.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/WriteEventStream.html) object that stores the ARNs of the Kinesis stream and the IAM role\. The `WriteEventStream` object is passed to a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/PutEventStreamRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/PutEventStreamRequest.html) object to configure Amazon Pinpoint to stream events for a specific app\. The `PutEventStreamRequest` object is passed to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#putEventStream-com.amazonaws.services.pinpoint.model.PutEventStreamRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#putEventStream-com.amazonaws.services.pinpoint.model.PutEventStreamRequest-) method of the Amazon Pinpoint client\.

You can assign a Kinesis stream to multiple apps\. Amazon Pinpoint will send event data from each app to the stream, enabling you to analyze the data as a collection\. The following example method accepts a list of app IDs, and it uses the previous example method, `createEventStream`, to assign a stream to each app:

```
public List<PutEventStreamResult> createEventStreamFromAppList(
        AmazonPinpoint pinClient, List<String> appIDs, String streamArn, String roleArn) {
    return appIDs.stream()
            .map(appId -> createEventStream(pinClient, appId, streamArn, roleArn))
            .collect(Collectors.toList());
}
```

With Amazon Pinpoint, you can assign one stream to multiple apps, but you cannot assign multiple streams to one app\.

## Disabling Event Streaming<a name="analytics-streaming-disable"></a>

If you assigned a Kinesis stream to an app, you can disable event streaming for that app\. Amazon Pinpoint stops streaming the events, but you can view analytics based on the events in the Amazon Pinpoint console\.

### AWS CLI<a name="analytics-streaming-disable-cli"></a>

Use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/delete-event-stream.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/delete-event-stream.html) command:

```
aws pinpoint delete-event-stream --application-id application-id
```

### AWS SDK for Java<a name="analytics-streaming-disable-java"></a>

Use the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEventStream-com.amazonaws.services.pinpoint.model.DeleteEventStreamRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEventStream-com.amazonaws.services.pinpoint.model.DeleteEventStreamRequest-) method of the Amazon Pinpoint client:

```
pinClient.deleteEventStream(new DeleteEventStreamRequest().withApplicationId(appId));
```

## Event Data<a name="analytics-streaming-data"></a>

After you set up event streaming, Amazon Pinpoint sends each event reported by your application, and each campaign event created by Amazon Pinpoint, as a JSON data object to your Kinesis stream\.

The event type is indicated by the `event_type` attribute in the event JSON object\.

### App Events<a name="analytics-streaming-data-app"></a>

After you integrate your app with Amazon Pinpoint and you run one or more campaigns, Amazon Pinpoint streams events about user activity and campaign message deliveries\.

#### Example<a name="analytics-streaming-data-app-example"></a>

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
    "session_id": "f549dea9-1090-945d-c3d1-e496780baac5",
    "start_timestamp": 1487973202531,
    "stop_timestamp": 1487973802507
  },
  "attributes": {},
  "metrics": {}
}
```

#### Standard App Event Types<a name="analytics-streaming-data-app-types"></a>

Amazon Pinpoint streams the following standard types for app events:
+ \_campaign\.send
+ \_monetization\.purchase
+ \_session\.start
+ \_session\.stop
+ \_session\.pause
+ \_session\.resume
+ \_userauth\.sign\_in
+ \_userauth\.sign\_up
+ \_userauth\.auth\_fail

### Email Events<a name="analytics-streaming-data-email"></a>

If the email channel is enabled, Amazon Pinpoint streams events about email deliveries, complaints, opens, and more\.

#### Example<a name="analytics-streaming-data-email-example"></a>

The JSON object for an email event contains the data shown in the following example\.

```
{
  "event_type": "_email.delivered",
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
    "session_id": "f549dea9-1090-945d-c3d1-e496780baac5",
    "start_timestamp": 1487973202531,
    "stop_timestamp": 1487973802507
  },
  "attributes": {},
  "metrics": {}
}
```

#### Standard Email Event Types<a name="analytics-streaming-data-email-types"></a>

Amazon Pinpoint streams the following standard types for the email channel:
+ \_email\.send
+ \_email\.delivered
+ \_email\.rejected
+ \_email\.hardbounce
+ \_email\.softbounce
+ \_email\.complaint
+ \_email\.open
+ \_email\.click
+ \_email\.unsubscribe

### SMS Events<a name="analytics-streaming-data-sms"></a>

If the SMS channel is enabled, Amazon Pinpoint streams events about SMS deliveries\.

#### Example<a name="analytics-streaming-data-sms-example"></a>

The JSON object for an SMS event contains the data shown in the following example\.

```
{
  "account_id": "123412341234",
  "event_type": "_SMS.SUCCESS",
  "arrival_timestamp": 2345678,
  "timestamp": 13425345,
  "timestamp_created": "1495756908285",
  "application_key": "AppKey-688037015201",
  "unique_id": "uniqueId-68803701520114087975969",
  "attributes": {
    "message_id": "12234sdv",
    "sender_request_id": "abdfg",
    "number_of_message_parts": 1,
    "record_status": "SUCCESS",
    "message_type": "Transactional",
    "keyword": "test",
    "mcc_mnc": "456123",
    "iso_country_code": "US"
  },
  "metrics": {
    "price_in_millicents_usd": 0.0
  },
  "facets": {},
  "additional_properties": {}
}
```

#### Standard SMS Event Types<a name="analytics-streaming-data-sms-types"></a>

Amazon Pinpoint streams the following standard types for the SMS channel:
+ \_sms\.send
+ \_sms\.success
+ \_sms\.fail
+ \_sms\.optout

### Event Attributes<a name="analytics-streaming-data-attributes"></a>

The JSON object for an event contains the following attributes\.


**Event**  

| Attribute | Description | 
| --- | --- | 
| event\_type | The type of event reported by your app\. | 
| event\_timestamp | The time at which the event is reported as Unix time in milliseconds\. If your app reports events using the AWS Mobile SDK for Android or the AWS Mobile SDK for iOS, the time stamp is automatically generated\. | 
| arrival\_timestamp | The time at which the event is received by Amazon Pinpoint as Unix time in milliseconds\. Does not apply to campaign events\. | 
| event\_version | The version of the event JSON schema\.Check this version in your event\-processing application so that you know when to update the application in response to a schema update\. | 
| application | Your Amazon Pinpoint app\. See the Application table\. | 
| client | The app client installed on the device that reports the event\. See the Client table\. | 
| device | The device that reports the event\. See the Device table\. | 
| session | The app session on the device\. Typically a session begins when a user opens your app\. | 
| attributes | Custom attributes that your app reports to Amazon Pinpoint as part of the event\. | 
| metrics | Custom metrics that your app reports to Amazon Pinpoint as part of the event\. | 


**Application**  

| Attribute | Description | 
| --- | --- | 
| app\_id | The unique ID of the Amazon Pinpoint app that reports the event\. | 
| cognito\_identity\_pool\_id | The unique ID of the Amazon Cognito identity pool used by your app\. | 
| package\_name | The name of your app package\. For example, com\.example\.my\_app\. | 
| sdk | The SDK used to report the event\. | 
| title | The title of your app\. | 
| version\_name | The customer\-facing app version, such as V2\.0\. | 
| version\_code | The internal code that represents your app version\. | 


**Client**  

| Attribute | Description | 
| --- | --- | 
| client\_id | The unique ID for the app client installed on the device\. This ID is automatically generated by the AWS Mobile SDK for iOS and the AWS Mobile SDK for Android\. | 
| cognito\_id | The unique ID assigned to the app client in the Amazon Cognito identity pool used by your app\. | 


**Device**  

| Attribute | Description | 
| --- | --- | 
| locale | The device locale\. | 
| make | The device make, such as Apple or Samsung\. | 
| model | The device model, such as iPhone\. | 
| platform | The device platform, such as ios or android\. | 


**Session**  

| Attribute | Description | 
| --- | --- | 
| session\_id | The unique ID for the app session\. | 
| start\_timestamp | The time at which the session starts as Unix time in milliseconds\. | 
| stop\_timestamp | The time at which the session stops as Unix time in milliseconds\. | 
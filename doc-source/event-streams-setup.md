# **Setting Up Event Streaming**<a name="event-streams-setup"></a>

You can set up Amazon Pinpoint to send data about campaigns and transactional messages to an Amazon Kinesis stream or an Amazon Kinesis Data Firehose delivery stream\.

This section includes information about setting up event streaming programmatically\. You can also use the Amazon Pinpoint console to set up event streaming\. For information about setting up event streaming by using the Amazon Pinpoint console, see [Event Stream Settings](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-event-streams.html) in the *Amazon Pinpoint User Guide*\.

## Prerequisites<a name="event-streams-setup-prerequisites"></a>

The examples in this section require the following input:
+ The app ID of an app that's integrated with Amazon Pinpoint and reporting events\. For information about how to integrate, see [Integrating Amazon Pinpoint with Your Application](integrate.md)\.
+ The Amazon Resource Name \(ARN\) of a Kinesis stream or Kinesis Data Firehose delivery stream in your AWS account\. For information about creating these resources, see [Creating and Updating Data Streams](https://docs.aws.amazon.com/streams/latest/dev/amazon-kinesis-streams.html) in the *Amazon Kinesis Data Streams Developer Guide* or [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.
+ The ARN of an AWS Identity and Access Management \(IAM\) role that authorizes Amazon Pinpoint to send data to the stream\. For information about creating a role, see [IAM Role for Streaming Events to Kinesis](permissions-streams.md)\.

## AWS CLI<a name="event-streams-setup-cli"></a>

The following AWS CLI example uses the [put\-event\-stream](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/put-event-stream.html) command\. This command configures Amazon Pinpoint to send app and campaign events to a Kinesis stream:

```
aws pinpoint put-event-stream \
--application-id projectId \
--write-event-stream DestinationStreamArn=streamArn,RoleArn=roleArn
```

## AWS SDK for Java<a name="event-streams-setup-java"></a>

The following Java example configures Amazon Pinpoint to send app and campaign events to a Kinesis stream:

```
public PutEventStreamResult createEventStream(AmazonPinpoint pinClient, 
        String appId, String streamArn, String roleArn) {
        
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

You can assign a Kinesis stream to multiple apps\. If you do this, Amazon Pinpoint sends event data from each app to the stream, which enables you to analyze the data as a collection\. The following example method accepts a list of app IDs, and it uses the previous example method, `createEventStream`, to assign a stream to each app:

```
public List<PutEventStreamResult> createEventStreamFromAppList(
        AmazonPinpoint pinClient, List<String> appIDs, 
        String streamArn, String roleArn) {

    return appIDs.stream()
            .map(appId -> createEventStream(pinClient, appId, streamArn, 
                    roleArn))
            .collect(Collectors.toList());
}
```

Although you can assign one stream to multiple apps, you can't assign multiple streams to one app\.

## Disabling Event Streaming<a name="event-streams-disable"></a>

If you assign a Kinesis stream to an app, you can disable event streaming for that app\. Amazon Pinpoint stops streaming the events to Kinesis, but you can view event analytics by using the Amazon Pinpoint console\.

### AWS CLI<a name="event-streams-disable-cli"></a>

Use the [delete\-event\-stream](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/delete-event-stream.html) command:

```
aws pinpoint delete-event-stream --application-id application-id
```

### AWS SDK for Java<a name="event-streams-disable-java"></a>

Use the [deleteEventStream](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEventStream-com.amazonaws.services.pinpoint.model.DeleteEventStreamRequest-) method of the Amazon Pinpoint client:

```
pinClient.deleteEventStream(new DeleteEventStreamRequest().withApplicationId(appId));
```
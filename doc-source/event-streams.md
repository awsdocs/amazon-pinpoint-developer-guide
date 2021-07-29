# Streaming Amazon Pinpoint events to Kinesis<a name="event-streams"></a>

In Amazon Pinpoint, an *event* is an action that occurs when a user interacts with one of your applications, when you send a message from a campaign or journey, or when you send a transactional SMS or email message\. For example, if you send an email message, several events occur:
+ When you send the message, a *send* event occurs\.
+ When the message reaches the recipient's inbox, a *delivered* event occurs\.
+ When the recipient opens the message, an *open* event occurs\. 

You can configure Amazon Pinpoint to send information about events to Amazon Kinesis\. The Kinesis platform offers services that you can use to collect, process, and analyze data from AWS services in real time\. Amazon Pinpoint can send event data to Kinesis Data Firehose, which streams this data to AWS data stores such as Amazon S3 or Amazon Redshift\. Amazon Pinpoint can also stream data to Kinesis Data Streams, which ingests and stores multiple data streams for processing by analytics applications\.

The Amazon Pinpoint event stream includes information about user interactions with applications \(apps\) that you connect to Amazon Pinpoint\. It also includes information about all the messages that you send from campaigns, through any channel, and from journeys\. This can also include any custom events that you've defined\. Finally, it includes information about all the transactional email and SMS messages that you send\.

**Note**  
Amazon Pinpoint doesn't stream information about transactional push notifications or voice messages\.

This chapter provides information about setting up Amazon Pinpoint to stream event data to Kinesis\. It also contains examples of the event data that Amazon Pinpoint streams\.

**Topics**
+ [**Setting up event streaming**](event-streams-setup.md)
+ [App events](event-streams-data-app.md)
+ [Campaign events](event-streams-data-campaign.md)
+ [Journey events](event-streams-data-journey.md)
+ [Email events](event-streams-data-email.md)
+ [SMS events](event-streams-data-sms.md)
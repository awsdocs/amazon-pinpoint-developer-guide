# Streaming Amazon Pinpoint Events to Kinesis<a name="event-streams"></a>

In Amazon Pinpoint, an *event* is an action that occurs when a user interacts with one of your apps, or when you send a campaign or a transactional message\. For example, when you send an email, several events occur: when you send the message, a *send* event occurs; when the message reaches the recipient's inbox, a *delivered* event occurs; when the recipient opens the email, an *open* event occurs, and so on\.

You can configure Amazon Pinpoint to send information about events to Amazon Kinesis\. Kinesis is an AWS service that can collect, process, and analyze data from other AWS services in real\-time\. Amazon Pinpoint can send event data to Kinesis Data Firehose, which streams this data to AWS data stores such as Amazon S3 or Amazon Redshift\. Amazon Pinpoint can also stream data to Kinesis Data Streams, which ingests and stores multiple data streams for processing in analytics applications\.

The Amazon Pinpoint event stream includes information about user interactions with your applications that are connected to Amazon Pinpoint\. It also streams information about all of the campaigns that you send across all channels\. Finally, it includes information about the transactional emails and SMS messages that you send\.

**Note**  
Amazon Pinpoint doesn't stream information about transactional push notifications or voice messages\.

This section includes information about setting up Amazon Pinpoint to stream event data\. It also contains examples of the event data that Amazon Pinpoint streams\.

**Topics**
+ [**Setting Up Event Streaming**](event-streams-setup.md)
+ [App Events](event-streams-data-app.md)
+ [Campaign Events](event-streams-data-campaign.md)
+ [Email Events](event-streams-data-email.md)
+ [SMS Events](event-streams-data-sms.md)
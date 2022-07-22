# Using the Amazon Pinpoint SMS and Voice API, version 2<a name="sms-voice-v2"></a>

Amazon Pinpoint includes an API \(called the SMS and Voice API, version 2\) that was designed for sending SMS and voice messages\. While the Amazon Pinpoint API is focused on sending messages through scheduled and event\-driven campaigns and journeys, the SMS and Voice API provides new features and capabilities for sending SMS and voice messages directly to individual recipients\. You can use SMS and Voice API independently of the Amazon Pinpoint campaign and journey features, or you can use both at the same time to accomodate different use cases\. If you already use Amazon Pinpoint to send SMS or voice messages, your account is already configured to use this API\.

This API is a good solution for users who have a multi\-tenant architecture, such as Independent Software Vendors \(ISVs\)\. This API makes it easier to ensure that event data, origination phone numbers, and opt\-out lists are separated for different tenants\.

When you use the SMS and Voice API, we recommend that you set up configuration sets and event destinations\. The SMS and Voice API doesn't automatically emit event data for the messages that you send\. Setting up event destinations ensures that you capture important event data, such as message delivery and failure events\.

Version 2 of this API was preceded by Version 1\. If you currenly use Version 1 of this API, it will continue to be available, and you can continue to use it\. However, if you migrate to Version 2, you will gain additional features, such as the ability to create pools of phone numbers, request new phone numbers programmatically, and enable or disable certain capabilities of phone numbers\.

**Note**  
There are some tasks that currently can only be completed by using the Amazon Pinpoint console\. For example, if you want to [verify a phone number to use while your account is in the SMS sandbox](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-sandbox.html#channels-sms-verify-number), or if you want to [register to use 10DLC](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-10dlc.html), you must use the Amazon Pinpoint console\.

This section provides information about this API, and includes examples of how to use it\. You can also find reference documentation in the [SMS and Voice, version 2 API Reference](https://docs.aws.amazon.com/pinpoint/latest/apireference_smsvoicev2/Welcome.html)\.

**Topics**
+ [Concepts in the SMS and Voice API, version 2](#sms-voice-v2-concepts)
+ [Managing phone numbers](sms-voice-v2-phone-numbers.md)
+ [Managing sender IDs](sms-voice-v2-sender-ids.md)
+ [Managing pools](sms-voice-v2-pools.md)
+ [Managing opt\-out lists](sms-voice-v2-opt-out-lists.md)
+ [Managing configuration sets](sms-voice-v2-configuration-sets.md)
+ [Managing event destinations](sms-voice-v2-event-destinations.md)
+ [Sending messages using the SMS and Voice API](sms-voice-v2-messages.md)

## Concepts in the SMS and Voice API, version 2<a name="sms-voice-v2-concepts"></a>

The SMS and Voice API, version 2, includes several new concepts related to sending messages\. This section provides additional information about these new terms\.

**Origination Identity**  
A phone number \(or sender ID, for SMS messages\) that SMS or voice messages are sent from\.  
For more information about managing origination identities, see [Managing phone numbers](sms-voice-v2-phone-numbers.md)\.

**Destination phone number**  
A phone number that an SMS or voice message is sent to\.

**Pool**  
An object that contains several origination identities that are used for the same purpose\. A pool can have an opt\-out list associated with it\.  
For more information about managing pools, see [Managing pools](sms-voice-v2-pools.md)\.

**Configuration set**  
Configuration sets are sets of rules that are applied when you send a message\. For example, a configuration set can specify a destination for events related to a message\. When SMS events occur \(such as delivery or failure events\), they are routed to the destination associated with the configuration set that you specified when you sent the message\.  
For more information about managing configuration sets, see [Managing configuration sets](sms-voice-v2-configuration-sets.md)\.

**Event destination**  
An event destination is a location \(such as a Amazon CloudWatch Logs Group, a Amazon Kinesis Data Firehose stream, or an Amazon Simple Notification Service topic\) that SMS and voice events are sent to\. To use event destinations, you first create the destination, and then associate it with a configuration set\. When you send a message, your call to the API can include a reference to a configuration set\.  
For more information about managing event destinations, see [Managing event destinations](sms-voice-v2-event-destinations.md)\.

**Opt\-out list**  
A list of destination identities that should not have messages sent to them\. Destination identities are automatically added to the opt\-out list if they reply to your origination number with the keyword STOP\. If you attempt to send a message to a destination number that is on an opt\-out list, and the opt\-out list is associated with the pool used to send the message, Amazon Pinpoint doesn't attempt to send the message\.  
If you enable the self\-managed opt\-out feature for a phone number, then your recipients aren't automatically opted out when they reply to your messages with the keyword STOP\.
For more information about managing opt\-out lists, see [Managing opt\-out lists](sms-voice-v2-opt-out-lists.md)\.
# Event types<a name="sms-voice-v2-event-destinations-types"></a>

The easiest way to use event destinations is to send all SMS and voice events to a single destination\. However, you can configure event destinations so that specific types of events are sent to different destinations\. For example, you could send all delivery\-related events to an Amazon S3 bucket for storage, and all failure events to an Amazon SNS topic so that you can be notified when they occur\. You can also send SMS events and voice events to different locations\.

You can configure event destinations to send the following types of events:
+ **ALL** – Sends all SMS and voice events to the specified destination\.
+ **TEXT\_ALL** – Sends all SMS events to the specified destination\.
+ **VOICE\_ALL** – Sends all voice events to the specified destination\.
+ **TEXT\_SENT** – Sends events to the specified destination each time an SMS message is sent\.
+ **TEXT\_DELIVERED** – Sends all SMS delivery events to the specified destination\.
+ **TEXT\_SUCCESSFUL** – Sends all SMS success events to the specified destination\. Success events occur when the message is accepted by the recipient's carrier\.
+ **TEXT\_QUEUED** – Sends all SMS queued events to the specified destination\. Queued events occur when the message is queued for delivery, but not delivered yet\.
+ **TEXT\_PENDING** – Sends all SMS pending events to the specified destination\. Pending events occur when a message is in the process of being delivered, but hasn't been delivered \(or failed to be delivered\) yet\.
+ **TEXT\_BLOCKED** – Sends all SMS blocked events to the specified destination\. Blocked events occur when the recipient's device or carrier is blocking messages to that recipient\.
+ **TEXT\_TTL\_EXPIRED** – Sends all SMS TTL Expired events to the specified destination\. TTL Expired events occur when the time required to deliver the message exceeds the `TTL` value that you specified when you sent the message\.
+ **TEXT\_CARRIER\_UNREACHABLE** – Sends all Carrier Unreachable events for SMS messages to the specified destination\. Carrier Unreachable events occur when a transient error occurs on the carrier network of the message recipient\.
+ **TEXT\_INVALID** – Sends all SMS invalid events to the specified destination\. Invalid events occur when the destination phone number is not valid\.
+ **TEXT\_INVALID\_MESSAGE** – Sends all invalid message events for SMS messages to the specified destination\. Invalid message events occur when the body of the SMS message is invalid and can't be delivered\.
+ **TEXT\_CARRIER\_BLOCKED** – Sends all carrier blocked events for SMS messages to the specified destination\. Carrier blocked events occur when the recipient's carrier blocks the delivery of the message\. This typically occurs when the carrier identifies the message as malicious \(for example, if the message contains information related to a phishing scam\) or abusive \(for example, if the message is suspected of being unsolicited or prohibited content\)\.
+ **TEXT\_UNREACHABLE** – Sends all unreachable events for SMS messages to the specified destination\. Unreachable events occur when the recipient's device is unavailable\. This might occur if the device is not connected to a mobile network, or is powered off\.
+ **TEXT\_SPAM** – Sends all spam events for SMS messages to the specified destination\. Spam events occur when the recipient's carrier identifies the message as containing unsolicited commercial content and blocks the delivery of the message\.
+ **TEXT\_UNKNOWN** – Sends all unknown SMS events to the specified destination\. Unknown events occur when a message fails to be delivered for a reason that isn't covered by one of the other event types\. Unknown errors might be transient or permanent\.
+ **VOICE\_COMPLETED** – Sends all completed events for voice messages to the specified destination\. Completed events occur when the audio message is played to the recipient\. This status doesn't necessarily mean that the message was delivered to a human recipient\. For example, it could indicate that the message was delivered to a voicemail system\.
+ **VOICE\_ANSWERED** – Sends all answered events for voice messages to the specified destination\. Answered events occur when the recipient answers the phone\. 
+ **VOICE\_INITIATED** – Sends events to the specified destination each time a voice message is initiated\.
+ **VOICE\_TTL\_EXPIRED** – Sends all voice TTL Expired events to the specified destination\. TTL Expired events occur when the time required to deliver the message exceeds the `TTL` value that you specified when you sent the message\.
+ **VOICE\_BUSY** – Sends all busy events for voice messages to the specified destination\. Busy events occur when the recipient's phone line is busy\.
+ **VOICE\_NO\_ANSWER** – Sends all no answer events for voice messages to the specified destination\. No answer events occur after the call has been placed, but the recipient \(or their voicemail system\) never answer\.
+ **VOICE\_RINGING** – Sends all ringing events for voice messages to the specified destination\. Ringing events occur after the call has been placed, but before the recipient answers\.
+ **VOICE\_FAILED** – Sends all voice message failure events to the specified destination\. Failure events occur when the message fails to be delivered\.
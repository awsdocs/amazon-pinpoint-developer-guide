# Integrating Amazon Pinpoint with your application<a name="integrate"></a>

Integrate Amazon Pinpoint with your client code to understand and engage your users\.

After you integrate, as users launch your application, it connects to the Amazon Pinpoint service to add or update *endpoints*\. Endpoints represent the destinations that you can message—such as user devices, email addresses, or phone numbers\.

Also, your application provides usage data, or *events*\. View event data in the Amazon Pinpoint console to learn how many users you have, how often they use your application, when they use it, and more\. 

After your application supplies endpoints and events, you can use this information to tailor messaging campaigns for specific audiences, or *segments*\. \(You can also directly message simple lists of recipients without creating campaigns\.\)

Use the topics in this section to integrate Amazon Pinpoint with a mobile or web client\. These topics include code examples and procedures to integrate with an Android, iOS, or JavaScript application\.

Outside of your client, you can use [supported AWS SDKs](integrate-supported-sdks.md) or the [Amazon Pinpoint API](https://docs.aws.amazon.com/pinpoint/latest/apireference/) to import endpoints, export event data, define customer segments, create and run campaigns, and more\.

To start integrating your apps, see [Integrating the AWS mobile SDKs or JavaScript library with your application](integrate-sdk.md)\.

**Topics**
+ [AWS SDK support for Amazon Pinpoint](integrate-supported-sdks.md)
+ [Integrating the AWS mobile SDKs or JavaScript library with your application](integrate-sdk.md)
+ [Registering endpoints in your application](integrate-endpoints.md)
+ [Reporting events in your application](integrate-events.md)
+ [Handling push notifications](integrate-push.md)
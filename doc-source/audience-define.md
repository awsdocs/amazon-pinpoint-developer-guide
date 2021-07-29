# Defining your audience to Amazon Pinpoint<a name="audience-define"></a>

In Amazon Pinpoint, each member of your audience is represented by one or more endpoints\. When you use Amazon Pinpoint to send a message, you direct the message to the endpoints that represent the members of your target audience\. Each endpoint definition includes a message destination—such as a device token, email address, or phone number\. It also includes data about your users and their devices\. Before you analyze, segment, or engage your audience, the first step is to add endpoints to your Amazon Pinpoint project\. 

To add endpoints, you can:
+ Integrate Amazon Pinpoint with your Android, iOS, or JavaScript client so that endpoints are added automatically when users visit your application\.
+ Use the Amazon Pinpoint API to add endpoints individually or in batches\.
+ Import endpoint definitions that are stored outside of Amazon Pinpoint\.

After you add endpoints, you can:
+ View analytics about your audience in the Amazon Pinpoint console\.
+ Learn about your audience by looking up or exporting endpoint data\.
+ Define audience segments based on endpoint attributes, such as demographic data or user interests\.
+ Engage your target audiences with tailored messaging campaigns\.
+ Send messages directly to lists of endpoints\.

Use the topics in this section to add, update, and delete endpoints by using the Amazon Pinpoint API\. If you want to add endpoints automatically from your Android, iOS, or JavaScript client, see [Registering endpoints in your application](integrate-endpoints.md) instead\.

**Topics**
+ [Adding endpoints](audience-define-endpoints.md)
+ [Associating users with endpoints](audience-define-user.md)
+ [Adding a batch of endpoints](audience-define-endpoints-batch.md)
+ [Importing endpoints](audience-define-import.md)
+ [Deleting endpoints](audience-define-remove.md)
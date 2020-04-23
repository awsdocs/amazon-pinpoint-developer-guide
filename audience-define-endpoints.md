# Adding Endpoints to Amazon Pinpoint<a name="audience-define-endpoints"></a>

An *endpoint* represents a destination that you can message—such as a mobile device, phone number, or email address\. Before you can message a member of your audience, you must define one or more endpoints for that individual\. 

When you define an endpoint, you specify the *channel* and *address*\. The channel is the type of platform that you use to message the endpoint\. Examples of channels include a push notification service, SMS, or email\. The address specifies where to message the endpoint, such as a device token, phone number, or email address\.

To add more details about your audience, you can enrich your endpoints with custom and standard attributes\. These attributes might include data about your users, their preferences, their devices, the versions of the client that they use, or their locations\. When you add this type of data to your endpoints, you're able to:
+ View charts about your audience in the Amazon Pinpoint console\.
+ Segment your audience based on endpoint attributes so that you can send your messages to the right target audience\.
+ Personalize your messages by incorporating message variables that are substituted with endpoint attribute values\.

A mobile or JavaScript client application registers endpoints automatically if you integrate Amazon Pinpoint by using the AWS Mobile SDKs or the AWS Amplify JavaScript library\. The client registers an endpoint for each new user, and it updates endpoints for returning users\. To register endpoints from a mobile or JavaScript client, see [Registering Endpoints in Your Application](integrate-endpoints.md)\.

## Examples<a name="audience-define-endpoints-examples"></a>

The following examples show you how to add an endpoint to an Amazon Pinpoint project\. The endpoint represents an audience member who lives in Seattle and uses an iPhone\. This person can be messaged through the Apple Push Notification service \(APNs\)\. The endpoint's address is the device token that's provided by APNs\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Update Endpoint Command**  
To add or update an endpoint, use the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoint.html) command:  

```
$ aws pinpoint update-endpoint \
> --application-id application-id \
> --endpoint-id endpoint-id \
> --endpoint-request file://endpoint-request-file.json
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating an endpoint\.
+ *example\-endpoint* is the ID that you're assigning to a new endpoint, or it's the ID of an existing endpoint that you're updating\.
+ *endpoint\-request\-file\.json* is the file path to a local JSON file that contains the input for the `--endpoint-request` parameter\.

**Example Endpoint Request File**  
The example `update-endpoint` command uses a JSON file as the argument for the `--endpoint-request` parameter\. This file contains an endpoint definition like the following:  

```
{
  "ChannelType": "APNS",
  "Address": "1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7a8b9c0d1e2f",
  "Attributes": {
    "Interests": [
      "Technology",
      "Music",
      "Travel"
    ]
  },
  "Metrics": {
    "technology_interest_level": 9.0,
    "music_interest_level": 6.0,
    "travel_interest_level": 4.0
  },
  "Demographic": {
    "AppVersion": "1.0",
    "Make": "apple",
    "Model": "iPhone",
    "ModelVersion": "8",
    "Platform": "ios",
    "PlatformVersion": "11.3.1",
    "Timezone": "America/Los_Angeles"
  },
  "Location": {
    "Country": "US",
    "City": "Seattle",
    "PostalCode": "98121",
    "Latitude": 47.61,
    "Longitude": -122.33
  }
}
```
For the attributes that you can use to define an endpoint, see the [EndpointRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#apps-application-id-endpoints-endpoint-id-schemas) schema in the *Amazon Pinpoint API Reference*\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To add an endpoint, initialize an [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointRequest.html) object, and pass it to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpoint-com.amazonaws.services.pinpoint.model.UpdateEndpointRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpoint-com.amazonaws.services.pinpoint.model.UpdateEndpointRequest-) method of the `AmazonPinpoint` client:  

```
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.*;
import java.util.Arrays;

public class AddExampleEndpoint {

	public static void main(String[] args) {
	
		final String USAGE = "\n" +
			"AddExampleEndpoint - Adds an example endpoint to an Amazon Pinpoint application." +
			"Usage: AddExampleEndpoint <applicationId>" +
			"Where:\n" +
			"  applicationId - The ID of the Amazon Pinpoint application to add the example " +
			"endpoint to.";
		
		if (args.length < 1) {
		    System.out.println(USAGE);
		    System.exit(1);
		}
		
		String applicationId = args[0];
		
		// The device token assigned to the user's device by Apple Push Notification service (APNs).
		String deviceToken = "1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7a8b9c0d1e2f";
		
		// Initializes an endpoint definition with channel type and address.
		EndpointRequest wangXiulansIphoneEndpoint = new EndpointRequest()
			.withChannelType(ChannelType.APNS)
			.withAddress(deviceToken);
		
		// Adds custom attributes to the endpoint.
		wangXiulansIphoneEndpoint.addAttributesEntry("interests", Arrays.asList(
			"technology",
			"music",
			"travel"));
		
		// Adds custom metrics to the endpoint.
		wangXiulansIphoneEndpoint.addMetricsEntry("technology_interest_level", 9.0);
		wangXiulansIphoneEndpoint.addMetricsEntry("music_interest_level", 6.0);
		wangXiulansIphoneEndpoint.addMetricsEntry("travel_interest_level", 4.0);
		
		// Adds standard demographic attributes.
		wangXiulansIphoneEndpoint.setDemographic(new EndpointDemographic()
			.withAppVersion("1.0")
			.withMake("apple")
			.withModel("iPhone")
			.withModelVersion("8")
			.withPlatform("ios")
			.withPlatformVersion("11.3.1")
			.withTimezone("America/Los_Angeles"));
		
		// Adds standard location attributes.
		wangXiulansIphoneEndpoint.setLocation(new EndpointLocation()
			.withCountry("US")
			.withCity("Seattle")
			.withPostalCode("98121")
			.withLatitude(47.61)
			.withLongitude(-122.33));
		
		// Initializes the Amazon Pinpoint client.
		AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
			.withRegion(Regions.US_EAST_1).build();
		
		// Updates or creates the endpoint with Amazon Pinpoint.
		UpdateEndpointResult result = pinpointClient.updateEndpoint(new UpdateEndpointRequest()
			.withApplicationId(applicationId)
			.withEndpointId("example_endpoint")
			.withEndpointRequest(wangXiulansIphoneEndpoint));
		
		System.out.format("Update endpoint result: %s\n", result.getMessageBody().getMessage());
	
	}
}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example PUT Endpoint Request**  
To add an endpoint, issue a `PUT` request to the [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) resource at the following URI:  
`/v1/apps/application-id/endpoints/endpoint-id`  
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating an endpoint\.
+ *endpoint\-id* is the ID that you're assigning to a new endpoint, or it's the ID of an existing endpoint that you're updating\.
In your request, include the required headers, and provide the [EndpointRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#apps-application-id-endpoints-endpoint-id-schemas) JSON as the body:  

```
PUT /v1/apps/application_id/endpoints/example_endpoint HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180415T182538Z
Content-Type: application/json
Accept: application/json
X-Amz-Date: 20180428T004705Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180428/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;content-length;content-type;host;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{
  "ChannelType": "APNS",
  "Address": "1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7a8b9c0d1e2f",
  "Attributes": {
    "Interests": [
      "Technology",
      "Music",
      "Travel"
    ]
  },
  "Metrics": {
    "technology_interest_level": 9.0,
    "music_interest_level": 6.0,
    "travel_interest_level": 4.0
  },
  "Demographic": {
    "AppVersion": "1.0",
    "Make": "apple",
    "Model": "iPhone",
    "ModelVersion": "8",
    "Platform": "ios",
    "PlatformVersion": "11.3.1",
    "Timezone": "America/Los_Angeles"
  },
  "Location": {
    "Country": "US",
    "City": "Seattle",
    "PostalCode": "98121",
    "Latitude": 47.61,
    "Longitude": -122.33
  }
}
```
If your request succeeds, you receive a response like the following:  

```
{
    "RequestID": "67e572ed-41d5-11e8-9dc5-db288f3cbb72",
    "Message": "Accepted"
}
```

------

## Related Information<a name="audience-define-endpoints-related"></a>

For more information about the Endpoint resource in the Amazon Pinpoint API, including supported HTTP methods and request parameters, see [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) in the *Amazon Pinpoint API Reference\.*

For more information about personalizing messages with variables, see [Message Variables](https://docs.aws.amazon.com/pinpoint/latest/userguide/campaigns-message.html#campaigns-message-variables.html) in the *Amazon Pinpoint User Guide*\.

For information about the quotas that apply to endpoints, such as the number of attributes that you can assign, see [Endpoint Quotas](quotas.md#quotas-endpoint)\.
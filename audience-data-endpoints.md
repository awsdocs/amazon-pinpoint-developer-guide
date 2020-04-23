# Looking Up Endpoints with Amazon Pinpoint<a name="audience-data-endpoints"></a>

You can look up the details for any individual endpoint that was added to an Amazon Pinpoint project\. These details can include the destination address for your messages, the messaging channel, data about the user's device, data about the user's location, and any custom attributes that you record in your endpoints\.

To look up an endpoint, you need the endpoint ID\. If you don't know the ID, you can get the endpoint data by exporting instead\. To export endpoints, see [Exporting Endpoints from Amazon Pinpoint](audience-data-export.md)\.

## Examples<a name="audience-data-endpoints-examples"></a>

The following examples show you how to look up an individual endpoint by specifying its ID\. 

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Get Endpoint Command**  
To look up an endpoint, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-endpoint.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-endpoint.html) command:  

```
$ aws pinpoint get-endpoint \
> --application-id application-id \
> --endpoint-id endpoint-id
```
Where:  
+ `application-id` is the ID of the Amazon Pinpoint project that contains the endpoint\.
+ `endpoint-id` is the ID of the endpoint that you're looking up\.
The response to this command is the JSON definition of the endpoint, as in the following example:  

```
{
    "EndpointResponse": {
        "Address": "1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7a8b9c0d1e2f",
        "ApplicationId": "application-id",
        "Attributes": {
            "Interests": [
                "Technology",
                "Music",
                "Travel"
            ]
        },
        "ChannelType": "APNS",
        "CohortId": "63",
        "CreationDate": "2018-05-01T17:31:01.046Z",
        "Demographic": {
            "AppVersion": "1.0",
            "Make": "apple",
            "Model": "iPhone",
            "ModelVersion": "8",
            "Platform": "ios",
            "PlatformVersion": "11.3.1",
            "Timezone": "America/Los_Angeles"
        },
        "EffectiveDate": "2018-05-07T19:03:29.963Z",
        "EndpointStatus": "ACTIVE",
        "Id": "example_endpoint",
        "Location": {
            "City": "Seattle",
            "Country": "US",
            "Latitude": 47.6,
            "Longitude": -122.3,
            "PostalCode": "98121"
        },
        "Metrics": {
            "music_interest_level": 6.0,
            "travel_interest_level": 4.0,
            "technology_interest_level": 9.0
        },
        "OptOut": "ALL",
        "RequestId": "7f546cac-6858-11e8-adcd-2b5a07aab338",
        "User": {
            "UserAttributes": {
                "Gender": "Female",
                "FirstName": "Wang",
                "LastName": "Xiulan",
                "Age": "39"
            },
            "UserId": "example_user"
        }
    }
}
```

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To look up an endpoint, initialize a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/GetEndpointRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/GetEndpointRequest.html) object\. Then, pass this object to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#getEndpoint-com.amazonaws.services.pinpoint.model.GetEndpointRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#getEndpoint-com.amazonaws.services.pinpoint.model.GetEndpointRequest-) method of the `AmazonPinpoint` client:  

```
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.EndpointResponse;
import com.amazonaws.services.pinpoint.model.GetEndpointRequest;
import com.amazonaws.services.pinpoint.model.GetEndpointResult;
import com.google.gson.FieldNamingPolicy;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class LookUpEndpoint {

    public static void main(String[] args) {

        final String USAGE = "\n" +
                "LookUpEndpoint - Prints the definition of the endpoint that has the specified ID." +
                "Usage: LookUpEndpoint <applicationId> <endpointId>\n\n" +

                "Where:\n" +
                "  applicationId - The ID of the Amazon Pinpoint application that has the " +
                "endpoint." +
                "  endpointId - The ID of the endpoint ";

        if (args.length < 1) {
            System.out.println(USAGE);
            System.exit(1);
        }

        String applicationId = args[0];
        String endpointId = args[1];

        // Specifies the endpoint that the Amazon Pinpoint client looks up.
        GetEndpointRequest request = new GetEndpointRequest()
                .withEndpointId(endpointId)
                .withApplicationId(applicationId);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        // Uses the Amazon Pinpoint client to get the endpoint definition.
        GetEndpointResult result = pinpointClient.getEndpoint(request);
        EndpointResponse endpoint = result.getEndpointResponse();

        // Uses the Google Gson library to pretty print the endpoint JSON.
        Gson gson = new GsonBuilder()
                .setFieldNamingPolicy(FieldNamingPolicy.UPPER_CAMEL_CASE)
                .setPrettyPrinting()
                .create();
        String endpointJson = gson.toJson(endpoint);

        System.out.println(endpointJson);
    }
}
```
To print the endpoint data in a readable format, this example uses the Google GSON library to convert the `EndpointResponse` object to a JSON string\.

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example GET Endpoint Request**  
To look up an endpoint, issue a `GET` request to the [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html) resource:  

```
GET /v1/apps/application_id/endpoints/endpoint_id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache
```
Where:  
+ `application-id` is the ID of the Amazon Pinpoint project that contains the endpoint\.
+ `endpoint-id` is the ID of the endpoint that you're looking up\.
The response to this request is the JSON definition of the endpoint, as in the following example:  

```
{
    "ChannelType": "APNS",
    "Address": "1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4x5y6z7a8b9c0d1e2f",
    "EndpointStatus": "ACTIVE",
    "OptOut": "NONE",
    "RequestId": "b720cfa8-6924-11e8-aeda-0b22e0b0fa59",
    "Location": {
        "Latitude": 47.6,
        "Longitude": -122.3,
        "PostalCode": "98121",
        "City": "Seattle",
        "Country": "US"
    },
    "Demographic": {
        "Make": "apple",
        "Model": "iPhone",
        "ModelVersion": "8",
        "Timezone": "America/Los_Angeles",
        "AppVersion": "1.0",
        "Platform": "ios",
        "PlatformVersion": "11.3.1"
    },
    "EffectiveDate": "2018-06-06T00:58:19.865Z",
    "Attributes": {
        "Interests": [
            "Technology",
            "Music",
            "Travel"
        ]
    },
    "Metrics": {
        "music_interest_level": 6,
        "travel_interest_level": 4,
        "technology_interest_level": 9
    },
    "User": {},
    "ApplicationId": "application_id",
    "Id": "example_endpoint",
    "CohortId": "39",
    "CreationDate": "2018-06-06T00:58:19.865Z"
}
```

------

## Related Information<a name="audience-data-endpoints-related"></a>

For more information about the Endpoint resource in the Amazon Pinpoint API, see [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html) in the *Amazon Pinpoint API Reference\.*
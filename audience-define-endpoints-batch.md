# Adding a Batch of Endpoints to Amazon Pinpoint<a name="audience-define-endpoints-batch"></a>

You can add or update multiple endpoints in a single operation by providing the endpoints in batches\. Each batch request can include up to 100 endpoint definitions\.

If you want to add or update more than 100 endpoints in a single operation, see [Importing Endpoints into Amazon Pinpoint](audience-define-import.md) instead\.

## Examples<a name="audience-define-endpoints-batch-examples"></a>

The following examples show you how to add two endpoints at once by including the endpoints in a batch request\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Update Endpoints Batch Command**  
To submit an endpoint batch request, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoints-batch.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoints-batch.html) command:  

```
$ aws pinpoint update-endpoints-batch \
> --application-id application-id \
> --endpoint-batch-request file://endpoint_batch_request_file.json
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating the endpoints\.
+ *endpoint\_batch\_request\_file\.json* is the file path to a local JSON file that contains the input for the `--endpoint-batch-request` parameter\.

**Example Endpoint Batch Request File**  
The example `update-endpoints-batch` command uses a JSON file as the argument for the `--endpoint-request` parameter\. This file contains a batch of endpoint definitions like the following:  

```
{
    "Item": [
        {
            "ChannelType": "EMAIL",
            "Address": "richard_roe@example.com",
            "Attributes": {
                "Interests": [
                    "Music",
                    "Books"
                ]
            },
            "Metrics": {
                "music_interest_level": 3.0,
                "books_interest_level": 7.0
            },
            "Id": "example_endpoint_1",
            "User":{
                "UserId": "example_user_1",
                "UserAttributes": {
                    "FirstName": "Richard",
                    "LastName": "Roe"
                }
            }
        },
        {
            "ChannelType": "SMS",
            "Address": "+16145550100",
            "Attributes": {
                "Interests": [
                    "Cooking",
                    "Politics",
                    "Finance"
                ]
            },
            "Metrics": {
                "cooking_interest_level": 5.0,
                "politics_interest_level": 8.0,
                "finance_interest_level": 4.0
            },
            "Id": "example_endpoint_2",
            "User": {
                "UserId": "example_user_2",
                "UserAttributes": {
                    "FirstName": "Mary",
                    "LastName": "Major"
                }
            }
        }
    ]
}
```
For the attributes that you can use to define a batch of endpoints, see the [EndpointBatchRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints.html#apps-application-id-endpoints-schemas) schema in the *Amazon Pinpoint API Reference*\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To submit an endpoint batch request, initialize an [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointRequest.html) object, and pass it to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpointsBatch-com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpointsBatch-com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchRequest-) method of the `AmazonPinpoint` client\. The following example populates an `EndpointBatchRequest` object with two `EndpointBatchItem` objects:  

```
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.ChannelType;
import com.amazonaws.services.pinpoint.model.EndpointBatchItem;
import com.amazonaws.services.pinpoint.model.EndpointBatchRequest;
import com.amazonaws.services.pinpoint.model.EndpointUser;
import com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchRequest;
import com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchResult;

import java.util.Arrays;

public class AddExampleEndpoints {

    public static void main(String[] args) {

        final String USAGE = "\n" +
                "AddExampleEndpoints - Adds example endpoints to an Amazon Pinpoint application." +
                "Usage: AddExampleEndpoints <applicationId>" +
                "Where:\n" +
                "  applicationId - The ID of the Amazon Pinpoint application to add the example endpoints to.";

        if (args.length < 1) {
            System.out.println(USAGE);
            System.exit(1);
        }


        String applicationId = args[0];

        // Initializes an endpoint definition with channel type, address, and ID.
        EndpointBatchItem richardRoesEmailEndpoint = new EndpointBatchItem()
                .withChannelType(ChannelType.EMAIL)
                .withAddress("richard_roe@example.com")
                .withId("example_endpoint_1");

        // Adds custom attributes to the endpoint.
        richardRoesEmailEndpoint.addAttributesEntry("interests", Arrays.asList(
                "music",
                "books"));

        // Adds custom metrics to the endpoint.
        richardRoesEmailEndpoint.addMetricsEntry("music_interest_level", 3.0);
        richardRoesEmailEndpoint.addMetricsEntry("books_interest_level", 7.0);

        // Initializes a user definition with a user ID.
        EndpointUser richardRoe = new EndpointUser().withUserId("example_user_1");

        // Adds custom user attributes.
        richardRoe.addUserAttributesEntry("name", Arrays.asList("Richard", "Roe"));

        // Adds the user definition to the endpoint.
        richardRoesEmailEndpoint.setUser(richardRoe);

        // Initializes an endpoint definition with channel type, address, and ID.
        EndpointBatchItem maryMajorsSmsEndpoint = new EndpointBatchItem()
                .withChannelType(ChannelType.SMS)
                .withAddress("+16145550100")
                .withId("example_endpoint_2");

        // Adds custom attributes to the endpoint.
        maryMajorsSmsEndpoint.addAttributesEntry("interests", Arrays.asList(
                "cooking",
                "politics",
                "finance"));

        // Adds custom metrics to the endpoint.
        maryMajorsSmsEndpoint.addMetricsEntry("cooking_interest_level", 5.0);
        maryMajorsSmsEndpoint.addMetricsEntry("politics_interest_level", 8.0);
        maryMajorsSmsEndpoint.addMetricsEntry("finance_interest_level", 4.0);

        // Initializes a user definition with a user ID.
        EndpointUser maryMajor = new EndpointUser().withUserId("example_user_2");

        // Adds custom user attributes.
        maryMajor.addUserAttributesEntry("name", Arrays.asList("Mary", "Major"));

        // Adds the user definition to the endpoint.
        maryMajorsSmsEndpoint.setUser(maryMajor);

        // Adds multiple endpoint definitions to a single request object.
        EndpointBatchRequest endpointList = new EndpointBatchRequest()
                .withItem(richardRoesEmailEndpoint)
                .withItem(maryMajorsSmsEndpoint);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        // Updates or creates the endpoints with Amazon Pinpoint.
        UpdateEndpointsBatchResult result = pinpointClient.updateEndpointsBatch(
                new UpdateEndpointsBatchRequest()
                .withApplicationId(applicationId)
                .withEndpointBatchRequest(endpointList));

        System.out.format("Update endpoints batch result: %s\n",
                result.getMessageBody().getMessage());

    }

}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example Put Endpoints Request**  
To submit an endpoint batch request, issue a `PUT` request to the [Endpoints](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints.html) resource at the following URI:  
`/v1/apps/application-id/endpoints`  
Where *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating the endpoints\.  
In your request, include the required headers, and provide the [EndpointBatchRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints.html#apps-application-id-endpoints-schemas) JSON as the body:  

```
PUT /v1/apps/application_id/endpoints HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
X-Amz-Date: 20180501T184948Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180501/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;content-length;content-type;host;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{
    "Item": [
        {
            "ChannelType": "EMAIL",
            "Address": "richard_roe@example.com",
            "Attributes": {
                "Interests": [
                    "Music",
                    "Books"
                ]
            },
            "Metrics": {
                "music_interest_level": 3.0,
                "books_interest_level": 7.0
            },
            "Id": "example_endpoint_1",
            "User":{
                "UserId": "example_user_1",
                "UserAttributes": {
                    "FirstName": "Richard",
                    "LastName": "Roe"
                }
            }
        },
        {
            "ChannelType": "SMS",
            "Address": "+16145550100",
            "Attributes": {
                "Interests": [
                    "Cooking",
                    "Politics",
                    "Finance"
                ]
            },
            "Metrics": {
                "cooking_interest_level": 5.0,
                "politics_interest_level": 8.0,
                "finance_interest_level": 4.0
            },
            "Id": "example_endpoint_2",
            "User": {
                "UserId": "example_user_2",
                "UserAttributes": {
                    "FirstName": "Mary",
                    "LastName": "Major"
                }
            }
        }
    ]
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

## Related Information<a name="audience-define-endpoints-batch-related"></a>

For more information about the Endpoint resource in the Amazon Pinpoint API, including the supported HTTP methods and request parameters, see [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) in the *Amazon Pinpoint API Reference\.*
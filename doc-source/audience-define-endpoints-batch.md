# Adding a batch of endpoints to Amazon Pinpoint<a name="audience-define-endpoints-batch"></a>

You can add or update multiple endpoints in a single operation by providing the endpoints in batches\. Each batch request can include up to 100 endpoint definitions\.

If you want to add or update more than 100 endpoints in a single operation, see [Importing endpoints into Amazon Pinpoint](audience-define-import.md) instead\.

## Examples<a name="audience-define-endpoints-batch-examples"></a>

The following examples show you how to add two endpoints at once by including the endpoints in a batch request\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Update endpoints batch command**  
To submit an endpoint batch request, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoints-batch.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoints-batch.html) command:  

```
$ aws pinpoint update-endpoints-batch \
> --application-id application-id \
> --endpoint-batch-request file://endpoint_batch_request_file.json
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating the endpoints\.
+ *endpoint\_batch\_request\_file\.json* is the file path to a local JSON file that contains the input for the `--endpoint-batch-request` parameter\.

**Example Endpoint batch request file**  
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
To submit an endpoint batch request, initialize an [https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/model/EndpointRequest.html](https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/model/EndpointRequest.html) object, and pass it to the [https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpointsBatch-com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchRequest-](https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpointsBatch-com.amazonaws.services.pinpoint.model.UpdateEndpointsBatchRequest-) method of the `AmazonPinpoint` client\. The following example populates an `EndpointBatchRequest` object with two `EndpointBatchItem` objects:  

```
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpoint.PinpointClient;
import software.amazon.awssdk.services.pinpoint.model.UpdateEndpointsBatchResponse;
import software.amazon.awssdk.services.pinpoint.model.EndpointUser;
import software.amazon.awssdk.services.pinpoint.model.EndpointBatchItem;
import software.amazon.awssdk.services.pinpoint.model.ChannelType;
import software.amazon.awssdk.services.pinpoint.model.EndpointBatchRequest;
import software.amazon.awssdk.services.pinpoint.model.PinpointException;
import software.amazon.awssdk.services.pinpoint.model.UpdateEndpointsBatchRequest;
import java.util.Map;
import java.util.List;
import java.util.ArrayList;
import java.util.HashMap;
```

```
    public static void updateEndpointsViaBatch( PinpointClient pinpoint, String applicationId) {

        try {
            List<String> myList = new ArrayList<String>();
            myList.add("music");
            myList.add("books");

            Map myMap = new HashMap<String, List>();
            myMap.put("attributes", myList);

            List<String> myNames = new ArrayList<String>();
            myList.add("Richard");
            myList.add("Roe");

            Map myMap2 = new HashMap<String, List>();
            myMap2.put("name",myNames );

            EndpointUser richardRoe = EndpointUser.builder()
                .userId("example_user_1")
                .userAttributes(myMap2)
                .build();

            // Create an EndpointBatchItem object for Richard Roe.
            EndpointBatchItem richardRoesEmailEndpoint = EndpointBatchItem.builder()
                .channelType(ChannelType.EMAIL)
                .address("richard_roe@example.com")
                .id("example_endpoint_1")
                .attributes(myMap)
                .user(richardRoe)
                .build();

            List<String> myListMary = new ArrayList<String>();
            myListMary.add("cooking");
            myListMary.add("politics");
            myListMary.add("finance");

            Map myMapMary = new HashMap<String, List>();
            myMapMary.put("interests", myListMary);

            List<String> myNameMary = new ArrayList<String>();
            myNameMary.add("Mary ");
            myNameMary.add("Major");

            Map maryName = new HashMap<String, List>();
            myMapMary.put("name",myNameMary );

            EndpointUser maryMajor = EndpointUser.builder()
                .userId("example_user_2")
                .userAttributes(maryName)
                .build();

            // Create an EndpointBatchItem object for Mary Major.
            EndpointBatchItem maryMajorsSmsEndpoint = EndpointBatchItem.builder()
                .channelType(ChannelType.SMS)
                .address("+16145550100")
                .id("example_endpoint_2")
                .attributes(myMapMary)
                .user(maryMajor)
                .build();

            // Adds multiple endpoint definitions to a single request object.
            EndpointBatchRequest endpointList = EndpointBatchRequest.builder()
                .item( richardRoesEmailEndpoint)
                .item( maryMajorsSmsEndpoint)
                .build();

            // Create the UpdateEndpointsBatchRequest.
            UpdateEndpointsBatchRequest batchRequest = UpdateEndpointsBatchRequest.builder()
                .applicationId(applicationId)
                .endpointBatchRequest(endpointList)
                .build();

            //  Updates the endpoints with Amazon Pinpoint.
            UpdateEndpointsBatchResponse result = pinpoint.updateEndpointsBatch(batchRequest);
            System.out.format("Update endpoints batch result: %s\n",
                result.messageBody().message());

     } catch (PinpointException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
  }
```

For the full SDK example, see [AddExampleEndpoints\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/AddExampleEndpoints.java/) on [GitHub](https://github.com/)\.

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example Put endpoints request**  
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

## Related information<a name="audience-define-endpoints-batch-related"></a>

For more information about the Endpoint resource in the Amazon Pinpoint API, including the supported HTTP methods and request parameters, see [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) in the *Amazon Pinpoint API Reference\.*
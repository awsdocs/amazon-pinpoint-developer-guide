# Deleting endpoints from Amazon Pinpoint<a name="audience-define-remove"></a>

You can delete endpoints when you no longer want to message a certain destination—such as when the destination becomes unreachable, or when a customer closes an account\.

## Examples<a name="audience-define-remove-endpoints"></a>

The following examples show you how to delete an endpoint\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Delete endpoint command**  
To delete an endpoint, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/delete-endpoint.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/delete-endpoint.html) command:  

```
$ aws pinpoint delete-endpoint \
> --application-id application-id \
> --endpoint-id endpoint-id
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project that contains the endpoint\.
+ *endpoint\-id* is the ID of the endpoint that you're deleting\.
The response to this command is the JSON definition of the endpoint that you deleted\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To delete an endpoint, use the [https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEndpoint-com.amazonaws.services.pinpoint.model.DeleteEndpointRequest-](https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEndpoint-com.amazonaws.services.pinpoint.model.DeleteEndpointRequest-) method of the `AmazonPinpoint` client\. Provide a [https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/model/DeleteEndpointRequest.html](https://docs.aws.amazon.com/sdk-for-java/latest/reference/com/amazonaws/services/pinpoint/model/DeleteEndpointRequest.html) object as the method argument:  

```
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpoint.PinpointClient;
import software.amazon.awssdk.services.pinpoint.model.DeleteEndpointRequest;
import software.amazon.awssdk.services.pinpoint.model.DeleteEndpointResponse;
import software.amazon.awssdk.services.pinpoint.model.PinpointException;
```

```
    public static void deletePinEncpoint(PinpointClient pinpoint, String appId, String endpointId ) {

        try {
            DeleteEndpointRequest appRequest = DeleteEndpointRequest.builder()
                .applicationId(appId)
                .endpointId(endpointId)
                .build();

            DeleteEndpointResponse result = pinpoint.deleteEndpoint(appRequest);
            String id = result.endpointResponse().id();
            System.out.println("The deleted endpoint id  " + id);

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        System.out.println("Done");
    }
```

For the full SDK example, see [DeleteEndpoint\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/DeleteEndpoint.java/) on [GitHub](https://github.com/)\.

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example DELETE endpoint request**  
To delete an endpoint, issue a `DELETE` request to the [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoint.html) resource:  

```
DELETE /v1/apps/application-id/endpoints/endpoint-id HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
Content-Type: application/json
Accept: application/json
Cache-Control: no-cache
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project that contains the endpoint\.
+ *endpoint\-id* is the ID of the endpoint that you're deleting\.
The response to this request is the JSON definition of the endpoint that you deleted\.

------
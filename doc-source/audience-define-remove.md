# Deleting Endpoints from Amazon Pinpoint<a name="audience-define-remove"></a>

You can delete endpoints when you no longer want to message a certain destination—such as when the destination becomes unreachable, or when a customer closes an account\.

## Examples<a name="audience-define-remove-endpoints"></a>

The following examples show you how to delete an endpoint\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Delete Endpoint Command**  
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
To delete an endpoint, use the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEndpoint-com.amazonaws.services.pinpoint.model.DeleteEndpointRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#deleteEndpoint-com.amazonaws.services.pinpoint.model.DeleteEndpointRequest-) method of the `AmazonPinpoint` client\. Provide a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/DeleteEndpointRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/DeleteEndpointRequest.html) object as the method argument:  

```
import com.amazonaws.AmazonServiceException;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.DeleteEndpointRequest;
import com.amazonaws.services.pinpoint.model.DeleteEndpointResult;

import java.util.Arrays;

public class DeleteEndpoints {

    public static void main(String[] args) {

        final String USAGE = "\n" +
                "DeleteEndpoints - Removes one or more endpoints from an " +
                "Amazon Pinpoint application.\n\n" +

                "Usage: DeleteEndpoints <applicationId> <endpointId1> [endpointId2 ...]\n";

        if (args.length < 2) {
            System.out.println(USAGE);
            System.exit(1);
        }

        String applicationId = args[0];
        String[] endpointIds = Arrays.copyOfRange(args, 1, args.length);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        try {
            // Deletes each of the specified endpoints with the Amazon Pinpoint client.
            for (String endpointId: endpointIds) {
                DeleteEndpointResult result =
                        pinpointClient.deleteEndpoint(new DeleteEndpointRequest()
                        .withEndpointId(endpointId)
                        .withApplicationId(applicationId));
                System.out.format("Deleted endpoint %s.\n", result.getEndpointResponse().getId());
            }
        } catch (AmazonServiceException e) {
            System.err.println(e.getErrorMessage());
            System.exit(1);
        }
    }
}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example DELETE Endpoint Request**  
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
# Listing endpoint IDs with Amazon Pinpoint<a name="audience-data-list-ids"></a>

To update or delete an endpoint, you need the endpoint ID\. So, if you want to perform these operations on all of the endpoints in an Amazon Pinpoint project, the first step is to list all of the endpoint IDs that belong to that project\. Then, you can iterate over these IDs to, for example, add an attribute globally or delete all of the endpoints in your project\.

The following example uses the AWS SDK for Java and does the following:

1. Calls the example `exportEndpointsToS3` method from the example code in [Exporting endpoints from Amazon Pinpoint](audience-data-export.md)\. This method exports the endpoint definitions from an Amazon Pinpoint project\. The endpoint definitions are added as gzip files to an Amazon S3 bucket\.

1. Downloads the exported gzip files\.

1. Reads the gzip files and obtains the endpoint ID from each endpoint's JSON definition\.

1. Prints the endpoint IDs to the console\.

1. Cleans up by deleting the files that Amazon Pinpoint added to Amazon S3\.

```
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.pinpoint.PinpointClient;
import software.amazon.awssdk.services.pinpoint.model.EndpointResponse;
import software.amazon.awssdk.services.pinpoint.model.GetUserEndpointsRequest;
import software.amazon.awssdk.services.pinpoint.model.GetUserEndpointsResponse;
import software.amazon.awssdk.services.pinpoint.model.PinpointException;
import java.util.List;
```

```
    public static void listAllEndpoints(PinpointClient pinpoint,
                                        String applicationId,
                                       String userId ) {

        try {
            GetUserEndpointsRequest endpointsRequest = GetUserEndpointsRequest.builder()
                    .userId(userId)
                    .applicationId(applicationId)
                    .build();

            GetUserEndpointsResponse response = pinpoint.getUserEndpoints(endpointsRequest);
            List<EndpointResponse> endpoints = response.endpointsResponse().item();

            // Display the results
            for (EndpointResponse endpoint: endpoints) {
                System.out.println("The channel type is: "+endpoint.channelType());
                System.out.println("The address is  "+endpoint.address());
            }

        } catch ( PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

For the full SDK example, see [ListEndpointIs\.java](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javav2/example_code/pinpoint/src/main/java/com/example/pinpoint/ListEndpointIds.java/) on [GitHub](https://github.com/)\.
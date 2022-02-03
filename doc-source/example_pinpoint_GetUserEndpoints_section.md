# List Amazon Pinpoint endpoints that are associated with a specific user ID<a name="example_pinpoint_GetUserEndpoints_section"></a>

The following code example shows how to list endpoints\.

------
#### [ Java ]

**SDK for Java 2\.x**  
  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [GetUserEndpoints](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/GetUserEndpoints) in *AWS SDK for Java 2\.x API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
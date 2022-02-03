# Delete an Amazon Pinpoint endpoint<a name="example_pinpoint_DeleteEndpoint_section"></a>

The following code examples show how to delete an endpoint\.

------
#### [ Java ]

**SDK for Java 2\.x**  
Delete an endpoint\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [DeleteEndpoint](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/DeleteEndpoint) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun deletePinEncpoint(appIdVal: String?, endpointIdVal: String?) {

        val deleteEndpointRequest = DeleteEndpointRequest {
                applicationId =  appIdVal
                endpointId = endpointIdVal
        }

       PinpointClient { region = "us-west-2" }.use { pinpoint ->
            val result = pinpoint.deleteEndpoint(deleteEndpointRequest)
            val id = result.endpointResponse?.id
            println("The deleted endpoint is  $id")
        }
 }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [DeleteEndpoint](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
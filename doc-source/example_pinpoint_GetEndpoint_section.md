# Display information about an existing Amazon Pinpoint endpoint<a name="example_pinpoint_GetEndpoint_section"></a>

The following code examples show how to get endpoints\.

------
#### [ Java ]

**SDK for Java 2\.x**  
  

```
    public static void lookupPinpointEndpoint(PinpointClient pinpoint, String appId, String endpoint ) {

        try {
            GetEndpointRequest appRequest = GetEndpointRequest.builder()
                    .applicationId(appId)
                    .endpointId(endpoint)
                    .build();

            GetEndpointResponse result = pinpoint.getEndpoint(appRequest);
            EndpointResponse endResponse = result.endpointResponse();

            // Uses the Google Gson library to pretty print the endpoint JSON.
            Gson gson = new GsonBuilder()
                    .setFieldNamingPolicy(FieldNamingPolicy.UPPER_CAMEL_CASE)
                    .setPrettyPrinting()
                    .create();

            String endpointJson = gson.toJson(endResponse);
            System.out.println(endpointJson);

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        System.out.println("Done");
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [GetEndpoint](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/GetEndpoint) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun lookupPinpointEndpoint(appId: String?, endpoint: String?) {

          PinpointClient { region = "us-west-2" }.use { pinpoint ->
            val result = pinpoint.getEndpoint(GetEndpointRequest {
                applicationId = appId
                endpointId = endpoint
            })
            val endResponse = result.endpointResponse

            // Uses the Google Gson library to pretty print the endpoint JSON.
            val gson: com.google.gson.Gson = GsonBuilder()
                .setFieldNamingPolicy(FieldNamingPolicy.UPPER_CAMEL_CASE)
                .setPrettyPrinting()
                .create()

            val endpointJson: String = gson.toJson(endResponse)
            println(endpointJson)

          }
   }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [GetEndpoint](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
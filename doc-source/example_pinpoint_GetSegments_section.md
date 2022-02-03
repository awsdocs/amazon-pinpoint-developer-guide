# List Amazon Pinpoint segments in an application<a name="example_pinpoint_GetSegments_section"></a>

The following code examples show how to list segments\.

------
#### [ Java ]

**SDK for Java 2\.x**  
List segments\.  

```
    public static void listSegs( PinpointClient pinpoint, String appId) {

        try {
            GetSegmentsRequest request = GetSegmentsRequest.builder()
                    .applicationId(appId)
                    .build();

            GetSegmentsResponse response = pinpoint.getSegments(request);
            List<SegmentResponse> segments = response.segmentsResponse().item();

            for(SegmentResponse segment: segments) {
                System.out.println("Segement " + segment.id() + " " + segment.name() + " " + segment.lastModifiedDate());
            }
        } catch ( PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [GetSegments](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/GetSegments) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun listSegs(appId: String?) {

    PinpointClient { region = "us-west-2" }.use { pinpoint ->

        val response = pinpoint.getSegments(GetSegmentsRequest {
            applicationId = appId
        })
        response.segmentsResponse?.item?.forEach { segment ->
                 println("Segement id is ${segment.id.toString()}")
            }
        }
 }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [GetSegments](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
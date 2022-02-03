# Create an Amazon Pinpoint application<a name="example_pinpoint_CreateApp_section"></a>

The following code examples show how to create an appliation\.

------
#### [ Java ]

**SDK for Java 2\.x**  
  

```
    public static String createApplication(PinpointClient pinpoint, String appName) {

        try {
            CreateApplicationRequest appRequest = CreateApplicationRequest.builder()
                    .name(appName)
                    .build();

            CreateAppRequest request = CreateAppRequest.builder()
                    .createApplicationRequest(appRequest)
                    .build();

            CreateAppResponse result = pinpoint.createApp(request);
            return result.applicationResponse().id();

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [CreateApp](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/CreateApp) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
 suspend fun createApplication(applicationName: String?):String? {

            val createApplicationRequestOb = CreateApplicationRequest{
                name = applicationName
            }

            PinpointClient { region = "us-west-2" }.use { pinpoint ->
             val result = pinpoint.createApp(CreateAppRequest {
                createApplicationRequest = createApplicationRequestOb
             })
             return result.applicationResponse?.id
        }
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [CreateApp](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
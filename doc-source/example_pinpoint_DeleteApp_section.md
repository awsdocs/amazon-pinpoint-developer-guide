# Delete an Amazon Pinpoint application<a name="example_pinpoint_DeleteApp_section"></a>

The following code examples show how to delete an application\.

------
#### [ Java ]

**SDK for Java 2\.x**  
Delete an application\.  

```
    public static void deletePinApp(PinpointClient pinpoint, String appId ) {

        try {

            DeleteAppRequest appRequest = DeleteAppRequest.builder()
                    .applicationId(appId)
                    .build();

            DeleteAppResponse result = pinpoint.deleteApp(appRequest);
            String appName = result.applicationResponse().name();
            System.out.println("Application " + appName + " has been deleted.");

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
   }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [DeleteApp](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/DeleteApp) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
 suspend fun deletePinApp(appId: String?) {

             PinpointClient { region = "us-west-2" }.use { pinpoint ->
               val result = pinpoint.deleteApp(DeleteAppRequest {
                 applicationId = appId
               })
               val appName= result.applicationResponse?.name
               println("Application $appName has been deleted.")
             }
  }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [DeleteApp](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
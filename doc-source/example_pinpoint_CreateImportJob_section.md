# Import an Amazon Pinpoint segment<a name="example_pinpoint_CreateImportJob_section"></a>

The following code example shows how to import a segment\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
Import a segment\.  

```
    public static ImportJobResponse createImportSegment(PinpointClient client,
                                                        String appId,
                                                        String bucket,
                                                        String key,
                                                        String roleArn) {

        try {
             ImportJobRequest importRequest = ImportJobRequest.builder()
                    .defineSegment(true)
                    .registerEndpoints(true)
                    .roleArn(roleArn)
                    .format(Format.JSON)
                    .s3Url("s3://" + bucket + "/" + key)
                    .build();

            CreateImportJobRequest jobRequest = CreateImportJobRequest.builder()
                    .importJobRequest(importRequest)
                    .applicationId(appId)
                    .build();

            CreateImportJobResponse jobResponse = client.createImportJob(jobRequest);

            return jobResponse.importJobResponse();

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return null;
    }
```
+  For API details, see [CreateImportJob](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/CreateImportJob) in *AWS SDK for Java 2\.x API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.
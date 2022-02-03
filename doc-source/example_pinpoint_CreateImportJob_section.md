# Import an Amazon Pinpoint segment<a name="example_pinpoint_CreateImportJob_section"></a>

The following code example shows how to import a segment\.

------
#### [ Java ]

**SDK for Java 2\.x**  
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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [CreateImportJob](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/CreateImportJob) in *AWS SDK for Java 2\.x API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
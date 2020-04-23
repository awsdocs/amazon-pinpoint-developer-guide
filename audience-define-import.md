# Importing Endpoints into Amazon Pinpoint<a name="audience-define-import"></a>

You can add or update endpoints in large numbers by importing them from an Amazon S3 bucket\. Importing endpoints is useful if you have records about your audience outside of Amazon Pinpoint, and you want to add this information to an Amazon Pinpoint project\. In this case, you would:

1. Create endpoint definitions that are based on your own audience data\.

1. Save these endpoint definitions in one or more files, and upload the files to an Amazon S3 bucket\.

1. Add the endpoints to your Amazon Pinpoint project by importing them from the bucket\.

Each import job can transfer up to 1 GB of data\. In a typical job, where each endpoint is 4 KB or less, you could import around 250,000 endpoints\. You can run up to two concurrent import jobs per AWS account\. If you need more bandwidth for your import jobs, you can submit a service quota increase request to AWS Support\. For more information, see [Requesting a Quota Increase](quotas.md#quotas-increase)\.

## Before You Begin<a name="audience-define-import-before"></a>

Before you can import endpoints, you need the following resources in your AWS account:
+ An Amazon S3 bucket\. To create a bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.
+ An AWS Identity and Access Management \(IAM\) role that grants Amazon Pinpoint read permissions for your Amazon S3 bucket\. To create the role, see [IAM Role for Importing Endpoints or SegmentsImporting Endpoints or Segments](permissions-import-segment.md)\.

## Examples<a name="audience-define-import-examples"></a>

The following examples demonstrate how to add endpoint definitions to your Amazon S3 bucket, and then import those endpoints into an Amazon Pinpoint project\.

### Files with Endpoint Definitions<a name="audience-define-import-examples-files"></a>

The files that you add to your Amazon S3 bucket can contain endpoint definitions in CSV or newline\-delimited JSON format\. For the attributes that you can use to define your endpoints, see the [EndpointRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#apps-application-id-endpoints-endpoint-id-schemas) JSON schema in the *Amazon Pinpoint API Reference*\.

------
#### [ CSV ]

You can import endpoints that are defined in a CSV file, as in the following example:

```
ChannelType,Address,Location.Country,Demographic.Platform,Demographic.Make,User.UserId
SMS,2065550182,CAN,Android,LG,example-user-id-1
APNS,1a2b3c4d5e6f7g8h9i0j1a2b3c4d5e6f,USA,iOS,Apple,example-user-id-2
EMAIL,john.stiles@example.com,USA,iOS,Apple,example-user-id-2
```

The first line is the header, which contains the endpoint attributes\. Specify nested attributes by using dot notation, as in `Location.Country`\.

The subsequent lines define the endpoints by providing values for each of the attributes in the header\.

To include a comma, line break, or double quote in a value, enclose the value in double quotes, as in `"aaa,bbb"`\. For more information about the CSV format, see [RFC 4180 Common Format and MIME Type for Comma\-Separated Values \(CSV\) Files](https://tools.ietf.org/html/rfc4180)\.

------
#### [ JSON ]

You can import endpoints that are defined in a newline\-delimited JSON file, as in the following example:

```
{"ChannelType":"SMS","Address":"2065550182","Location":{"Country":"CAN"},"Demographic":{"Platform":"Android","Make":"LG"},"User":{"UserId":"example-user-id-1"}}
{"ChannelType":"APNS","Address":"1a2b3c4d5e6f7g8h9i0j1a2b3c4d5e6f","Location":{"Country":"USA"},"Demographic":{"Platform":"iOS","Make":"Apple"},"User":{"UserId":"example-user-id-2"}}
{"ChannelType":"EMAIL","Address":"john.stiles@example.com","Location":{"Country":"USA"},"Demographic":{"Platform":"iOS","Make":"Apple"},"User":{"UserId":"example-user-id-2"}}
```

In this format, each line is a complete JSON object that contains an individual endpoint definition\.

------

### Import Job Requests<a name="audience-define-import-examples-jobs"></a>

The following examples show you how to add endpoint definitions to Amazon S3 by uploading a local file to a bucket\. Then, the examples import the endpoint definitions into an Amazon Pinpoint project\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example S3 CP Command**  
To upload a local file to an Amazon S3 bucket, use the Amazon S3 [https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command:  

```
$ aws s3 cp ./endpoints-file s3://bucket-name/prefix/
```

Where:
+ *\./endpoints\-file* is the file path to a local file that contains the endpoint definitions\.
+ *bucket\-name/prefix/* is the name of your Amazon S3 bucket and, optionally, a prefix that helps you organize the objects in your bucket hierarchically\. For example, a useful prefix might be `pinpoint/imports/endpoints/`\.

**Example Create Import Job Command**  
To import endpoint definitions from an Amazon S3 bucket, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/create-import-job.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/create-import-job.html) command:  

```
$ aws pinpoint create-import-job \
> --application-id application-id \
> --import-job-request \
> S3Url=s3://bucket-name/prefix/key,\
> RoleArn=iam-import-role-arn,\
> Format=format,\
> RegisterEndpoints=true
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project that you're importing endpoints for\.
+ *bucket\-name/prefix/key* is the location in Amazon S3 that contains one or more objects to import\. The location can end with the key for an individual object, or it can end with a prefix that qualifies multiple objects\.
+ *iam\-import\-role\-arn* is the Amazon Resource Name \(ARN\) of an IAM role that grants Amazon Pinpoint read access to the bucket\.
+ *format* can be either `JSON` or `CSV`, depending on which format you used to define your endpoints\. If the Amazon S3 location includes multiple objects of mixed formats, Amazon Pinpoint imports only the objects that match the specified format\.
The response includes details about the import job:  

```
{
    "ImportJobResponse": {
        "CreationDate": "2018-05-24T21:26:33.995Z",
        "Definition": {
            "DefineSegment": false,
            "ExternalId": "463709046829",
            "Format": "JSON",
            "RegisterEndpoints": true,
            "RoleArn": "iam-import-role-arn",
            "S3Url": "s3://bucket-name/prefix/key"
        },
        "Id": "d5ecad8e417d498389e1d5b9454d4e0c",
        "JobStatus": "CREATED",
        "Type": "IMPORT"
    }
}
```
The response provides the job ID with the `Id` attribute\. You can use this ID to check the current status of the import job\.

**Example Get Import Job Command**  
To check the current status of an import job, use the `get-import-job` command:  

```
$ aws pinpoint get-import-job \
> --application-id application-id \
> --job-id job-id
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project that the import job was initiated for\.
+ *job\-id* is the ID of the import job that you're checking\.
The response to this command provides the current state of the import job:  

```
{
    "ImportJobResponse": {
        "ApplicationId": "application-id",
        "CompletedPieces": 1,
        "CompletionDate": "2018-05-24T21:26:45.308Z",
        "CreationDate": "2018-05-24T21:26:33.995Z",
        "Definition": {
            "DefineSegment": false,
            "ExternalId": "463709046829",
            "Format": "JSON",
            "RegisterEndpoints": true,
            "RoleArn": "iam-import-role-arn",
            "S3Url": "s3://s3-bucket-name/prefix/endpoint-definitions.json"
        },
        "FailedPieces": 0,
        "Id": "job-id",
        "JobStatus": "COMPLETED",
        "TotalFailures": 0,
        "TotalPieces": 1,
        "TotalProcessed": 3,
        "Type": "IMPORT"
    }
}
```
The response provides the job status with the `JobStatus` attribute\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To upload a file with endpoint definitions to Amazon S3, use the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html#putObject-java.lang.String-java.lang.String-java.io.File-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html#putObject-java.lang.String-java.lang.String-java.io.File-) method of the `AmazonS3` client\.   
To import the endpoints into an Amazon Pinpoint project, initialize a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/CreateImportJobRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/CreateImportJobRequest.html) object\. Then, pass this object to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#createImportJob-com.amazonaws.services.pinpoint.model.CreateImportJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#createImportJob-com.amazonaws.services.pinpoint.model.CreateImportJobRequest-) method of the `AmazonPinpoint` client\.  

```
package com.amazonaws.examples.pinpoint;

import com.amazonaws.AmazonServiceException;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.CreateImportJobRequest;
import com.amazonaws.services.pinpoint.model.CreateImportJobResult;
import com.amazonaws.services.pinpoint.model.Format;
import com.amazonaws.services.pinpoint.model.GetImportJobRequest;
import com.amazonaws.services.pinpoint.model.GetImportJobResult;
import com.amazonaws.services.pinpoint.model.ImportJobRequest;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.s3.model.AmazonS3Exception;
import java.io.File;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class ImportEndpoints {

    public static void main(String[] args) {

        final String USAGE = "\n" +
        "ImportEndpoints - Adds endpoints to an Amazon Pinpoint application by: \n" +
        "1.) Uploading the endpoint definitions to an Amazon S3 bucket. \n" +
        "2.) Importing the endpoint definitions from the bucket to an Amazon Pinpoint " +
                "application.\n\n" +
        "Usage: ImportEndpoints <endpointsFileLocation> <s3BucketName> <iamImportRoleArn> " +
                "<applicationId>\n\n" +
        "Where:\n" +
        "  endpointsFileLocation - The relative location of the JSON file that contains the " +
                "endpoint definitions.\n" +
        "  s3BucketName - The name of the Amazon S3 bucket to upload the JSON file to. If the " +
                "bucket doesn't exist, a new bucket is created.\n" +
        "  iamImportRoleArn - The ARN of an IAM role that grants Amazon Pinpoint read " +
                "permissions to the S3 bucket.\n" +
        "  applicationId - The ID of the Amazon Pinpoint application to add the endpoints to.";

        if (args.length < 1) {
            System.out.println(USAGE);
            System.exit(1);
        }

        String endpointsFileLocation = args[0];
        String s3BucketName = args[1];
        String iamImportRoleArn = args[2];
        String applicationId = args[3];

        Path endpointsFilePath = Paths.get(endpointsFileLocation);
        File endpointsFile = new File(endpointsFilePath.toAbsolutePath().toString());
        uploadToS3(endpointsFile, s3BucketName);

        importToPinpoint(endpointsFile.getName(), s3BucketName, iamImportRoleArn, applicationId);

    }

    private static void uploadToS3(File endpointsFile, String s3BucketName) {

        // Initializes Amazon S3 client.
        final AmazonS3 s3 = AmazonS3ClientBuilder.defaultClient();

        // Checks whether the specified bucket exists. If not, attempts to create one.
        if (!s3.doesBucketExistV2(s3BucketName)) {
            try {
                s3.createBucket(s3BucketName);
                System.out.format("Created S3 bucket %s.\n", s3BucketName);
            } catch (AmazonS3Exception e) {
                System.err.println(e.getErrorMessage());
                System.exit(1);
            }
        }

        // Uploads the endpoints file to the bucket.
        String endpointsFileName = endpointsFile.getName();
        System.out.format("Uploading %s to S3 bucket %s . . .\n", endpointsFileName, s3BucketName);
        try {
            s3.putObject(s3BucketName, "imports/" + endpointsFileName, endpointsFile);
            System.out.println("Finished uploading to S3.");
        } catch (AmazonServiceException e) {
            System.err.println(e.getErrorMessage());
            System.exit(1);
        }
    }

    private static void importToPinpoint(String endpointsFileName, String s3BucketName,
                                         String iamImportRoleArn, String applicationId) {

        // The S3 URL that Amazon Pinpoint requires to find the endpoints file.
        String s3Url = "s3://" + s3BucketName + "/imports/" + endpointsFileName;

        // Defines the import job that Amazon Pinpoint runs.
        ImportJobRequest importJobRequest = new ImportJobRequest()
                .withS3Url(s3Url)
                .withRegisterEndpoints(true)
                .withRoleArn(iamImportRoleArn)
                .withFormat(Format.JSON);
        CreateImportJobRequest createImportJobRequest = new CreateImportJobRequest()
                .withApplicationId(applicationId)
                .withImportJobRequest(importJobRequest);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        System.out.format("Importing endpoints in %s to Amazon Pinpoint application %s . . .\n",
                endpointsFileName, applicationId);

        try {

            // Runs the import job with Amazon Pinpoint.
            CreateImportJobResult importResult =
                    pinpointClient.createImportJob(createImportJobRequest);

            String jobId = importResult.getImportJobResponse().getId();
            GetImportJobResult getImportJobResult = null;
            String jobStatus = null;

            // Checks the job status until the job completes or fails.
            do {
                getImportJobResult = pinpointClient.getImportJob(new GetImportJobRequest()
                        .withJobId(jobId)
                        .withApplicationId(applicationId));
                jobStatus = getImportJobResult.getImportJobResponse().getJobStatus();
                System.out.format("Import job %s . . .\n", jobStatus.toLowerCase());
                TimeUnit.SECONDS.sleep(3);
            } while (!jobStatus.equals("COMPLETED") && !jobStatus.equals("FAILED"));

            if (jobStatus.equals("COMPLETED")) {
                System.out.println("Finished importing endpoints.");
            } else {
                System.err.println("Failed to import endpoints.");
                System.exit(1);
            }

            // Checks for entries that failed to import.
            // getFailures provides up to 100 of the first failed entries for the job, if any exist.
            List<String> failedEndpoints = getImportJobResult.getImportJobResponse().getFailures();
            if (failedEndpoints != null) {
                System.out.println("Failed to import the following entries:");
                for (String failedEndpoint : failedEndpoints) {
                    System.out.println(failedEndpoint);
                }
            }

        } catch (AmazonServiceException | InterruptedException e) {
            System.err.println(e.getMessage());
            System.exit(1);
        }

    }

}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example S3 PUT Object Request**  
To add your endpoint definitions to a bucket, use the Amazon S3 [PUT Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html) operation, and provide the endpoint definitions as the body:  

```
PUT /prefix/key HTTP/1.1
Content-Type: text/plain
Accept: application/json
Host: bucket-name.s3.amazonaws.com
X-Amz-Content-Sha256: c430dc094b0cec2905bc88d96314914d058534b14e2bc6107faa9daa12fdff2d
X-Amz-Date: 20180605T184132Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180605/us-east-1/s3/aws4_request, SignedHeaders=accept;cache-control;content-length;content-type;host;postman-token;x-amz-content-sha256;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{"ChannelType":"SMS","Address":"2065550182","Location":{"Country":"CAN"},"Demographic":{"Platform":"Android","Make":"LG"},"User":{"UserId":"example-user-id-1"}}
{"ChannelType":"APNS","Address":"1a2b3c4d5e6f7g8h9i0j1a2b3c4d5e6f","Location":{"Country":"USA"},"Demographic":{"Platform":"iOS","Make":"Apple"},"User":{"UserId":"example-user-id-2"}}
{"ChannelType":"EMAIL","Address":"john.stiles@example.com","Location":{"Country":"USA"},"Demographic":{"Platform":"iOS","Make":"Apple"},"User":{"UserId":"example-user-id-2"}}
```

Where:
+ */prefix/key* is the prefix and key name for the object that will contain the endpoint definitions after the upload\. You can use the prefix to organize your objects hierarchically\. For example, a useful prefix might be `pinpoint/imports/endpoints/`\.
+ *bucket\-name* is the name of the Amazon S3 bucket that you're adding the endpoint definitions to\.

**Example POST Import Job Request**  
To import endpoint definitions from an Amazon S3 bucket, issue a POST request to the [Import Jobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-import.html) resource\. In your request, include the required headers and provide the [ImportJobRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-import.html#apps-application-id-jobs-import-schemas) JSON as the body:  

```
POST /v1/apps/application_id/jobs/import HTTP/1.1
Content-Type: application/json
Accept: application/json
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180605T214912Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180605/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;cache-control;content-length;content-type;host;postman-token;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{
  "S3Url": "s3://bucket-name/prefix/key",
  "RoleArn": "iam-import-role-arn",
  "Format": "format",
  "RegisterEndpoints": true
}
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project that you're importing endpoints for\.
+ *bucket\-name/prefix/key* is the location in Amazon S3 that contains one or more objects to import\. The location can end with the key for an individual object, or it can end with a prefix that qualifies multiple objects\.
+ *iam\-import\-role\-arn* is the Amazon Resource Name \(ARN\) of an IAM role that grants Amazon Pinpoint read access to the bucket\.
+ *format* can be either `JSON` or `CSV`, depending on which format you used to define your endpoints\. If the Amazon S3 location includes multiple files of mixed formats, Amazon Pinpoint imports only the files that match the specified format\.
If your request succeeds, you receive a response like the following:  

```
{
    "Id": "a995ce5d70fa44adb563b7d0e3f6c6f5",
    "JobStatus": "CREATED",
    "CreationDate": "2018-06-05T21:49:15.288Z",
    "Type": "IMPORT",
    "Definition": {
        "S3Url": "s3://bucket-name/prefix/key",
        "RoleArn": "iam-import-role-arn",
        "ExternalId": "external-id",
        "Format": "JSON",
        "RegisterEndpoints": true,
        "DefineSegment": false
    }
}
```
The response provides the job ID with the `Id` attribute\. You can use this ID to check the current status of the import job\.

**Example GET Import Job Request**  
To check the current status of an import job, issue a `GET` request to the [Import Job](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-import-job-id.html) resource:  

```
GET /v1/apps/application_id/jobs/import/job_id HTTP/1.1
Content-Type: application/json
Accept: application/json
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180605T220744Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180605/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;cache-control;content-type;host;postman-token;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache
```
Where:  
+ *application\_id* is the ID of the Amazon Pinpoint project for which the import job was initiated\.
+ *job\_id* is the ID of the import job that you're checking\.
If your request succeeds, you receive a response like the following:  

```
{
    "ApplicationId": "application_id",
    "Id": "70a51b2cf442447492d2c8e50336a9e8",
    "JobStatus": "COMPLETED",
    "CompletedPieces": 1,
    "FailedPieces": 0,
    "TotalPieces": 1,
    "CreationDate": "2018-06-05T22:04:49.213Z",
    "CompletionDate": "2018-06-05T22:04:58.034Z",
    "Type": "IMPORT",
    "TotalFailures": 0,
    "TotalProcessed": 3,
    "Definition": {
        "S3Url": "s3://bucket-name/prefix/key.json",
        "RoleArn": "iam-import-role-arn",
        "ExternalId": "external-id",
        "Format": "JSON",
        "RegisterEndpoints": true,
        "DefineSegment": false
    }
}
```
The response provides the job status with the `JobStatus` attribute\.

------

## Related Information<a name="audience-define-import-related"></a>

For more information about the Import Jobs resource in the Amazon Pinpoint API, including the supported HTTP methods and request parameters, see [Import Jobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-import.html) in the *Amazon Pinpoint API Reference*\.
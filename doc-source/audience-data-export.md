# Exporting Endpoints from Amazon Pinpoint<a name="audience-data-export"></a>

To get all of the information that Amazon Pinpoint has about your audience, you can export the endpoint definitions that belong to a project\. When you export, Amazon Pinpoint places the endpoint definitions in an Amazon S3 bucket that you specify\. Exporting endpoints is useful when you want to:
+ View the latest data about new and existing endpoints that your client application registered with Amazon Pinpoint\.
+ Synchronize the endpoint data in Amazon Pinpoint with your own Customer Relationship Management \(CRM\) system\.
+ Create reports about or analyze your customer data\.

## Before You Begin<a name="audience-data-export-before"></a>

Before you can export endpoints, you need the following resources in your AWS account:
+ An Amazon S3 bucket\. To create a bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.
+ An AWS Identity and Access Management \(IAM\) role that grants Amazon Pinpoint write permissions for your Amazon S3 bucket\. To create the role, see [IAM Role for Exporting Endpoints or SegmentsExporting Endpoints or Segments](permissions-export-endpoints.md)\.

## Examples<a name="audience-data-export-examples"></a>

The following examples demonstrate how to export endpoints from an Amazon Pinpoint project, and then download those endpoints from your Amazon S3 bucket\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Create Export Job Command**  
To export the endpoints in your Amazon Pinpoint project, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/create-export-job.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/create-export-job.html) command:  

```
$ aws pinpoint create-export-job \
> --application-id application-id \
> --export-job-request \
> S3UrlPrefix=s3://bucket-name/prefix/,\
> RoleArn=iam-export-role-arn
```
Where:  
+ *`application-id`* is the ID of the Amazon Pinpoint project that contains the endpoints\.
+ *`bucket-name/prefix/`* is the name of your Amazon S3 bucket and, optionally, a prefix that helps you organize the objects in your bucket hierarchically\. For example, a useful prefix might be `pinpoint/exports/endpoints/`\.
+ *`iam-export-role-arn`* is the Amazon Resource Name \(ARN\) of an IAM role that grants Amazon Pinpoint write access to the bucket\.
The response to this command provides details about the export job:  

```
{
    "ExportJobResponse": {
        "CreationDate": "2018-06-04T22:04:20.585Z",
        "Definition": {
            "RoleArn": "iam-export-role-arn",
            "S3UrlPrefix": "s3://s3-bucket-name/prefix/"
        },
        "Id": "7390e0de8e0b462380603c5a4df90bc4",
        "JobStatus": "CREATED",
        "Type": "EXPORT"
    }
}
```
The response provides the job ID with the `Id` attribute\. You can use this ID to check the current status of the export job\.

**Example Get Export Job Command**  
To check the current status of an export job, use the [https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-export-job.html](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/get-export-job.html) command:  

```
$ aws pinpoint get-export-job \
> --application-id application-id \
> --job-id job-id
```

Where:
+ *`application-id`* is the ID the Amazon Pinpoint project that you exported the endpoints from\.
+ *`job-id`* is the ID of the job that you're checking\.

The response to this command provides the current state of the export job:

```
{
    "ExportJobResponse": {
        "ApplicationId": "application-id",
        "CompletedPieces": 1,
        "CompletionDate": "2018-05-08T22:16:48.228Z",
        "CreationDate": "2018-05-08T22:16:44.812Z",
        "Definition": {},
        "FailedPieces": 0,
        "Id": "6c99c463f14f49caa87fa27a5798bef9",
        "JobStatus": "COMPLETED",
        "TotalFailures": 0,
        "TotalPieces": 1,
        "TotalProcessed": 215,
        "Type": "EXPORT"
    }
}
```

The response provides the job status with the `JobStatus` attribute\. When the job status value is `COMPLETED`, you can get your exported endpoints from your Amazon S3 bucket\.

**Example S3 CP Command**  
To download your exported endpoints, use the Amazon S3 [https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command:  

```
$ aws s3 cp s3://bucket-name/prefix/key.gz /local/directory/
```
Where:  
+ *`bucket-name/prefix/key`* is the location of the \.gz file that Amazon Pinpoint added to your bucket when you exported your endpoints\. This file contains the exported endpoint definitions\.
+ *`/local/directory/`* is the file path to the local directory that you want to download the endpoints to\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To export endpoints from an Amazon Pinpoint project, initialize a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/CreateExportJobRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/CreateExportJobRequest.html) object\. Then, pass this object to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#createExportJob-com.amazonaws.services.pinpoint.model.CreateExportJobRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#createExportJob-com.amazonaws.services.pinpoint.model.CreateExportJobRequest-) method of the `AmazonPinpoint` client\.  
To download the exported endpoints from Amazon Pinpoint, use the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html#getObject-java.lang.String-java.lang.String-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html#getObject-java.lang.String-java.lang.String-) method of the `AmazonS3` client\.  

```
import com.amazonaws.AmazonServiceException;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.*;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.s3.model.S3Object;
import com.amazonaws.services.s3.model.S3ObjectInputStream;
import com.amazonaws.services.s3.model.S3ObjectSummary;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Date;
import java.util.List;
import java.util.concurrent.TimeUnit;
import java.util.stream.Collectors;

public class ExportEndpoints {

    public static void main(String[] args) {

        final String USAGE = "\n" +
                "ExportEndpoints - Downloads endpoints from an Amazon Pinpoint application by: \n" +
                "1.) Exporting the endpoint definitions to an Amazon S3 bucket. \n" +
                "2.) Downloading the endpoint definitions to the specified file path.\n\n" +

                "Usage: ExportEndpoints <s3BucketName> <iamExportRoleArn> <downloadDirectory> " +
                "<applicationId>\n\n" +

                "Where:\n" +
                "  s3BucketName - The name of the Amazon S3 bucket to export the endpoints files " +
                "to. If the bucket doesn't exist, a new bucket is created.\n" +
                "  iamExportRoleArn - The ARN of an IAM role that grants Amazon Pinpoint write " +
                "permissions to the S3 bucket.\n" +
                "  downloadDirectory - The directory to download the endpoints files to.\n" +
                "  applicationId - The ID of the Amazon Pinpoint application that has the " +
                "endpoints.";

        if (args.length < 1) {
            System.out.println(USAGE);
            System.exit(1);
        }

        String s3BucketName = args[0];
        String iamExportRoleArn = args[1];
        String downloadDirectory = args[2];
        String applicationId = args[3];

        // Exports the endpoints to Amazon S3 and stores the keys of the new objects.
        List<String> objectKeys =
                exportEndpointsToS3(s3BucketName, iamExportRoleArn, applicationId);

        // Filters the keys to only those objects that have the endpoint definitions.
        // These objects have the .gz extension.
        List<String> endpointFileKeys = objectKeys
                .stream()
                .filter(o -> o.endsWith(".gz"))
                .collect(Collectors.toList());

        // Downloads the exported endpoints files to the specified directory.
        downloadFromS3(s3BucketName, endpointFileKeys, downloadDirectory);
    }

    public static List<String> exportEndpointsToS3(String s3BucketName, String iamExportRoleArn,
                                                   String applicationId) {

        // The S3 path that Amazon Pinpoint exports the endpoints to.
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd-HH_mm:ss.SSS_z");
        String endpointsKeyPrefix = "exports/" + applicationId + "_" + dateFormat.format(new Date
                ());
        String s3UrlPrefix = "s3://" + s3BucketName + "/" + endpointsKeyPrefix + "/";

        // Defines the export job that Amazon Pinpoint runs.
        ExportJobRequest exportJobRequest = new ExportJobRequest()
                .withS3UrlPrefix(s3UrlPrefix)
                .withRoleArn(iamExportRoleArn);
        CreateExportJobRequest createExportJobRequest = new CreateExportJobRequest()
                .withApplicationId(applicationId)
                .withExportJobRequest(exportJobRequest);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        System.out.format("Exporting endpoints from Amazon Pinpoint application %s to Amazon S3 " +
                "bucket %s . . .\n", applicationId, s3BucketName);

        List<String> objectKeys = null;

        try {
            // Runs the export job with Amazon Pinpoint.
            CreateExportJobResult exportResult =
                    pinpointClient.createExportJob(createExportJobRequest);

            // Prints the export job status to the console while the job runs.
            String jobId = exportResult.getExportJobResponse().getId();
            printExportJobStatus(pinpointClient, applicationId, jobId);

            // Initializes the Amazon S3 client.
            AmazonS3 s3Client = AmazonS3ClientBuilder.defaultClient();

            // Lists the objects created by Amazon Pinpoint.
            objectKeys = s3Client
                    .listObjectsV2(s3BucketName, endpointsKeyPrefix)
                    .getObjectSummaries()
                    .stream()
                    .map(S3ObjectSummary::getKey)
                    .collect(Collectors.toList());

        } catch (AmazonServiceException e) {
            System.err.println(e.getMessage());
            System.exit(1);
        }

        return objectKeys;
    }

    private static void printExportJobStatus(AmazonPinpoint pinpointClient,
                                             String applicationId, String jobId) {

        GetExportJobResult getExportJobResult;
        String jobStatus;

        try {
            // Checks the job status until the job completes or fails.
            do {
                getExportJobResult = pinpointClient.getExportJob(new GetExportJobRequest()
                        .withJobId(jobId)
                        .withApplicationId(applicationId));
                jobStatus = getExportJobResult.getExportJobResponse().getJobStatus();
                System.out.format("Export job %s . . .\n", jobStatus.toLowerCase());
                TimeUnit.SECONDS.sleep(3);
            } while (!jobStatus.equals("COMPLETED") && !jobStatus.equals("FAILED"));

            if (jobStatus.equals("COMPLETED")) {
                System.out.println("Finished exporting endpoints.");
            } else {
                System.err.println("Failed to export endpoints.");
                System.exit(1);
            }

            // Checks for entries that failed to import.
            // getFailures provides up to 100 of the first failed entries for the job, if any exist.
            List<String> failedEndpoints = getExportJobResult.getExportJobResponse().getFailures();
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

    public static void downloadFromS3(String s3BucketName, List<String> objectKeys,
                                      String downloadDirectory) {

        // Initializes the Amazon S3 client.
        AmazonS3 s3Client = AmazonS3ClientBuilder.defaultClient();

        try {
            // Downloads each object to the specified file path.
            for (String key : objectKeys) {
                S3Object object = s3Client.getObject(s3BucketName, key);
                String endpointsFileName = key.substring(key.lastIndexOf("/"));
                Path filePath = Paths.get(downloadDirectory + endpointsFileName);

                System.out.format("Downloading %s to %s . . .\n",
                        filePath.getFileName(), filePath.getParent());

                writeObjectToFile(filePath, object);
            }
            System.out.println("Download finished.");
        } catch (AmazonServiceException | NullPointerException e) {
            System.err.println(e.getMessage());
            System.exit(1);
        }

    }

    private static void writeObjectToFile(Path filePath, S3Object object) {

        // Writes the contents of the S3 object to a file.
        File endpointsFile = new File(filePath.toAbsolutePath().toString());
        try (FileOutputStream fos = new FileOutputStream(endpointsFile);
             S3ObjectInputStream s3is = object.getObjectContent()) {
            byte[] read_buf = new byte[1024];
            int read_len = 0;
            while ((read_len = s3is.read(read_buf)) > 0) {
                fos.write(read_buf, 0, read_len);
            }
        } catch (IOException e) {
            System.err.println(e.getMessage());
            System.exit(1);
        }
    }
}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example POST Export Job Request**  
To export the endpoints in your Amazon Pinpoint project, issue a `POST` request to the [Export Jobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html) resource:  

```
POST /v1/apps/application_id/jobs/export HTTP/1.1
Content-Type: application/json
Accept: application/json
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180606T001238Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180606/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;cache-control;content-length;content-type;host;postman-token;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{
  "S3UrlPrefix": "s3://bucket-name/prefix",
  "RoleArn": "iam-export-role-arn"
}
```
Where:  
+ *`application-id`* is the ID of the Amazon Pinpoint project that contains the endpoints\.
+ *`bucket-name/prefix`* is the name of your Amazon S3 bucket and, optionally, a prefix that helps you organize the objects in your bucket hierarchically\. For example, a useful prefix might be `pinpoint/exports/endpoints/`\.
+ *`iam-export-role-arn`* is the Amazon Resource Name \(ARN\) of an IAM role that grants Amazon Pinpoint write access to the bucket\.
The response to this request provides details about the export job:  

```
{
    "Id": "611bdc54c75244bfa51fe7001ddb2e36",
    "JobStatus": "CREATED",
    "CreationDate": "2018-06-06T00:12:43.271Z",
    "Type": "EXPORT",
    "Definition": {
        "S3UrlPrefix": "s3://bucket-name/prefix",
        "RoleArn": "iam-export-role-arn"
    }
}
```
The response provides the job ID with the `Id` attribute\. You can use this ID to check the current status of the export job\.

**Example GET Export Job Request**  
To check the current status of an export job, issue a `GET` request to the [Export Job](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-job.html) resource:  

```
GET /v1/apps/application_id/jobs/export/job_id HTTP/1.1
Content-Type: application/json
Accept: application/json
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180606T002443Z
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180606/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;cache-control;content-type;host;postman-token;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache
```
Where:  
+ *`application-id`* is the ID the Amazon Pinpoint project that you exported the endpoints from\.
+ *`job-id`* is the ID of the job that you're checking\.
The response to this request provides the current state of the export job:  

```
{
    "ApplicationId": "application_id",
    "Id": "job_id",
    "JobStatus": "COMPLETED",
    "CompletedPieces": 1,
    "FailedPieces": 0,
    "TotalPieces": 1,
    "CreationDate": "2018-06-06T00:12:43.271Z",
    "CompletionDate": "2018-06-06T00:13:01.141Z",
    "Type": "EXPORT",
    "TotalFailures": 0,
    "TotalProcessed": 217,
    "Definition": {}
}
```
The response provides the job status with the `JobStatus` attribute\. When the job status value is `COMPLETED`, you can get your exported endpoints from your Amazon S3 bucket\.

------

## Related Information<a name="audience-data-export-related"></a>

For more information about the Export Jobs resource in the Amazon Pinpoint API, including the supported HTTP methods and request parameters, see [Export Jobs](https://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-export-jobs.html) in the *Amazon Pinpoint API Reference*\.
# Importing Segments<a name="segments-importing"></a>

With Amazon Pinpoint, you can define a user segment by importing information about the endpoints that belong to the segment\. An *endpoint* is a single messaging destination, such as a mobile push device token, a mobile phone number, or an email address\.

Importing segments is useful if you've already created segments of your users outside of Amazon Pinpoint but you want to engage your users with Amazon Pinpoint campaigns\.

When you import a segment, Amazon Pinpoint gets the segment's endpoints from Amazon Simple Storage Service \(Amazon S3\)\. Before you import, you add the endpoints to Amazon S3, and you create an IAM role that grants Amazon Pinpoint access to Amazon S3\. Then, you give Amazon Pinpoint the Amazon S3 location where the endpoints are stored, and Amazon Pinpoint adds each endpoint to the segment\.

To create the IAM role, see [IAM Role for Importing Segments](permissions-import.md)\. For information about importing a segment by using the Amazon Pinpoint console, see [Importing Segments](http://docs.aws.amazon.com/pinpoint/latest/userguide/segments-importing.html) in the *Amazon Pinpoint User Guide*\.

## Importing a Segment<a name="segments-importing-example-java"></a>

The following example demonstrates how to import a segment by using the AWS SDK for Java\.

```
import com.amazonaws.services.pinpoint.AmazonPinpointClient;
import com.amazonaws.services.pinpoint.model.AttributeDimension;
import com.amazonaws.services.pinpoint.model.AttributeType;
import com.amazonaws.services.pinpoint.model.CreateImportJobRequest;
import com.amazonaws.services.pinpoint.model.CreateImportJobResult;
import com.amazonaws.services.pinpoint.model.CreateSegmentRequest;
import com.amazonaws.services.pinpoint.model.CreateSegmentResult;
import com.amazonaws.services.pinpoint.model.Format;
import com.amazonaws.services.pinpoint.model.GetImportJobRequest;
import com.amazonaws.services.pinpoint.model.GetImportJobResult;
import com.amazonaws.services.pinpoint.model.GetSegmentRequest;
import com.amazonaws.services.pinpoint.model.GetSegmentResult;
import com.amazonaws.services.pinpoint.model.ImportJobRequest;
import com.amazonaws.services.pinpoint.model.JobStatus;
import com.amazonaws.services.pinpoint.model.RecencyDimension;
import com.amazonaws.services.pinpoint.model.SegmentBehaviors;
import com.amazonaws.services.pinpoint.model.SegmentDemographics;
import com.amazonaws.services.pinpoint.model.SegmentDimensions;
import com.amazonaws.services.pinpoint.model.SegmentLocation;
import com.amazonaws.services.pinpoint.model.SegmentResponse;
import com.amazonaws.services.pinpoint.model.WriteSegmentRequest;

import java.util.HashMap;
import java.util.Map;

public class PinpointImportSample {

    public SegmentResponse createImportSegment(AmazonPinpointClient client, String appId,
                                               String bucket, String key,
                                               String roleArn) throws Exception {

        // Create the job.
        ImportJobRequest importRequest = new ImportJobRequest()
                .withDefineSegment(true)
                .withRegisterEndpoints(true)
                .withRoleArn(roleArn)
                .withFormat(Format.JSON)
                .withS3Url("s3://" + bucket + "/" + key);

        CreateImportJobRequest jobRequest = new CreateImportJobRequest()
                .withImportJobRequest(importRequest)
                .withApplicationId(appId);

        CreateImportJobResult jobResponse = client.createImportJob(jobRequest);

        GetImportJobRequest fetchRequest = new GetImportJobRequest()
                .withApplicationId(appId)
                .withJobId(jobResponse.getImportJobResponse().getId());

        // Wait for job to finish.

        GetImportJobResult getJobResponse;

        do {
            // Lets only check once every 10 seconds if done, busy waits are bad.

            Thread.sleep(10 * 1000);

            getJobResponse = client.getImportJob(fetchRequest);

            if (getJobResponse.getImportJobResponse().getJobStatus().equals(JobStatus.FAILED)) {
                throw new Exception("Failed to process import job successfully");
            }
        } while(!getJobResponse.getImportJobResponse().getJobStatus().equals(JobStatus.COMPLETED));

        // Finally get the import segment that was created.

        GetSegmentRequest segmentRequest = new GetSegmentRequest()
                .withApplicationId(appId)
                .withSegmentId(getJobResponse.getImportJobResponse().getId());

        GetSegmentResult getSegmentResult = client.getSegment(segmentRequest);

        // Print out what we got

        System.out.println("Segment ID: " + getSegmentResult.getSegmentResponse().getId()
                + " with size " + getSegmentResult.getSegmentResponse().getImportDefinition().getSize());

        return getSegmentResult.getSegmentResponse();
    }

}
```
# Step 7: Set up Amazon S3 events<a name="tutorials-importing-data-s3-events"></a>

In this section, you set up your Amazon S3 bucket so that it triggers your Lambda functions when you add files to the folders in the bucket\. You also test the entire function to make sure that the triggers work as expected\.

By using event triggers in Amazon S3, you make the process of executing the Lambda functions automatic\. When you upload a file to the `input` folder, Amazon S3 automatically sends a notification to the `CustomerImport_ReadIncomingAndSplit` function\. When that function runs, it sends files to the `to_process` folder\. When files are added to the `to_process` folder, Amazon S3 triggers the `CustomerImport_ProcessInput` function\. When that function runs, it adds files to the `processed` folder, which triggers the `CustomerImport_CreateJob` function\.

## Step 7\.1: Set up event notifications<a name="tutorials-importing-data-s3-events-setup"></a>

In this section, you configure three event notifications for your Amazon S3 bucket\. These notifications automatically trigger your Lambda functions when files are added to specific folders in your bucket\.

**To configure the event triggers**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, choose the bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md)\.

1. On the **Properties** tab, choose **Events**\. Next, do the following:
   + Choose **Add notification**, as shown in the following image\.
   + For **Name**, enter **SplitInput**\.
   + For **Events**, choose **PUT**\.
   + For **Prefix**, enter **input/**\.
   + For **Suffix**, enter **\.csv**\.
   + For **Send to**, choose **Lambda Function**\.
   + For **Lambda**, choose **CustomerImport\_ReadIncomingAndSplit**\.

     When you finish, the **Events** section resembles the example that's shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Data_Importer_Tutorial_S3_Event1.png)
   + Choose **Save**\.

1. Choose **Events**, and then choose **Add notification** again\. Do the following:
   + Choose **Add notification**, as shown in the following image\.
   + For **Name**, enter **ProcessInput**\.
   + For **Events**, choose **PUT**\.
   + For **Prefix**, enter **to\_process/**\.
   + For **Suffix**, enter **\.csv**\.
   + For **Send to**, choose **Lambda Function**\.
   + For **Lambda**, choose **CustomerImport\_ProcessInput**\.
   + Choose **Save**\.

1. Choose **Events**, and then choose **Add notification** again\. Do the following:
   + Choose **Add notification**, as shown in the following image\.
   + For **Name**, enter **CreateImportJob**\.
   + For **Events**, choose **PUT**\.
   + For **Prefix**, enter **processed/**\.
   + For **Suffix**, enter **\.csv**\.
   + For **Send to**, choose **Lambda Function**\.
   + For **Lambda**, choose **CustomerImport\_CreateJob**\.
   + Choose **Save**\.

## Step 7\.2: Test the triggers<a name="tutorials-importing-data-s3-events-test"></a>

The last step in setting up the solution that's discussed in this tutorial is to upload a file to the `input` folder in your Amazon S3 bucket\. Uploading a file triggers the Lambda function that you created in [Step 4](tutorials-importing-data-lambda-function-input-split.md)\. When this function finishes running, it creates files in the other folders that you configured event notifications for in the preceding section\. After a few minutes, the entire sequence of Lambda functions finishes running, and your Amazon Pinpoint project contains a new segment\.

**To test the event triggers**

1. On your computer, locate the `testfile.csv` file that you created in [Step 4\.2](tutorials-importing-data-lambda-function-input-split.md#tutorials-importing-data-lambda-function-input-split-test)\. Change the name of the file to `testfile1.csv`\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, choose the bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md), and then choose the `input` folder\.

1. Upload `testfile1.csv` to the `input` folder\. After the file finishes uploading, wait for several minutes\.

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the list of projects, choose the project that you're importing segments into\.

1. In the navigation pane, choose **Segments**\. On the **Segments** tab, look for a segment named **testfile1**\. If the segment isn't listed, check the **Scheduled imports** tab to see if the import job is still in progress\.

   If you don't see a new segment on either tab, wait a few minutes longer, and then refresh the **Segments** page\. If you still don't see the segment, do the following:
   + Make sure that the Amazon S3 notification events are configured exactly as described in the preceding section\.
   + Check the logs for all three of the Lambda functions in CloudWatch Logs\. If any of the functions ended in error, fix the issues that caused the error\.

**Next**: [Next Steps](tutorials-importing-data-next-steps.md)
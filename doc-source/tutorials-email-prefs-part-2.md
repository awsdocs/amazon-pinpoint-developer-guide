# Step 2: Add or configure endpoints<a name="tutorials-email-prefs-part-2"></a>

When you send campaigns in Amazon Pinpoint, you send them to endpoints\. An endpoint represents a single method of contacting a customer\. For example, a customer's phone number, their email address, and their unique Apple Push Notification Service \(APNs\) token are three separate endpoints\. In this example, these three endpoints represent three different ways of communicating with a customer—in this case, by sending SMS messages, emails, or push notifications\.

If you're new to Amazon Pinpoint, your Amazon Pinpoint project doesn't contain any endpoints\. There are a few ways that you can add endpoints to Amazon Pinpoint:
+ Import from a CSV or JSON file\.
+ Use the UpdateEndpoint API operation in the Amazon Pinpoint API\.
+ Export event data from an app that uses the AWS Mobile SDK or the AWS Amplify JavaScript library to send analytics data to Amazon Pinpoint\.

To complete this tutorial, your Amazon Pinpoint project has to contain at least one email endpoint\. If your Amazon Pinpoint project already contains email endpoints, you can use those\. You can also use the sample CSV file in this section to import a list of test endpoints\.

## Import test endpoints<a name="tutorials-email-prefs-part-2-import"></a>

This section includes a file that you can use as a source file for importing a test endpoint into Amazon Pinpoint\. This method of adding endpoints is helpful if you don't have an application that sends endpoint information to Amazon Pinpoint, if you don't want to use the Amazon Pinpoint API to create endpoints, or if you don't want to modify the endpoints that already exist in your Amazon Pinpoint project\.

### Create the source file<a name="tutorials-email-prefs-part-2-import-create"></a>

First, create the CSV file that contains information about your endpoints\.

**To create the CSV file**

1. In a text editor, create a new file\.

1. Paste the following text into the file\.

   ```
   ChannelType,Address,EndpointStatus,OptOut,Id,Attributes.Email,Attributes.EndpointId,User.UserAttributes.FirstName,User.UserAttributes.LastName
   EMAIL,recipient@example.com,ACTIVE,NONE,12345,recipient%40example.com,12345,Carlos,Salazar
   ```

1. In the text from the previous step, make the following changes:
   + Replace *recipient@example\.com* with your email address\.
   + Replace *recipient%40example\.com* with your email address, but use `%40` instead of the at sign \(@\)\.
   + Replace *Carlos* and *Salazar* with your first and last name, respectively\.
   + \(Optional\) Replace both occurrences of *12345* with an endpoint ID value\. The value that you use has to be exactly the same in both locations\.

1. Save the file as `testImports.csv`\.

### Upload the file<a name="tutorials-email-prefs-part-2-import-upload"></a>

Amazon Pinpoint requires you to store source files in Amazon S3\. Complete the procedure below to upload your CSV file to a folder in an Amazon S3 bucket\.

**To upload the file to an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. On the **Create bucket** window, for **Bucket name**, enter a name for the bucket that you want to store the file in\. The name that you specify has to be unique\. Also, there are several restrictions related to the bucket name\. For more information about these restrictions, see [Bucket restrictions](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\. Choose **Create**\.

1. In the list of buckets, choose the bucket that you just created\.

1. Choose **Create folder**\. Name the folder `Imports`\. Choose **Save**\.

1. Choose the **Imports** folder that you just created\.

1. Choose **Upload**\.

1. Choose **Add files**\. Locate the `testImports.csv` file that you created in the [previous section](#tutorials-email-prefs-part-2-import-create)\. Choose **Upload**\.

### Import the file into Amazon Pinpoint<a name="tutorials-email-prefs-part-2-import-segment"></a>

After you upload your CSV file into an Amazon S3 bucket, you can import it into your Amazon Pinpoint project\.

**To import the file into Amazon Pinpoint**

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\.

1. Choose **Create a segment**\.

1. On the **Create a segment** page, choose **Import a segment**\.

1. Under **Specifications**, do the following:
   + For **Name**, enter `EmailRegistrationTestRecipients`\.
   + For **Amazon Simple Storage Service URL**, enter the address of the Amazon S3 bucket and folder that you created in the previous section\. For example, if your bucket is named email\-registration\-tutorial, enter the following URL: `s3://email-registration-tutorial/Imports`\.
   + Under **IAM role**, choose **Automatically create a role**\. Then, enter a name for the IAM role, such as `SegmentImportRole`\.
   + Under **What type of file are you importing**, choose **Comma\-Separated Values**\. Choose **Create segment**\.

1. After you submit the segment, you see the **Scheduled imports** tab on the **Segments** page\. Wait for 30 seconds after you submit the segment, and then refresh the page\. The **Import status** column should indicate that the segment import is complete\. If it indicates that the import is still pending, wait an additional 30 seconds, and then refresh the page again\. Repeat this process until the status of the import is **Completed**\.

1. On the **Segments** tab, choose the **EmailRegistrationTestRecipients** segment\. In the **Import details** section, confirm that the value under **Number of records** is accurate\. If you used the sample CSV file in this section, the correct value is **1**\.

## Use existing endpoints \(for advanced users\)<a name="tutorials-email-prefs-part-2-use-existing"></a>

If your Amazon Pinpoint project already contains email endpoints, you have to modify those endpoints slightly before you can use them in this tutorial\. First, the endpoints have to include two endpoint Attribute values\. These values are listed in the following table\.


| Attribute name | Value | 
| --- | --- | 
|  `Attributes.Email`  |  A URL\-encoded version of the existing `Address` attribute for the endpoint\.  | 
|  `Attributes.EndpointId`  |  Equal to the existing `Id` attribute for the endpoint\. If the endpoint ID value contains non\-alphanumeric characters, then you should URL\-encode this value\.  For more information about URL encoding, see the [HTML URL encoding reference](https://www.w3schools.com/tags/ref_urlencode.asp) on the W3Schools website\.   | 

The endpoints should also contain two User Attribute values\. These values aren't strictly required for the solution to work\. These values are listed in the following table\.


| Attribute name | Value | 
| --- | --- | 
| User\.UserAttributes\.FirstName | The recipient's first name\. | 
| User\.UserAttributes\.LastName | The recipient's last name\. | 

If your Amazon Pinpoint project contains a large number of existing endpoints, you can use the [CreateExportJob](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-jobs-export.html#CreateExportJob) API operation to export a list of all the endpoints for a segment or project to an Amazon S3 bucket\. After that, you can write a script that uses the [UpdateEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#UpdateEndpoint) operation to programmatically updates endpoints to include these attributes\.

**Next**: [Create IAM Policies and Roles](tutorials-email-prefs-part-3.md)
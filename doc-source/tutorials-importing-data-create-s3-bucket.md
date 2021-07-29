# Step 1: Create an Amazon S3 bucket<a name="tutorials-importing-data-create-s3-bucket"></a>

In this solution, you upload files that you want to import into an `input` folder in an Amazon S3 bucket\. When you upload a file into this folder, Amazon S3 triggers a Lambda function\. This function moves the input file into an `archive` folder\. It also creates several smaller files in a `to_process` folder\. The first step in implementing this solution is to create an Amazon S3 bucket\. Next, you create an `input` folder in that bucket\.

## Step 1\.1: Create an Amazon S3 bucket and input folder<a name="tutorials-importing-data-create-s3-bucket-new-bucket"></a>

Complete the following procedure to create a new Amazon S3 bucket that contains a folder named `input`\.

**To create an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. For **Bucket name**, enter a unique name for the bucket, as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Data_Importer_Tutorial_Create_Bucket.png)
**Tip**  
The name that you specify has to meet all of the following requirements:  
It has to be unique across all of AWS\.
It has to contain at least 3 characters, and no more than 63 characters\. 
It can only contain lowercase ASCII letters \(a–z\), numbers \(0–9\), periods \(\.\), and dashes \(\-\)\.
The first character in the bucket name has to be a letter or a number\.
The name of the bucket can't be formatted like an IP address \(such as 192\.0\.2\.0\)\.

1. Choose **Create**\.

1. In the list of buckets, choose the bucket that you just created\.

1. Choose **Create folder**\.

1. For the folder name, enter `input`, as shown in the following image\. Choose **Save**\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Data_Importer_Tutorial_Create_Folder.png)

**Next**: [Create IAM Roles](tutorials-importing-data-create-iam-roles.md)
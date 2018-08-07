# Deleting Data from Amazon Pinpoint<a name="deleting-data"></a>

Depending on how you use it, Amazon Pinpoint might store certain data that could be considered personal\. For example, each endpoint in Amazon Pinpoint contains contact information for an end user, such as that person's email address or mobile phone number\.

You can use the Amazon Pinpoint API to permanently delete personal data\. This section includes procedures for deleting various types of data that could be considered personal\.

## Deleting Endpoints<a name="deleting-data-endpoints"></a>

An endpoint represents a single method of contacting one of your customers\. Each endpoint can refer to a customer's email address, mobile device identifier, or email address\. In many jurisdictions, this type of information might be considered personal\.

The procedure in this section uses the AWS CLI to interact with the Amazon Pinpoint API\. This procedure assumes that you've already installed and configured the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

**To delete an endpoint by using the AWS CLI**
+ At the command line, type the following command:

  ```
  aws pinpoint delete-endpoint --application-id example1884c7d659a2feaa0c5 --endpoint-id ad015a3bf4f1b2b0b82example
  ```

  In the preceding command, replace *example1884c7d659a2feaa0c5* with the ID of the application or project that the endpoint is located in\. Also, replace *ad015a3bf4f1b2b0b82example* with the unique ID of the endpoint itself\.

## Deleting Segment and Endpoint Data Stored in Amazon S3<a name="deleting-data-stored-in-s3"></a>

You can import segments from a file that's stored in an Amazon S3 bucket by using the Amazon Pinpoint console or the API\. You can also export application, segment, or endpoint data from Amazon Pinpoint to an Amazon S3 bucket\. Both the imported and exported files can contain personal data, including email addresses, mobile phone numbers, and information about the physical location of the endpoint\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

## Delete All AWS Data by Closing Your AWS Account<a name="deleting-data-close-account"></a>

It's also possible to delete all data that you've stored in Amazon Pinpoint by closing your AWS account\. However, this action also deletes all other data—personal or non\-personal—that you've stored in every other AWS service\.

When you close your AWS account, we retain the data in your AWS account for 90 days\. At the end of this retention period, we delete this data permanently and irreversibly\.

**Warning**  
The following procedure completely removes all data that's stored in your AWS account across all AWS services and regions\.

You can close your AWS account by using the AWS Management Console\.

**To close your AWS account**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com)\.

1. Go to the **Account Settings** page at [https://console\.aws\.amazon\.com/billing/home?\#/account](https://console.aws.amazon.com/billing/home?#/account)\.
**Warning**  
The following two steps permanently delete all of the data you've stored in all AWS services across all AWS Regions\.

1. Under **Close Account**, read the disclaimer that describes the consequences of closing your AWS account\. If you agree to the terms, select the check box, and then choose **Close Account**\.

1. On the confirmation dialog box, choose **Close Account**\.
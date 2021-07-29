# Deleting data from Amazon Pinpoint<a name="deleting-data"></a>

Depending on how you use it, Amazon Pinpoint might store certain data that could be considered personal\. For example, an endpoint in Amazon Pinpoint contains contact information for an end user, such as that person's email address or mobile phone number\.

You can use the console or the Amazon Pinpoint API to permanently delete personal data\. This topic includes procedures for deleting various types of data that could be considered personal\.

## Deleting endpoints<a name="deleting-data-endpoints"></a>

An endpoint represents a single method of contacting one of your customers\. Each endpoint can refer to a customer's email address, mobile device identifier, phone number, or other type of destination that you can send messages to\. In many jurisdictions, this type of information might be considered personal\.

To delete all the data for a specific endpoint, you can use the Amazon Pinpoint API to delete the endpoint\. The following procedure shows how to delete an endpoint by using the AWS CLI to interact with the Amazon Pinpoint API\. This procedure assumes that you've already installed and configured the AWS CLI\. For more information, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

**To delete an endpoint by using the AWS CLI**
+ At the command line, enter the following command:

  ```
  aws pinpoint delete-endpoint --application-id 810c7aab86d42fb2b56c8c966example --endpoint-id ad015a3bf4f1b2b0b82example
  ```

  In the preceding command, replace *810c7aab86d42fb2b56c8c966example* with the ID of the project that the endpoint is associated with\. Also, replace *ad015a3bf4f1b2b0b82example* with the unique ID of the endpoint itself\.

To find the endpoint ID for a specific endpoint, determine which segment the endpoint belongs to, and then export the segment from Amazon Pinpoint\. The exported data includes the endpoint ID for each endpoint\. You can export a segment to a file by using the Amazon Pinpoint console\. To learn how, see [Exporting Segments](https://docs.aws.amazon.com/pinpoint/latest/userguide/segments-exporting.html) in the *Amazon Pinpoint User Guide*\. You can also export a segment to an Amazon Simple Storage Service \(Amazon S3\) bucket by using the Amazon Pinpoint API\. To learn how, see [Exporting Endpoints](audience-data-export.md) in this guide\.

## Deleting segment and endpoint data stored in Amazon S3<a name="deleting-data-stored-in-s3"></a>

You can import segments from a file that's stored in an Amazon S3 bucket by using the Amazon Pinpoint console or the API\. You can also export application, segment, or endpoint data from Amazon Pinpoint to an Amazon S3 bucket\. Both the imported and exported files can contain personal data, including email addresses, mobile phone numbers, and information about the physical location of an endpoint\. You can delete these files from Amazon S3\.

Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

## Deleting all project data<a name="deleting-data-delete-project"></a>

It's possible to permanently delete all the data that you've stored for an Amazon Pinpoint project\. You can do this by deleting the project\.

**Warning**  
If you delete a project, Amazon Pinpoint deletes all project\-specific settings and data for the project\. The information can't be recovered\.

When you delete a project, Amazon Pinpoint deletes all project\-specific settings for the push notification and two\-way SMS messaging channels, and all segments, campaigns, journeys, and project\-specific analytics data that's stored in Amazon Pinpoint, such as the following:
+ Segments – All segment settings and data\. For dynamic segments, this includes segment groups and filters that you defined\. For imported segments, this includes endpoints, user IDs, and other data that you imported, and any filters that you applied\.
+ Campaigns – All messages, message treatments and variables, analytics data, schedules, and other settings\.
+ Journeys – All activities, analytics data, schedules, and other settings\.
+ Analytics – Data for all engagement metrics, such as the number of messages sent and delivered for campaigns and journeys, and all journey execution metrics\. For mobile and web apps, all event data that wasn’t streamed to another AWS service such as Amazon Kinesis, all funnels, and data for application usage, revenue, and demographic metrics\. Before you delete a project, we recommend that you export this data to another location\.

You can delete a project by using the Amazon Pinpoint console\. To learn more, see [Deleting a Project](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-general.html#settings-general-delete-project) in the *Amazon Pinpoint User Guide*\. You can also delete a project programmatically by using the [App](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id.html) resource of the Amazon Pinpoint API\.

## Deleting all AWS data by closing your AWS account<a name="deleting-data-close-account"></a>

It's also possible to delete all the data that you've stored in Amazon Pinpoint by closing your AWS account\. However, this action also deletes all other data—personal or non\-personal—that you've stored in every other AWS service\.

When you close your AWS account, we retain the data in your AWS account for 90 days\. At the end of this retention period, we delete this data permanently and irreversibly\.

**Warning**  
The following procedure completely removes all data that's stored in your AWS account across all AWS services and AWS Regions\.

You can close your AWS account by using the AWS Management Console\.

**To close your AWS account**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com)\.

1. Go to the **Account Settings** page at [https://console\.aws\.amazon\.com/billing/home?\#/account](https://console.aws.amazon.com/billing/home?#/account)\.
**Warning**  
The following steps permanently delete all the data that you've stored in all AWS services across all AWS Regions\.

1. Under **Close Account**, read the disclaimer that describes the consequences of closing your AWS account\. If you agree to the terms, select the check box, and then choose **Close Account**\.

1. On the confirmation dialog box, choose **Close Account**\.
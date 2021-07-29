# Tutorial: Importing data into Amazon Pinpoint from external sources<a name="tutorials-importing-data"></a>

Before you can create segments and send campaign messages, you have to create endpoints\. In Amazon Pinpoint, *endpoints* are destinations that you send messages to—such as email addresses, mobile device identifiers, or mobile phone numbers\. There are several ways to add endpoints to Amazon Pinpoint\. For example, your web or mobile apps can use an AWS Mobile SDK to automatically write endpoint data to Amazon Pinpoint\.

Alternatively, you can import lists of existing customers into Amazon Pinpoint by using the console, or by using the [CreateImportJob API operation](Amazon Pinpoint API Referencerest-api-import-jobs.html#rest-api-import-jobs-methods-post)\. However, when you create an import job, the files that you import have to be formatted in a way that Amazon Pinpoint can interpret\. Additionally, if the data that you want to import includes multiple contact methods \(such as email addresses and phone numbers\) for each customer, you have to create separate endpoints for each contact method\. You can then unite those endpoints by applying a common user ID attribute to each one\.

The solution that's described in this tutorial is intended to simplify the process of bringing customer information into Amazon Pinpoint from an external system, such as Salesforce, Segment, Braze, or Adobe Marketing Cloud\. This tutorial includes a small sample file that's based on data that was originally exported from Salesforce\.

**Note**  
AWS and Amazon Pinpoint aren't associated with any of the products or services that are mentioned in the preceding paragraph\. To learn more about exporting data from these systems, see their official documentation:  
Salesforce: See [Creating a custom report](https://help.salesforce.com/articleView?id=reports_custom.htm) on the Salesforce website\.
Segment: See [What are my data export options?](https://segment.com/docs/guides/destinations/what-are-my-data-export-options/) on the Segment website\.
Braze: See [Exporting to CSV](https://www.braze.com/docs/user_guide/data_and_analytics/export_braze_data/segment_data_to_csv/) in the User Guide on the Braze website\.
Adobe Marketing Cloud: See [Exporting data using segment export](https://marketing.adobe.com/resources/help/en_US/insight/client/c_exp_data_seg_exp.html) on the Adobe Experience Cloud Help website\.

After you implement this solution, you can easily send customer data to Amazon Pinpoint by uploading your source file to a specific Amazon S3 bucket\. When you add a new source file to the Amazon S3 bucket, Amazon S3 triggers an AWS Lambda function\. This function splits the source file into several smaller files, and then moves these files to a second Amazon S3 bucket\. When files are added to the second Amazon S3 bucket, it triggers another Lambda function\.

The second function processes each of the smaller input files by doing the following:
+ It creates a separate record for each endpoint \(email address, phone number\) that it finds in the file\.
+ It changes the headers in the incoming spreadsheet to the record names that Amazon Pinpoint expects\.
+ It uses the phone number validation feature of Amazon Pinpoint to properly format the phone numbers that it finds\. If the phone number validation service discovers that a phone number can receive SMS messages, then it creates the endpoint as an SMS endpoint\. Otherwise, it creates the endpoint as a voice endpoint\.
+ It changes some data to use the format that Amazon Pinpoint expects\. For example, Amazon Pinpoint expects you to specify country names in ISO 3166 alpha\-2 format\. If your input includes country names, this function automatically converts them to the correct format\. Additionally, Amazon Pinpoint expects you to specify phone numbers in E\.164 format\. It obtains this value from the phone number validation step\.
+ It stores the processed files in a third Amazon S3 bucket\.

Because this function only processes a small section of your data, it can run quickly each time it's invoked\. Also, each instance of the function runs concurrently, which makes it possible to process large amounts of data in a relatively short amount of time\.

When files are added to the third and final Amazon S3 bucket, an additional Lambda function is triggered\. This function takes the processed files and uses them to create import jobs in Amazon Pinpoint\. When these import jobs are complete, you can start sending campaigns to the imported endpoints\.

The following diagram shows the flow of data in this solution\.

![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Customer_Import_Tutorial_Architecture.png)

**Intended audience**  
This tutorial is intended for developers and system implementers\. You don't have to be familiar with Amazon Pinpoint to complete the steps in this tutorial\.

To implement the solution that's described in this tutorial, you have to install Python and the PIP package manager on your computer\. This enables you to download some of the required packages\. This tutorial includes all of the code that you need in order to complete it\. However, to make this solution work for your specific use case, you might have to modify some of the Python code that's included in the tutorial\. Finally, you have to be able to create new IAM policies and users\.

**Features used**  
This tutorial shows you how to import customer data by using custom Lambda functions to process your data\. In addition to modifying the format of some of the data in your source files, the solution described in this tutorial also uses the phone number validation service that's built into Amazon Pinpoint\.

After the data is processed, a Lambda function imports your customer data into Amazon Pinpoint by using the `CreateImportJob` API operation\.

**Time required**  
It should take 60–90 minutes to complete the procedures in this tutorial\. However, you should plan to spend additional time customizing this solution to fit your unique use case\.

**Regional restrictions**  
There are no regional restrictions associated with using this solution\.

**Resource usage costs**  
There's no charge for creating an AWS account\. However, by implementing this solution, you might incur some or all of the costs that are listed in the following table\.


| Description | Cost \(US dollars\) | 
| --- | --- | 
|  Lambda usage  |  This solution uses three Lambda functions that interact with the Amazon Pinpoint API\. When you call a Lambda function, you're charged based on the number of requests for your functions, for the time it takes your code to execute, and for the amount of memory that your functions use\. The number of requests, amount of compute time, and amount of memory required depends on the number of records that you're importing\. Your usage of Lambda in implementing this solution might be included in the AWS Free Tier\. For more information, see [AWS Lambda pricing](https://aws.amazon.com/lambda/pricing/)\.  | 
|  Amazon S3 usage  |  You pay $0\.023–0\.025 per GB of data that you store in Amazon S3, depending on which AWS Region you use\. You also pay $0\.005–0\.0055, depending on the Region, for every 1,000 `PUT` requests that you make to Amazon S3\. Your usage of Amazon S3 in implementing this solution might be included in the AWS Free Tier\. For more information, see [Amazon Simple Storage Service pricing](https://aws.amazon.com/s3/pricing/)\.  | 
|  Phone number validation usage  |  The solution in this tutorial uses the phone number validation feature in Amazon Pinpoint to verify that each phone number in your incoming data is valid and properly formatted\. You pay $0\.006 for each phone number validation request\. This tutorial includes a sample file that contains 10 endpoints\. Each time you execute the solution that's shown in this tutorial, you pay $0\.06 for your use of the phone number validation service\.  | 

**Next**: [Prerequisites](tutorials-importing-data-prereqs.md)
# Step 2: Create IAM roles<a name="tutorials-importing-data-create-iam-roles"></a>

The next step in implementing this solution is to configure policies and roles in AWS Identity and Access Management \(IAM\)\. For this solution, you need to create the following roles and policies: 
+ A role that can read and write from a specific set of Amazon S3 buckets\.
+ A role that can be passed to Amazon Pinpoint that allows Amazon Pinpoint to import segment data from your Amazon S3 bucket when you create an import job\.
+ A policy that can perform certain actions in your Amazon Pinpoint account\.

The policies that you create in this section use the principal of granting *least privilege*\. In other words, they only grant the specific permissions that are required to complete a specific task, and no more\.

## Step 2\.1: Create a policy and role for reading and writing from Amazon S3<a name="tutorials-importing-data-create-iam-roles-s3-read-write"></a>

The first policy that you need to create is one that allows Lambda to view the contents of a folder in an Amazon S3 bucket\. It also allows Lambda to read files in that bucket, and to move those files to other folders in the same bucket\.

**To create the policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, paste the following code:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "s3:PutObject",
                   "s3:GetObject",
                   "logs:CreateLogStream",
                   "s3:ListBucket",
                   "s3:DeleteObject",
                   "logs:PutLogEvents"
               ],
               "Resource": [
                   "arn:aws:logs:*:*:*",
                   "arn:aws:s3:::bucket-name/*",
                   "arn:aws:s3:::bucket-name"
               ]
           },
           {
               "Sid": "VisualEditor1",
               "Effect": "Allow",
               "Action": "logs:CreateLogGroup",
               "Resource": "arn:aws:logs:*:*:*"
           },
           {
               "Sid": "VisualEditor2",
               "Effect": "Allow",
               "Action": [
                   "s3:GetAccountPublicAccessBlock",
                   "s3:ListAllMyBuckets",
                   "s3:HeadBucket"
               ],
               "Resource": "*"
           }
       ]
      }
   ```

   In the preceding example, replace *bucket\-name* with the name of the bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md) of this tutorial\.

1. Choose **Review policy**\.

1. For **Name**, enter **ImporterS3Policy**\. Choose **Create policy**\.

When you finish creating the policy, you can create a role that uses it\.

**To create the role**

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Choose the service that will use this role**, choose **Lambda**, and then choose **Next: Permissions**\.

1. Under **Attach permissions policies**, choose **ImporterS3Policy**, and then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Name**, enter **ImporterS3Role**\. Choose **Create role**\.

## Step 2\.2: Create a policy and role for importing from Amazon S3<a name="tutorials-importing-data-create-iam-roles-pinpoint-import"></a>

To use the `CreateImportJob` API operation, you have to provide an IAM role that allows Amazon Pinpoint to read and write from your Amazon S3 bucket\. This role has to be passed from your user role to Amazon Pinpoint\. To enable the role to be passed to Amazon Pinpoint, you have to add a *trust policy* to the role\.

**To create the policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, paste the following code:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "s3:PutAccountPublicAccessBlock",
                   "s3:GetAccountPublicAccessBlock",
                   "s3:ListAllMyBuckets",
                   "s3:HeadBucket"
               ],
               "Resource": "*"
           },
           {
               "Sid": "VisualEditor1",
               "Effect": "Allow",
               "Action": "s3:*",
               "Resource": [
                   "arn:aws:s3:::bucket-name/*",
                   "arn:aws:s3:::bucket-name"
               ]
           }
       ]
   }
   ```

   In the preceding example, replace *bucket\-name* with the name of the bucket that you created in [Step 1](tutorials-importing-data-create-s3-bucket.md) of this tutorial\.

1. Choose **Review policy**\.

1. For **Name**, enter **ImporterS3PassthroughPolicy**\. Choose **Create policy**\.

Next, you create a role that uses this policy, and then attach a trust policy to the role\.

**To create the role and trust policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Choose the service that will use this role**, choose **Lambda**, and then choose **Next: Permissions**\.

1. Under **Attach permissions policies**, choose **ImporterS3PassthroughPolicy**, and then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Name**, enter `PinpointSegmentImport`\. Choose **Create role**\.

1. In the list of roles, choose the **PinpointSegmentImport** role that you just created\.

1. On the **Trust relationships** tab, choose **Edit trust relationship**\.

1. Remove the code in the policy editor, and then paste the following code:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "pinpoint.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update trust policy**\.

## Step 2\.3: Create a policy and role for using Amazon Pinpoint resources<a name="tutorials-importing-data-create-iam-roles-pinpoint"></a>

The next policy that you need to create is one that allows Lambda to interact with Amazon Pinpoint\. Specifically, this policy allows Lambda to call the Amazon Pinpoint phone number validation service, and to create import jobs in Amazon Pinpoint\. It also allows Amazon Pinpoint to read from your Amazon S3 bucket when you create an import job\.

**To create the policy**

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, paste the following code:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "mobiletargeting:CreateImportJob",
                   "mobiletargeting:GetImportJobs",
                   "iam:GetRole",
                   "iam:PassRole"
               ],
               "Resource": [
                   "arn:aws:mobiletargeting:us-east-1:123456789012:apps/01234567890123456789012345678901",
                   "arn:aws:mobiletargeting:us-east-1:123456789012:apps/01234567890123456789012345678901/*",
                   "arn:aws:iam::123456789012:role/PinpointSegmentImport"
               ]
           },
           {
               "Sid": "VisualEditor1",
               "Effect": "Allow",
               "Action": "mobiletargeting:PhoneNumberValidate",
               "Resource": "arn:aws:mobiletargeting:us-east-1:123456789012:phone/number/validate"
           }
       ]
   }
   ```

   In the preceding example, do the following:
   + Replace *us\-east\-1* with the AWS Region that you use Amazon Pinpoint in\.
   + Replace *123456789012* with your AWS account ID\.
   + Replace *01234567890123456789012345678901* with the Amazon Pinpoint project ID that you want to import contacts into\. The application ID that you specify has to exist in your Amazon Pinpoint account in the specified Region\.
**Note**  
You can use wildcards for any of these values\. Using wildcards makes this policy more flexible, but also reduces the security of the policy because it doesn't strictly conform to the principle of least privilege\.

1. Choose **Review policy**\.

1. For **Name**, enter **ImporterPinpointPolicy**\. Choose **Create policy**\.

When you finish creating the policy, you can create a role that uses it\. You also add the `ImporterS3Policy` to the role\.

**To create the role**

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Choose the service that will use this role**, choose **Lambda**, and then choose **Next: Permissions**\.

1. Under **Attach permissions policies**, choose **ImporterS3Policy** and **ImporterPinpointPolicy**, and then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Name**, enter `ImporterPinpointRole`\. Choose **Create role**\.

**Next**: [Create a Package That Contains Required Python Libraries](tutorials-importing-data-create-python-package.md)
# Step 3: Create IAM policies and roles<a name="tutorials-email-prefs-part-3"></a>

The next step in implementing the email preference management solution is to configure a policy and a role in AWS Identity and Access Management \(IAM\)\. For this solution, you need to create a policy that provides access to certain resources that are related to Amazon Pinpoint\. You then create a role and attach the policy to it\. Later in this tutorial, you create an AWS Lambda function that uses this role to call certain operations in the Amazon Pinpoint API\.

## Step 3\.1: Create an IAM policy<a name="tutorials-email-prefs-part-3-create-policy"></a>

This section shows you how to create an IAM policy\. Users and roles that use this policy are able to view, create, and update Amazon Pinpoint endpoints\.

In this tutorial, you want to give Lambda the ability to perform these tasks\. However, for added security, this policy uses the principal of granting *least privilege*\. In other words, it grants only the permissions that are required to complete this solution, and no more\. You can only use this policy to view, create, or update endpoints that are associated with a specific Amazon Pinpoint project\.

**To create the policy**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, paste the following code\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "logs:CreateLogGroup"
               ],
               "Resource": "arn:aws:logs:*:*:*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "mobiletargeting:UpdateEndpoint",
                   "mobiletargeting:GetEndpoint"
               ],
               "Resource": "arn:aws:mobiletargeting:region:accountId:apps/projectId/endpoints/*"
           }
       ]
   }
   ```

   In the preceding example, do the following:
   + Replace *region* with the AWS Region that you use Amazon Pinpoint in, such as `us-east-1` or `eu-central-1`\.
**Tip**  
For a complete list of AWS Regions where Amazon Pinpoint is available, see [AWS regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#pinpoint_region) in the *AWS General Reference*\.
   + Replace *accountId* with the unique ID for your AWS account\.
   + Replace *projectId* with the unique ID of the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project) of this tutorial\.
**Note**  
The `logs` actions enable Lambda to log its output in CloudWatch Logs\.

1. Choose **Review policy**\.

1. For **Name**, enter a name for the policy, such as **EmailPreferencesPolicy**\. Choose **Create policy**\.

## Step 3\.2: Create an IAM role<a name="tutorials-email-prefs-part-3-create-role"></a>

After you create the policy, you can create a role and attach the policy to it\. The Lambda function that you create in [Step 3](#tutorials-email-prefs-part-3) uses this role in order to gain access to the necessary AWS resources\. 

**To create the role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Choose the service that will use this role**, choose **Lambda**, and then choose **Next: Permissions**\.
**Note**  
The service that you choose in this step isn't important—regardless of the service that you choose, you apply your own policy in the next step\.

1. Under **Attach permissions policies**, choose the policy that you created in the previous section, and then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Under **Review**, for **Name**, enter a name for the role, such as **EmailPreferencesForm**\. Choose **Create role**\.

**Next**: [Create the Lambda Function](tutorials-email-prefs-part-4.md)
# Step 1: Create IAM policies and roles<a name="tutorials-using-postman-iam-user"></a>

The first step in using Postman to test the Amazon Pinpoint API is to create an IAM user\. In this section, you create a policy that provides users with the ability to interact with all the Amazon Pinpoint resources\. You then create a user account and attach the policy directly to the user account\.

## Step 1\.1: Create an IAM policy<a name="tutorials-using-postman-iam-user-create-policy"></a>

This section shows you how to create an IAM policy\. Users and roles that use this policy are able to interact with all of the resources in the Amazon Pinpoint API\. It also provides access to resources that are associated with the Amazon Pinpoint Email API, as well as the Amazon Pinpoint SMS and Voice API\.

**To create the policy**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, paste the following code\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "mobiletargeting:Update*",
                   "mobiletargeting:Get*",
                   "mobiletargeting:Send*",
                   "mobiletargeting:Put*",
                   "mobiletargeting:Create*"
               ],
               "Resource": [
                   "arn:aws:mobiletargeting:*:123456789012:apps/*",
                   "arn:aws:mobiletargeting:*:123456789012:apps/*/campaigns/*",
                   "arn:aws:mobiletargeting:*:123456789012:apps/*/segments/*"
               ]
           },
           {
               "Sid": "VisualEditor1",
               "Effect": "Allow",
               "Action": [
                   "mobiletargeting:TagResource",
                   "mobiletargeting:PhoneNumberValidate",
                   "mobiletargeting:ListTagsForResource",
                   "mobiletargeting:CreateApp"
               ],
               "Resource": "arn:aws:mobiletargeting:*:123456789012:*"
           },
           {
               "Sid": "VisualEditor2",
               "Effect": "Allow",
               "Action": [
                   "ses:TagResource",
                   "ses:Send*",
                   "ses:Create*",
                   "ses:Get*",
                   "ses:List*",
                   "ses:Put*",
                   "ses:Update*",
                   "sms-voice:SendVoiceMessage",
                   "sms-voice:List*",
                   "sms-voice:Create*",
                   "sms-voice:Get*",
                   "sms-voice:Update*"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

   In the preceding example, replace *123456789012* with the unique ID for your AWS account\.
**Note**  
To protect the data in your Amazon Pinpoint account, this policy only includes permissions that allow you to read, create, and modify resources\. It doesn't include permissions that allow you to delete resources\. You can modify this policy by using the visual editor in the IAM console\. For more information, see [Managing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the IAM User Guide\. You can also use the [CreatePolicyVersion](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreatePolicyVersion.html) operation in the IAM API to update this policy\.  
Also note that this policy includes permissions that allow you to interact with the `ses` and `sms-voice` services, in addition to the `mobiletargeting` service\. The `ses` and `sms-voice` permissions allow you to interact with the Amazon Pinpoint Email API and Amazon Pinpoint SMS and Voice API, respectively\. The `mobiletargeting` permissions allow you to interact with the Amazon Pinpoint API\.

1. Choose **Review policy**\.

1. For **Name**, enter a name for the policy, such as **PostmanAccessPolicy**\. Choose **Create policy**\.

## Step 1\.2: Create an IAM user<a name="tutorials-using-postman-iam-user-create-user"></a>

After you create the policy, you can create an IAM user and attach the policy to it\. When you create the user, IAM provides you with a set of credentials that you can use to allow Postman to execute Amazon Pinpoint API operations\.

**To create the user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. On the IAM console, in the navigation pane, choose **Users**, and then choose **Add user**\.

1. Under **Set user details**, for **User name**, enter a name that identifies the user account, such as **PostmanUser**\.

1. Under **Select AWS access type**, for **Access type**, choose **Programmatic access**\. Then choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Attach existing policies directly**\. In the list of policies, choose the policy that you created in [Step 1\.1](#tutorials-using-postman-iam-user-create-policy)\. Then choose **Next: Tags**\.

1. On the **Add tags** page, optionally add tags that help you identify the user\. For more information about using tags, see [Tagging IAM users and roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\. Then choose **Next: Review**\.

1. On the **Review** page, review and confirm the settings for the user\. When you're ready to create the user, choose **Create user**\.

1. On the **Success** page, copy the credentials that are shown in the **Access key ID** and **Secret access key** columns\.
**Note**  
You need to provide both the access key ID and the secret access key later in this tutorial\. This is the only time that you're able to view the secret access key, so you should copy it and save it in a safe location\.

**Next**: [Set up Postman](tutorials-using-postman-configuration.md)
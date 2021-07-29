# IAM role for retrieving recommendations from Amazon Personalize<a name="permissions-get-recommendations"></a>

You can configure Amazon Pinpoint to retrieve recommendation data from an Amazon Personalize solution that's been deployed as an Amazon Personalize campaign\. You can use this data to send personalized recommendations to message recipients based on each recipient's attributes and behavior\. To learn more, see [Machine learning models](https://docs.aws.amazon.com/pinpoint/latest/userguide/ml-models.html) in the *Amazon Pinpoint User Guide*\.

Before you can retrieve recommendation data from an Amazon Personalize campaign, you have to create an AWS Identity and Access Management \(IAM\) role that allows Amazon Pinpoint to retrieve the data from the campaign\. Amazon Pinpoint can create this role for you automatically when you use the console to set up a recommender model in Amazon Pinpoint\. Or, you can create this role manually\.

To create the role manually, use the IAM API to complete the following steps:

1. Create an IAM policy that allows an entity \(in this case, Amazon Pinpoint\) to retrieve recommendation data from an Amazon Personalize campaign\.

1. Create an IAM role and attach the IAM policy to it\.

This topic explains how to complete these steps by using the AWS Command Line Interface \(AWS CLI\)\. It assumes that you've already created the Amazon Personalize solution and deployed it as an Amazon Personalize campaign\. For information about creating and deploying a campaign, see [Creating a campaign](https://docs.aws.amazon.com/personalize/latest/dg/campaigns.html) in the *Amazon Personalize Developer Guide*\.

This topic also assumes that you've already installed and configured the AWS CLI\. For information about setting up the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

## Step 1: Create the IAM policy<a name="permissions-get-recommendations-create-policy"></a>

An IAM policy defines permissions for an entity, such as an identity or resource\. To create a role that allows Amazon Pinpoint to retrieve recommendation data from an Amazon Personalize campaign, you first have to create an IAM policy for the role\. This policy needs to allow Amazon Pinpoint to:
+ Retrieve configuration information for the solution that's deployed by the campaign \(`DescribeSolution`\)\.
+ Check the status of the campaign \(`DescribeCampaign`\)\.
+ Retrieve recommendation data from the campaign \(`GetRecommendations`\)\.

In the following procedure, the example policy allows this access for a particular Amazon Personalize solution that was deployed by a particular Amazon Personalize campaign\.

**To create the IAM policy**

1. In a text editor, create a new file\. Paste the following code into the file:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "RetrieveRecommendationsOneCampaign",
               "Effect": "Allow",
               "Action": [
                   "personalize:DescribeSolution",
                   "personalize:DescribeCampaign",
                   "personalize:GetRecommendations"
               ],
                "Resource": [
                   "arn:aws:personalize:region:accountId:solution/solutionId",
                   "arn:aws:personalize:region:accountId:campaign/campaignId"
                   ]
           }
       ]
   }
   ```

   In the preceding example, replace the *italicized* text with your information:
   + *region* – The name of the AWS Region that hosts the Amazon Personalize solution and campaign\.
   + *accountId* – Your AWS account ID\.
   + *solutionId* – The unique resource ID for the Amazon Personalize solution that's deployed by the campaign\. 
   + *campaignId* – The unique resource ID for the Amazon Personalize campaign to retrieve recommendation data from\.

1. When you finish, save the file as `RetrieveRecommendationsPolicy.json`\.

1. By using the command line interface, navigate to the directory where you saved the `RetrieveRecommendationsPolicy.json` file\. 

1. Enter the following command to create a policy and name it `RetrieveRecommendationsPolicy`\. To use a different name, change *RetrieveRecommendationsPolicy* to the name that you want\.

   ```
   aws iam create-policy --policy-name RetrieveRecommendationsPolicy --policy-document file://RetrieveRecommendationsPolicy.json
   ```

   If the policy was created successfully, you see output similar to the following:

   ```
   {
       "Policy": {
           "PolicyName": "RetrieveRecommendationsPolicy",
           "PolicyId": "ANPAJ2YJQRJCG3EXAMPLE",
           "Arn": "arn:aws:iam::123456789012:policy/RetrieveRecommendationsPolicy",
           "Path": "/",
           "DefaultVersionId": "v1",
           "AttachmentCount": 0,
           "PermissionBoundaryUsageCount": 0,
           "IsAttachable": true,
           "CreateDate": "2020-03-04T22:23:15Z",
           "UpdateDate": "2020-03-04T22:23:15Z"
       }
   }
   ```
**Note**  
If you receive a message that your account isn't authorized to perform the `CreatePolicy` operation, you need to attach a policy to your user account that lets you create new IAM policies and roles for your account\. For more information, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#attach-managed-policy-console) in the *IAM User Guide*\.

1. Copy the Amazon Resource Name \(ARN\) of the policy \(`arn:aws:iam::123456789012:policy/RetrieveRecommendationsPolicy` in the preceding example\)\. In the next section, you'll need this ARN to create the IAM role\.

## Step 2: Create the IAM role<a name="permissions-get-recommendations-create-role"></a>

After you create the IAM policy, you can create an IAM role and attach the policy to it\.

Each IAM role contains a *trust policy*, which is a set of rules that specifies which entities are allowed to assume the role\. In this section, you create a trust policy that allows Amazon Pinpoint to assume the role\. Next, you create the role itself\. Then, you attach the policy to the role\.

**To create the IAM role**

1. In a text editor, create a new file\. Paste the following code into the file:

   ```
   {
       "Version":"2012-10-17",
       "Statement":[
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

1. Save the file as `RecommendationsTrustPolicy.json`\.

1. By using the command line interface, navigate to the directory where you saved the `RecommendationsTrustPolicy.json` file\.

1. Enter the following command to create a new role and name it `PinpointRoleforPersonalize`\. To use a different name, change *PinpointRoleforPersonalize* to the name that you want\.

   ```
   aws iam create-role --role-name PinpointRoleforPersonalize --assume-role-policy-document file://RecommendationsTrustPolicy.json
   ```

   If the command runs successfully, you see output similar to the following:

   ```
   {                                                          
       "Role": {                                              
           "Path": "/",
           "RoleName": "PinpointRoleforPersonalize",                           
           "RoleId": "AKIAIOSFODNN7EXAMPLE",
           "Arn": "arn:aws:iam::123456789012:role/PinpointRoleforPersonalize",
           "CreateDate": "2020-03-04T22:29:45Z",
           "AssumeRolePolicyDocument": {                      
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
       }                                                      
   }
   ```

1. Enter the following command to attach the policy that you created in the previous section to the role that you just created:

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam::123456789012:policy/RetrieveRecommendationsPolicy --role-name PinpointRoleforPersonalize
   ```

   In the preceding command, replace *arn:aws:iam::123456789012:policy/RetrieveRecommendationsPolicy* with the ARN of the policy that you created in the previous section\. Also, replace *PinpointRoleforPersonalize* with the name of the role that you specified in step 4, if you specified a different name for the role\.
# IAM role for exporting endpoints or segments<a name="permissions-export-endpoints"></a>

You can obtain a list of endpoints by creating an export job\. When you create an export job, you have to specify a project ID, and you can optionally specify a segment ID\. Amazon Pinpoint then exports a list of the endpoints associated with the project or segment to an Amazon Simple Storage Service \(Amazon S3\) bucket\. The resulting file contains a JSON\-formatted list of endpoints and their attributes, such as channel, address, opt\-in/opt\-out status, creation date, and endpoint ID\. 

To create an export job, you have to configure an IAM role that allows Amazon Pinpoint to write to an Amazon S3 bucket\. The process of configuring the role consists of two steps:

1. Create an IAM policy that allows an entity \(in this case, Amazon Pinpoint\) to write to a specific Amazon S3 bucket\.

1. Create an IAM role and attach the policy to it\.

This topic contains procedures for completing both of these steps\. These procedures assume that you've already created an Amazon S3 bucket, and a folder in that bucket, for storing exported segments\. For information about creating buckets, see [Create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. 

These procedures also assume that you've already installed and configured the AWS Command Line Interface \(AWS CLI\)\. For information about setting up the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

## Step 1: Create the IAM policy<a name="permissions-export-endpoints-create-policy"></a>

An IAM policy defines the permissions for an entity, such as an identity or resource\. To create a role for exporting Amazon Pinpoint endpoints, you have to create a policy that grants permission to write to a specific folder in a specific Amazon S3 bucket\. The following policy example follows the security practice of granting least privilege—that is, it grants only the permissions that are required to perform a single task\.

**To create the IAM policy**

1. In a text editor, create a new file\. Paste the following code into the file:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowUserToSeeBucketListInTheConsole",
               "Action": [
                   "s3:ListAllMyBuckets",
                   "s3:GetBucketLocation"
               ],
               "Effect": "Allow",
               "Resource": [ "arn:aws:s3:::*" ]
           },
           {
               "Sid": "AllowRootAndHomeListingOfBucket",
               "Action": [
                   "s3:ListBucket"
               ],
               "Effect": "Allow",
               "Resource": [ "arn:aws:s3:::example-bucket" ],
               "Condition": {
                   "StringEquals": {
                       "s3:delimiter": [ "/" ],
                       "s3:prefix": [
                           "",
                           "Exports/"
                       ]
                   }
               }
           },
           {
               "Sid": "AllowListingOfUserFolder",
               "Action": [
                   "s3:ListBucket"
               ],
               "Effect": "Allow",
               "Resource": [ "arn:aws:s3:::example-bucket" ],
               "Condition": {
                   "StringLike": {
                       "s3:prefix": [
                           "Exports/*"
                       ]
                   }
               }    
           },
           {
               "Sid": "AllowAllS3ActionsInUserFolder",
               "Action": [ "s3:*" ],
               "Effect": "Allow",
               "Resource": [ "arn:aws:s3:::example-bucket/Exports/*" ]
           }
       ]
   }
   ```

   In the preceding code, replace all instances of *example\-bucket* with the name of the Amazon S3 bucket that contains the folder that you want to export the segment information into\. Also, replace all instances of *Exports* with the name of the folder itself\.

   When you finish, save the file as `s3policy.json`\.

1. By using the AWS CLI, navigate to the directory where the `s3policy.json` file is located\. Then enter the following command to create the policy:

   ```
   aws iam create-policy --policy-name s3ExportPolicy --policy-document file://s3policy.json
   ```

   If the policy was created successfully, you see output similar to the following:

   ```
   {
       "Policy": {
           "CreateDate": "2018-04-11T18:44:34.805Z",
           "IsAttachable": true,
           "DefaultVersionId": "v1",
           "AttachmentCount": 0,
           "PolicyId": "ANPAJ2YJQRJCG3EXAMPLE",
           "UpdateDate": "2018-04-11T18:44:34.805Z",
           "Arn": "arn:aws:iam::123456789012:policy/s3ExportPolicy",
           "PolicyName": "s3ExportPolicy",
           "Path": "/"
       }
   }
   ```

   Copy the Amazon Resource Name \(ARN\) of the policy \(`arn:aws:iam::123456789012:policy/s3ExportPolicy` in the preceding example\)\. In the next section, you must supply this ARN when you create the role\.
**Note**  
If you see a message stating that your account isn't authorized to perform the `CreatePolicy` operation, then you need to attach a policy to your user account that lets you create new IAM policies and roles\. For more information, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#attach-managed-policy-console) in the *IAM User Guide*\.

## Step 2: Create the IAM role<a name="permissions-export-endpoints-create-role"></a>

Now that you've created an IAM policy, you can create a role and attach the policy to it\. Each IAM role contains a *trust policy*—a set of rules that specifies which entities are allowed to assume the role\. In this section, you create a trust policy that allows Amazon Pinpoint to assume the role\. Next, you create the role itself, and then attach the policy that you created in the previous section\.

**To create the IAM role**

1. In a text editor, create a new file\. Paste the following code into the file:

   ```
   {
       "Version":"2012-10-17",
       "Statement":[
           {
               "Effect":"Allow",
               "Principal":{
                   "Service":"pinpoint.amazonaws.com"
               },
               "Action":"sts:AssumeRole"
           }
       ]
   }
   ```

   Save the file as `trustpolicy.json`\.

1. By using the AWS CLI, navigate to the directory where the `trustpolicy.json` file is located\. Then enter the following command to create a new role:

   ```
   aws iam create-role --role-name s3ExportRole --assume-role-policy-document file://trustpolicy.json
   ```

   If the command runs successfully, you see output similar to the following:

   ```
   {                                                          
       "Role": {                                              
           "RoleName": "s3ExportRole",                           
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
           },                                                 
           "RoleId": "AROAICPO353GIPEXAMPLE",                 
           "Arn": "arn:aws:iam::123456789012:role/s3ExportRole", 
           "CreateDate": "2018-04-11T18:52:36.712Z",          
           "Path": "/"                                        
       }                                                      
   }
   ```

1. At the command line, enter the following command to attach the policy that you created in the previous section to the role that you just created:

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam::123456789012:policy/s3ExportPolicy --role-name s3ExportRole
   ```

   In the preceding command, replace *arn:aws:iam::123456789012:policy/s3ExportPolicy* with the ARN of the policy that you created in the previous section\.
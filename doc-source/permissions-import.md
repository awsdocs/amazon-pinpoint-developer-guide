# IAM Role for Importing Segments<a name="permissions-import"></a>

With Amazon Pinpoint, you define a user segment by importing endpoint definitions from an Amazon S3 bucket in your AWS account\. Before you import, you must delegate the required permissions to Amazon Pinpoint\. Create an AWS Identity and Access Management \(IAM\) role and attach the following policies to the role: 

+ The `AmazonS3ReadOnlyAccess` AWS managed policy\. This policy is created and managed by AWS, and it grants read\-only access to your Amazon S3 bucket\.

+ A *trust policy* that allows Amazon Pinpoint to assume the role\.

For more information about IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

After you create the role, you can use Amazon Pinpoint to import segments\. For an example of how to import a segment by using the AWS SDK for Java, see [Importing Segments](segments-importing.md)\. For information about creating the Amazon S3 bucket, creating endpoint files, and importing a segment by using the console, see [Importing Segments](http://docs.aws.amazon.com/pinpoint/latest/userguide/segments-importing.html) in the *Amazon Pinpoint User Guide*\.

## Trust Policy<a name="permissions-import-trustpolicy"></a>

To allow Amazon Pinpoint to assume the IAM role and perform the actions allowed by the `AmazonS3ReadOnlyAccess` policy, attach the following trust policy to the role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "pinpoint.us-east-1.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Creating the IAM Role \(AWS CLI\)<a name="permissions-import-create"></a>

Complete the following steps to create the IAM role by using the AWS Command Line Interface \(AWS CLI\)\.

If you have not installed the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**To create the IAM role by using the AWS CLI**

1. Create a JSON file that contains the trust policy for your role, and save the file locally\. You can copy the trust policy provided in this topic\.

1. Use the [http://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html](http://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the role and attach the trust policy:

   ```
   aws iam create-role --role-name PinpointSegmentImport --assume-role-policy-document file://PinpointImportTrustPolicy.json
   ```

   Following the `file://` prefix, specify the path to the JSON file that contains the trust policy\.

   When you run this command, the AWS CLI prints the following output in your terminal:

   ```
   {
       "Role": {
           "AssumeRolePolicyDocument": {
               "Version": "2012-10-17", 
               "Statement": [
                   {
                       "Action": "sts:AssumeRole", 
                       "Effect": "Allow", 
                       "Principal": {
                           "Service": "pinpoint.us-east-1.amazonaws.com"
                       }
                   }
               ]
           }, 
           "RoleId": "AIDACKCEVSQ6C2EXAMPLE", 
           "CreateDate": "2016-12-20T00:44:37.406Z", 
           "RoleName": "PinpointSegmentImport", 
           "Path": "/", 
           "Arn": "arn:aws:iam::111122223333:role/PinpointSegmentImport"
       }
   }
   ```

1. Use the [http://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html](http://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command to attach the `AmazonS3ReadOnlyAccess` AWS managed policy to the role:

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess --role-name PinpointSegmentImport
   ```
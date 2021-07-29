# IAM role for importing endpoints or segments<a name="permissions-import-segment"></a>

With Amazon Pinpoint, you can define a user segment by importing endpoint definitions from an Amazon Simple Storage Service \(Amazon S3\) bucket in your AWS account\. Before you import, you must delegate the required permissions to Amazon Pinpoint\. To do this, you create an AWS Identity and Access Management \(IAM\) role and attach the following policies to the role: 
+ The `AmazonS3ReadOnlyAccess` AWS managed policy\. This policy is created and managed by AWS, and it grants read\-only access to your Amazon S3 bucket\.
+ A trust policy that allows Amazon Pinpoint to assume the role\.



After you create the role, you can use Amazon Pinpoint to import segments from an Amazon S3 bucket\. For information about creating the bucket, creating endpoint files, and importing a segment by using the console, see [Importing segments](https://docs.aws.amazon.com/pinpoint/latest/userguide/segments-importing.html) in the *Amazon Pinpoint User Guide*\. For an example of how to import a segment programmatically by using the AWS SDK for Java, see [Importing segments](segments-importing.md) in this guide\. 

## Attaching the trust policy<a name="permissions-import-segment-trustpolicy"></a>

To allow Amazon Pinpoint to assume the IAM role and perform the actions allowed by the `AmazonS3ReadOnlyAccess` policy, attach the following trust policy to the role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUserToImportEndpointDefinitions",
      "Effect": "Allow",
      "Principal": {
        "Service": "pinpoint.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Creating the IAM role \(AWS CLI\)<a name="permissions-import-segment-create"></a>

Complete the following steps to create the IAM role by using the AWS Command Line Interface \(AWS CLI\)\. If you haven't installed the AWS CLI, see [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**To create the IAM role by using the AWS CLI**

1. Create a JSON file that contains the trust policy for your role, and save the file locally\. You can copy the trust policy provided in this topic\.

1. At the command line, use the [https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the role and attach the trust policy:

   ```
   aws iam create-role --role-name PinpointSegmentImport --assume-role-policy-document file://PinpointImportTrustPolicy.json
   ```

   Following the `file://` prefix, specify the path to the JSON file that contains the trust policy\.

   After you run this command, you see output that's similar to the following in your terminal:

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
                           "Service": "pinpoint.amazonaws.com"
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

1. Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command to attach the `AmazonS3ReadOnlyAccess` AWS managed policy to the role:

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess --role-name PinpointSegmentImport
   ```
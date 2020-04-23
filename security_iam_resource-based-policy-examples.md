# Amazon Pinpoint Resource\-Based Policy Examples<a name="security_iam_resource-based-policy-examples"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on an Amazon Pinpoint resource and under what conditions they can perform those actions\.

**Topics**
+ [Example: Restricting Amazon Pinpoint Project Access to Specific IP Addresses](#security_iam_resource-based-policy-examples-restrict-project-access-by-ip)
+ [Example: Restricting Amazon Pinpoint Access Based on Tags](#security_iam_resource-based-policy-examples-restrict-access-by-tag)

## Example: Restricting Amazon Pinpoint Project Access to Specific IP Addresses<a name="security_iam_resource-based-policy-examples-restrict-project-access-by-ip"></a>

The following example policy grants permissions to any user to perform any Amazon Pinpoint action on a specified project \(*projectId*\)\. However, the request must originate from the range of IP addresses that are specified in the condition\.

The condition in this statement identifies the `54.240.143.*` range of allowed Internet Protocol version 4 \(IPv4\) addresses, with one exception: `54.240.143.188`\. The `Condition` block uses the `IpAddress` and `NotIpAddress` conditions and the `aws:SourceIp` condition key, which is an AWS\-wide condition key\. For more information about these condition keys, see [Specifying Conditions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/amazon-s3-policy-keys.html) *IAM User Guide*\. The `aws:SourceIp` IPv4 values use standard CIDR notation\. For more information, see [IP Address Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IPAddress) in the *IAM User Guide*\.

```
{
  "Version": "2012-10-17",
  "Id": "AMZPinpointPolicyId1",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "mobiletargeting:*",
      "Resource": [
                "arn:aws:mobiletargeting:*:*:apps/projectId",
                "arn:aws:mobiletargeting:*:*:apps/projectId/*"
                ],
      "Condition": {
         "IpAddress": {"aws:SourceIp": "54.240.143.0/24"},
         "NotIpAddress": {"aws:SourceIp": "54.240.143.188/32"} 
      } 
    } 
  ]
}
```

## Example: Restricting Amazon Pinpoint Access Based on Tags<a name="security_iam_resource-based-policy-examples-restrict-access-by-tag"></a>

The following example policy grants permissions to perform any Amazon Pinpoint action on a specified project \(*projectId*\)\. However, permissions are granted only if the request originates from a user whose name is a value in the `Owner` resource tag for the project, as specified in the condition\.

The `Condition` block uses the `StringEquals` condition and the `aws:ResourceTag/${TagKey}` condition key\. For more information about conditions and condition keys, see [Specifying Conditions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/amazon-s3-policy-keys.html) in the *IAM User Guide*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ModifyResourceIfOwner",
            "Effect": "Allow",
            "Action": "mobiletargeting:*",
            "Resource": [
                "arn:aws:mobiletargeting:*:*:apps/projectId",
                "arn:aws:mobiletargeting:*:*:apps/projectId/*"
                ],
            "Condition": {
                "StringEquals": {"aws:ResourceTag/Owner": "${aws:username}"}
            }
        }
    ]
}
```
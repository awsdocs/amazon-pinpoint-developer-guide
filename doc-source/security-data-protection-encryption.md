# Data encryption<a name="security-data-protection-encryption"></a>

Amazon Pinpoint data is encrypted in transit and at rest\. When you submit data to Amazon Pinpoint, it encrypts the data as it receives and stores it\. When you retrieve data from Amazon Pinpoint, it transmits the data to you by using current security protocols\.

## Encryption at rest<a name="security-data-protection-encryption-rest"></a>

Amazon Pinpoint encrypts all the data that it stores for you\. This includes configuration data, user and endpoint data, analytics data, and any data that you add or import into Amazon Pinpoint\. To encrypt your data, Amazon Pinpoint uses internal AWS Key Management Service \(AWS KMS\) keys that the service owns and maintains on your behalf\. We rotate these keys on a regular basis\. For information about AWS KMS, see the [AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

## Encryption in transit<a name="security-data-protection-encryption-transit"></a>

Amazon Pinpoint uses HTTPS and Transport Layer Security \(TLS\) 1\.0 or later to communicate with your clients and applications\. To communicate with other AWS services, Amazon Pinpoint uses HTTPS and TLS 1\.2\. In addition, when you create and manage Amazon Pinpoint resources by using the console, an AWS SDK, or the AWS Command Line Interface, all communications are secured using HTTPS and TLS 1\.2\.

## Key management<a name="security-data-protection-key-mgmt"></a>

To encrypt your Amazon Pinpoint data, Amazon Pinpoint uses internal AWS KMS keys that the service owns and maintains on your behalf\. We rotate these keys on a regular basis\. You can't provision and use your own AWS KMS or other keys to encrypt data that you store in Amazon Pinpoint\.
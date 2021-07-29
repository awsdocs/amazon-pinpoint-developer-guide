# Internetwork traffic privacy<a name="security-data-protection-internetwork-traffic"></a>

*Internetwork traffic privacy* refers to securing connections and traffic between Amazon Pinpoint and your on\-premises clients and applications, and between Amazon Pinpoint and other AWS resources in the same AWS Region\. The following features and practices can help you ensure internetwork traffic privacy for Amazon Pinpoint\.

## Traffic between Amazon Pinpoint and on\-premises clients and applications<a name="security-data-protection-internetwork-traffic-on-prem"></a>

To establish a private connection between Amazon Pinpoint and clients and applications on your on\-premises network, you can use AWS Direct Connect\. This enables you to link your network to an AWS Direct Connect location by using a standard, fiber\-optic Ethernet cable\. One end of the cable is connected to your router\. The other end is connected to an AWS Direct Connect router\. For more information, see [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) in the *AWS Direct Connect User Guide*\.

To help secure access to Amazon Pinpoint through published APIs, we recommend that you comply with Amazon Pinpoint requirements for API calls\. Amazon Pinpoint requires clients to use Transport Layer Security \(TLS\) 1\.0 or later\. \(We recommend TLS 1\.2 or later\.\) Clients must also support cipher suites with perfect forward secrecy \(PFS\), such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Diffie\-Hellman Ephemeral \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\. 

In addition, requests must be signed using an access key ID and a secret access key that's associated with an AWS Identity and Access Management \(IAM\) principal for your AWS account\. Alternatively, you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Traffic between Amazon Pinpoint and other AWS resources<a name="security-data-protection-internetwork-traffic-region"></a>

To secure communications between Amazon Pinpoint and other AWS resources in the same AWS Region, Amazon Pinpoint uses HTTPS and TLS 1\.2 by default\.
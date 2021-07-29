# Tutorial: Setting up an email preference management system<a name="tutorials-email-prefs"></a>

In many jurisdictions around the world, email senders are required to include a mechanism for opting out of email communications in each email that they send\.

A common way to allow customers to specify their preferences is to host a page that customers can use to choose the specific types of messages that they want to receive\. Typically, customers can also use this same page to completely opt out of all email communications that you send\.

This tutorial shows you how to set up a web form that you can use to capture your customers' email preferences\. The web form sends this information to Amazon Pinpoint\. Amazon Pinpoint then creates or modifies attributes in the customer's endpoint record that indicate the topics they want to receive information about\. You can use these attributes when you create segments in Amazon Pinpoint\.

The web form also includes an option that customers can choose to opt themselves out of all email communications\. When customers choose this option, they're automatically opted out of all topics\. Additionally, the solution in this tutorial modifies their endpoint records so that they don't appear in any future segments in your Amazon Pinpoint project\.

## Architecture<a name="tutorials-email-prefs-arch"></a>

When you use the solution that's described in this tutorial, you send a campaign email to your customers\. This email contains a link to a preference management page\. The link contains several attribute tags that identify each recipient\.

When customers receive the email from you, they can choose the link\. When they do, they're taken to a web form that they can use to opt into or out of various topics\. The form sends this data to an API that's hosted by API Gateway\. This API triggers a Lambda function, which makes changes to the customer's endpoint record\.

The following diagram shows the flow of information in this solution\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_Architecture.png)

## About this solution<a name="tutorials-email-prefs-about"></a>

This section contains information about the solution that you're building in this tutorial\.

**Intended Audience**  
This tutorial is intended for developers and system implementers\. You don't have to be familiar with Amazon Pinpoint to complete the steps in this tutorial\. You should be comfortable managing IAM policies, creating Lambda functions in Node\.js, and deploying web content\. However, you don't need to write any code—this tutorial includes complete example code that you can implement without having to make any changes\.

**Features Used**  
This tutorial includes usage examples for the following Amazon Pinpoint features:
+ Creating dynamic segments
+ Sending email campaigns that contain personalized content
+ Interacting with the Amazon Pinpoint API by using AWS Lambda

**Time Required**  
It should take about 30–45 minutes to complete this tutorial\. After you implement this solution, there are additional steps that you can take to refine the solution to suit your unique use case\.

**Regional Restrictions**  
There are no regional restrictions associated with using this solution\. However, you should make sure that the email campaigns that you send include all of the information that's required in each recipient's jurisdiction\. 

**Resource Usage Costs**  
There's no charge for creating an AWS account\. However, by implementing this solution, you might incur some or all of the costs that are listed in the following table\.


| Description | Cost \(US dollars\) | 
| --- | --- | 
| Message sending costs | You pay $0\.0001 for each email that you send through Amazon Pinpoint\. | 
| Monthly targeted audience | You pay $0 for the first 5,000 endpoints that you target in Amazon Pinpoint each month\. After that, you pay $0\.0012 per endpoint that you target\. | 
| Lambda usage | The Lambda function in this tutorial uses about 80–90 MB of memory and about 100–300 milliseconds of compute time each time it's executed\. Your usage of AWS Lambda in implementing this solution might be included in the Free Tier\. For more information, see [AWS Lambda pricing](https://aws.amazon.com/lambda/pricing/)\. | 
| API Gateway usage | You pay $0\.0000035–$0\.0000037 per API request, depending on which AWS Region you use\. For more information, see [Amazon API Gateway pricing](https://aws.amazon.com/api-gateway/pricing/)\. | 
| Web hosting costs | The price that you pay varies depending on your web hosting provider\. | 

**Next**: [Prerequisites](tutorials-email-prefs-prereqs.md)
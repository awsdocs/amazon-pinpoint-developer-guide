# Tutorial: Setting up an SMS registration system<a name="tutorials-two-way-sms"></a>

SMS messages \(text messages\) are a great way to send time\-sensitive messages to your customers\. These days, many people keep their phones nearby at all times\. Also, SMS messages tend to capture people's attention more than push notifications, emails, or phone calls\.

A common way to capture customers' mobile phone numbers is to use a web\-based form\. After you verify the customer's phone number and confirm their subscription, you can start sending promotional, transactional, and informational SMS messages to that customer\.

This tutorial shows you how to set up a web form to capture customers' contact information\. The web form sends this information to Amazon Pinpoint\. Next, Amazon Pinpoint verifies that the phone number is valid, and captures other metadata that's related to the phone number\. After that, Amazon Pinpoint sends the customer a message asking them to confirm their subscription\. After the customer confirms their subscription, Amazon Pinpoint opts them in to receiving your messages\.

The following architecture diagram shows the flow of data in this solution\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/SMS_Reg_Tutorial_Architecture.png)

## About double opt\-in<a name="tutorials-two-way-sms-double-opt-in"></a>

This tutorial shows you how to set up a double opt\-in system in Amazon Pinpoint that uses two\-way SMS messaging\.

In an SMS double opt\-in system, a customer provides you with their phone number by submitting it in a web form or within your app\. When you receive the request from the customer, you create a new endpoint in Amazon Pinpoint\. The new endpoint should be opted out of your communications\. Next, you send a message to that phone number\. In your message, you ask the recipient to confirm their subscription by replying with a specific word or phrase \(such as "Yes" or "Confirm"\)\. If the customer responds to the message with the word or phrase that you specified, you change the endpoint's status to opted\-in\. Otherwise, if the customer doesn't respond or they respond with a different word or phrase, you can leave the endpoint with a status of opted\-out\.

## About this solution<a name="tutorials-two-way-sms-about"></a>

This section contains information about the solution that you're building in this tutorial\.

**Intended Audience**  
This tutorial is intended for developer and system implementer audiences\. You don't have to be familiar with Amazon Pinpoint to complete the steps in this tutorial\. However, you should be comfortable managing IAM policies, creating Lambda functions in Node\.js, and deploying web content\.

**Features Used**  
This tutorial includes usage examples for the following Amazon Pinpoint features:
+ Sending transactional SMS messages
+ Obtaining information about phone numbers by using phone number validation
+ Receiving incoming SMS messages by using two\-way SMS messaging
+ Creating dynamic segments
+ Creating campaigns
+ Interacting with the Amazon Pinpoint API by using AWS Lambda

**Time Required**  
It should take about one hour to complete this tutorial\. After you implement this solution, there are additional steps that you can take to refine the solution to suit your unique use case\.

**Regional Restrictions**  
This tutorial requires you to lease a long code by using the Amazon Pinpoint console\. You can use the Amazon Pinpoint console to lease dedicated long codes that are based in several countries\. However, only long codes that are based in the United States or Canada can be used to send SMS messages\. \(You can use long codes that are based in other countries and regions to send voice messages\.\)

We developed the code examples in this tutorial with this restriction in mind\. For example, the code examples assume that the recipient's phone number always has 10 digits, and a country code of 1\. If you implement this solution in countries or regions other than the United States or Canada, you have to modify the code examples appropriately\.

**Resource Usage Costs**  
There's no charge for creating an AWS account\. However, by implementing this solution, you might incur the following costs:
+ **Long code lease costs** – To complete this tutorial, you have to lease a long code\. Long codes that are based in the United States \(excluding US Territories\) and Canada cost $1\.00 per month\.
+ **Phone number validation usage** – The solution in this tutorial uses the phone number validation feature of Amazon Pinpoint to verify that each number you receive is valid and properly formatted, and to obtain additional information about the phone number\. You pay $0\.006 for each phone number validation request\.
+ **Message sending costs** – The solution in this tutorial sends outbound SMS messages\. You pay for each message that you send through Amazon Pinpoint\. The price that you pay for each message depends on the country or region of the recipient\. If you send messages to recipients in the United States \(excluding US Territories\), you pay $0\.00645 per message\. If you send messages to recipients in Canada, you pay between $0\.00109–$0\.02, depending on the recipient's carrier and location\.
+ **Message receiving costs** – This solution also receives and processes incoming SMS messages\. You pay for each incoming message that's sent to phone numbers that are associated with your Amazon Pinpoint account\. The price that you pay depends on where the receiving phone number is based\. If your receiving number is based in the United States \(excluding US Territories\), you pay $0\.0075 per incoming message\. If your number is based in Canada, you pay $0\.00155 per incoming message\.
+ **Lambda usage** – This solution uses two Lambda functions that interact with the Amazon Pinpoint API\. When you call a Lambda function, you're charged based on the number of requests for your functions, for the time that it takes for your code to execute, and for the amount of memory that your functions use\. The functions in this tutorial use very little memory, and typically run for 1–3 seconds\. Some or all of your usage of this solution might fall under the Lambda free usage tier\. For more information, see [Lambda pricing](https://aws.amazon.com/lambda/pricing/)\.
+ **API Gateway usage** – The web form in this solution calls an API that's managed by API Gateway\. For every million calls to API Gateway, you pay $3\.50–$3\.70, depending on which AWS Region you use Amazon Pinpoint in\. For more information, see [API Gateway pricing](https://aws.amazon.com/api-gateway/pricing/)\.
+ **Web hosting costs** – This solution includes a web\-based form that you have to host on your website\. The price that you pay for hosting this content depends on your web hosting provider\.

**Note**  
All prices shown in this list are in US Dollars \(USD\)\.

**Next**: [Prerequisites](tutorials-two-way-sms-prereqs.md)
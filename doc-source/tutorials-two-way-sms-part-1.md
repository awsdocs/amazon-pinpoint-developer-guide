# Step 1: Set up Amazon Pinpoint<a name="tutorials-two-way-sms-part-1"></a>

The first step in implementing this solution is to set up Amazon Pinpoint\. In this section, you do the following:
+ Create an Amazon Pinpoint project
+ Enable the SMS channel and lease a long code
+ Configure two\-way SMS messaging

Before you begin with this tutorial, you should review the [prerequisites](tutorials-two-way-sms-prereqs.md)\.

## Step 1\.1: Create an Amazon Pinpoint project<a name="tutorials-two-way-sms-part-1-create-project"></a>

To get started, you need to create an Amazon Pinpoint project\. In Amazon Pinpoint, a *project* consists of segments, campaigns, configurations, and data that are united by a common purpose\. For example, you could use a project to contain all of the content that's related to a particular app, or to a specific brand or marketing initiative\. When you add customer information to Amazon Pinpoint, that information is associated with a project\.

The steps involved in creating a new project differ depending on whether you've created a project in Amazon Pinpoint previously\.

### Creating a project \(new Amazon Pinpoint users\)<a name="tutorials-two-way-sms-part-1-create-project-opt-1"></a>

These steps describe the process of creating a new Amazon Pinpoint project if you've never created a project in the current AWS Region\.

**To create a project**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Use the Region selector to choose the AWS Region that you want to use, as shown in the following image\. If you're unsure, choose the Region that's located closest to you\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Region_Selector.png)

1. Under **Get started**, for **Name**, enter a name for the campaign \(such as **SMSRegistration**\), and then choose **Create project**\.

1. On the **Configure features** page, choose **Skip this step**\.

1. In the navigation pane, choose **All projects**\.

1. On the **All projects** page, next to the project you just created, copy the value that's shown in the **Project ID** column\.
**Tip**  
You need to use this ID in a few different places in this tutorial\. Keep the project ID in a convenient place so that you can copy it later\.

### Creating a project \(existing Amazon Pinpoint users\)<a name="tutorials-two-way-sms-part-1-create-project-opt-2"></a>

These steps describe the process of creating a new Amazon Pinpoint project if you've already created projects in the current AWS Region\.

**To create a project**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Use the Region selector to choose the AWS Region that you want to use, as shown in the following image\. If you're unsure, choose the Region that's located closest to you\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Region_Selector.png)

1. On the **All projects** page, choose **Create a project**\.

1. On the **Create a project** window, for **Project name**, enter a name for the project \(such as **SMSRegistration**\)\. Choose **Create**\.

1. On the **Configure features** page, choose **Skip this step**\.

1. In the navigation pane, choose **All projects**\.

1. On the **All projects** page, next to the project you just created, copy the value that's shown in the **Project ID** column\.
**Tip**  
You need to use this ID in a few different places in this tutorial\. Keep the project ID in a convenient place so that you can copy it later\.

## Step 1\.2: Obtain a dedicated long code<a name="tutorials-two-way-sms-part-1-set-up-channel"></a>

After you create a project, you can start to configure features within that project\. In this section, you enable the SMS channel, and obtain a dedicated long code to use when sending SMS messages\.

**Note**  
This section assumes that you're leasing a long code that's based in the United States or Canada\. If you follow the procedures in this section, but choose a country other than the United States or Canada, you won't be able to use that number to send SMS messages\. To learn more about leasing SMS\-capable long codes in countries other than the United States or Canada, see [Requesting dedicated long codes for SMS messaging with Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-awssupport-long-code.html) in the *Amazon Pinpoint User Guide*\.

1. In the navigation pane, under **Settings**, choose **SMS and voice**\.

1. Next to **SMS settings**, choose **Edit**\.

1. Under **General settings**, choose **Enable the SMS channel for this project**, and then choose **Save changes**\.

1. Next to **Number settings**, choose **Request long codes**\.

1. Under **Long code specifications**, do the following:
   + For **Target country or region**, choose **United States** or **Canada**\.
   + For **Default call type**, choose **Transactional**\.
   + For **Quantity**, choose **1**\.

1. Choose **Request long code**\.

## Step 1\.3: Enable two\-way SMS<a name="tutorials-two-way-sms-part-1-enable-two-way"></a>

Now that you have a dedicated phone number, you can set up two\-way SMS\. Enabling two\-way SMS makes it possible for your customers to respond to the SMS messages that you send them\. In this solution, you use two\-way SMS to give your customers a way to confirm that they want to subscribe to your SMS program\.

1. On the **SMS and voice** settings page, under **Number settings**, choose the long code that you received in the previous section\.

1. Under **Required keywords**, do the following:
   + Next to the **HELP** keyword, for **Response message**, enter the message that you want Amazon Pinpoint to automatically send recipients when they send the keyword "HELP" \(or its variations\) to this long code\.
   + Next to the **STOP** keyword, for **Response message**, enter the message that you want Amazon Pinpoint to automatically send recipients when they send the keyword "STOP" \(or its variations\) to this long code\.
**Note**  
When a recipient sends the keyword "STOP" to one of your long codes, Amazon Pinpoint automatically opts that recipient out of all future SMS messages that are sent from this project\.

1. Under **Registered keyword**, for **Keyword**, enter the word that customers can send you to register to receive messages from you\. Then, for **Response message**, enter the message that Amazon Pinpoint automatically sends when a customer sends the keyword to this long code\.
**Note**  
For the purposes of this tutorial, the value that you enter in this section isn't important\. In this scenario, customers register for your SMS messaging program by completing a registration form, rather than by sending you a message directly\.

1. Under **Two\-Way SMS**, choose **Enable two\-way SMS**\.

1. Under **Incoming message destination**, choose **Create a new SNS topic**\. Enter an Amazon SNS topic name, such as **SMSRegistrationFormTopic**\.

1. Under **Two\-way SMS keywords**, for **Keyword**, enter the word that customers send you to confirm their subscriptions \(such as **Yes** or **Confirm**\)\.
**Note**  
This value isn't case sensitive\.

1. For **Response message**, enter the message that Amazon Pinpoint automatically sends to customers when they send you the keyword that you specified in the previous step\.

1. Choose **Save**\.

**Next**: [Create IAM Policies and Roles](tutorials-two-way-sms-part-2.md)
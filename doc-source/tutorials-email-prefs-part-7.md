# Step 7: Create and send Amazon Pinpoint campaigns<a name="tutorials-email-prefs-part-7"></a>

The email preference management solution is now set up and ready to use\. Now you can start sending campaign emails\. This section shows you how to send campaign emails that contain a special link that recipients can use to manage their subscription preferences\.

## Step 7\.1: Create a segment<a name="tutorials-email-prefs-part-7-create-segment"></a>

To send a campaign email, you first have to create the segment that you want to send the campaign to\. For the purpose of this tutorial, you should use a segment that only contains your own endpoints, or those of internal recipients within your organization\. After you confirm that the solution works as you expect it to work, you can start sending messages to external recipients\.

To create a segment, repeat the procedures in [Step 2](tutorials-email-prefs-part-2.md) to import a segment of internal recipients\.

## Step 7\.2: Create the campaign<a name="tutorials-email-prefs-part-7-create-campaign"></a>

After you create a segment of recipients, you can create a campaign that targets the segment\.

**To create the campaign**

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Campaigns**\.

1. Choose **Create a campaign**\.

1. On the **Create a campaign** page, do the following:
   + For **Campaign name**, enter a name for the campaign\.
   + For **Campaign type**, choose **Standard campaign**\.
   + Choose **Next**\.

1. On the **Choose a segment** page, do the following:
   + Choose **Use an existing segment**\.
   + Under **Segment details**, for **Segment**, choose the segment that you created in [Step 7\.1](#tutorials-email-prefs-part-7-create-segment)\.
   + Choose **Next**\.

1. On the **Create a message** page, do the following:
   + Under **Specifications**, for **Choose a channel for this campaign**, choose **Email**\.
   + Under **Email details**, for **Sender email address**, enter the email address that you want to send the email from\. The email address that you specify must be verified\.
   + Under **Message content**, choose **Create a new email**\.
   + For **Subject**, enter the subject line for the email\.
   + For **Message**, enter the message that you want to send\. In the body of the email, include an "Unsubscribe" or "Change your email preferences" link\. Use the following URL as the destination for the link:

     ```
     https://www.example.com/prefs.html
       ?firstName={{User.UserAttributes.FirstName}}
       &lastName={{User.UserAttributes.LastName}}
       &email={{Attributes.Email}}
       &endpointId={{Attributes.EndpointId}}
       &t1={{Attributes.SpecialOffersOptStatus}}
       &t2={{Attributes.NewProductsOptStatus}}
       &t3={{Attributes.ComingSoonOptStatus}}  
       &t4={{Attributes.DealOfTheDayOptStatus}}
     ```

     In the preceding example, replace *https://www\.example\.com/prefs\.html* with the location where you uploaded the form in [Step 6\.3](tutorials-email-prefs-part-6.md#tutorials-email-prefs-part-6-upload-form)\.
**Important**  
The URL in the preceding example includes line breaks and spaces in order to make it easier to read\. Remove all of the line breaks and spaces from this example before you paste it into the email editor\.
   + Choose **Next**\.

1. On the **Choose when to send the campaign** page, do the following:
   + For **Choose when the campaign should be sent**, choose **At a specific time**\.
   + If you want to send the message as soon as you finish creating the campaign, choose **Immediately**\. If you want to send the message at a specific time, choose **Once**, and then specify the date and time when the message should be sent\.
   + Choose **Next**\.

1. On the **Review and launch** page, confirm the settings for the campaign\. If the campaign settings are correct, choose **Launch campaign**\.

1. When you receive the campaign email, choose the "Unsubscribe" or "Manage your email preferences" link that you specified in the message\. Confirm that the form loads properly, and that it's filled in with the appropriate values for **Email address**, **ID**, **First name**, and **Last name**\. Also, make sure that the selections in the **Subscriptions** section correspond with the opt\-in values that you specified when you created the endpoint\.

## Step 7\.3: Create production segments<a name="tutorials-email-prefs-part-7-create-prod-segments"></a>

After you complete the procedures in the preceding sections and confirmed that the preference page is working as expected, you're ready to start creating segments of customers who've opted into your various topics\.

**To create production segments for your topics**

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\.

1. On the **Create a segment** page, choose **Build a segment**\.

1. Under **Specifications**, for **Name**, enter a name for the segment\.

1. In **Segment group 1**, add a **Filter by endpoint**\.

1. For **Choose an endpoint attribute**, choose **ComingSoonOptStatus**\. Then, for **Choose values**, choose **OptIn**\.

1. Choose **Create segment**\.

1. Repeat steps 4–8 for each of the remaining opt topics \(**DealOfTheDayOptStatus**, **NewProductsOptStatus**, and **SpecialOffersOptStatus**\)\. When you complete this step, you have a separate segment for each of your opt topics\. When you want to send an email campaign to customers who subscribe to a specific topic, choose the appropriate segment when you create the campaign\.

1. \(Optional\) Create additional segments that further refine your audience\. When you create additional segments, use the **Include endpoints that are in any of the following segments** menu to choose the appropriate base segment of opted\-in endpoints\.

   To learn more about creating segments, see [Customizing segments with AWS Lambda](segments-dynamic.md)\.

**Next**: [Next Steps](tutorials-email-prefs-next-steps.md)
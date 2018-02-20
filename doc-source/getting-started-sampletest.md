# Testing the Sample App With Amazon Pinpoint<a name="getting-started-sampletest"></a>

To verify that your AWS Mobile Hub sample app can receive push notifications from Amazon Pinpoint, use Amazon Pinpoint to create a simple campaign and send a message to the app\.

**Prerequisite**  
To complete this task, you need a Mobile Hub sample app with Amazon Pinpoint support\. To receive push notifications, you must build the sample app and run it\. For more information, see [Getting Started With iOS Apps](getting-started-ios.md) or [Getting Started With Android Apps](getting-started-android.md)\.

**To send a push notification to the sample app**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **Projects** page, choose the app that you created in AWS Mobile Hub\.

1. Unless you have already created a campaign for your app, the console shows the **Campaigns** page\. Choose **New campaign** to create a campaign\.  
![\[The Campaigns page for a first-time user.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/campaigns_ftue.png)![\[The Campaigns page for a first-time user.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[The Campaigns page for a first-time user.\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. The **Create a campaign** page displays at the **Details** step\.

1. For **Name your campaign**, type a name to make the campaign easy to recognize later\.

1. For **Choose the type of your campaign**, choose **Standard campaign**\.

1. Choose **Next step**, and the console displays the **Segments** step\.

1. Keep the default options for the segment, which include users who have used your app in the last 30 days\. The **Segment estimate** count indicates how many user endpoints your campaign will deliver messages to\. Because you built the sample app for iOS or Android and ran that app, you will see a segment estimate of 1 if your app successfully registered as an endpoint with Amazon Pinpoint\.

1. Choose **Next step**\. The console displays the **Message** step\.

1. Keep **Standard notification** selected, and type a title and message for your test push notification\.

1. For **Action**, keep **Open app**\.

1. Choose **Next step**\. The console displays the **Schedule** step\.

1. Choose **Immediate**, and then choose **Next step**\. The console displays the **Review and launch** step\.

1. Choose **Launch campaign**\. Amazon Pinpoint delivers your test push notification to the sample app on your test device\.
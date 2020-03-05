# Step 1: Set Up Amazon Pinpoint<a name="tutorials-email-prefs-part-1"></a>

The first step in implementing this solution is to set up Amazon Pinpoint\. In this section, you do the following:
+ Create an Amazon Pinpoint project
+ Enable the email channel and verify an identity
+ Set up endpoint attributes

Before you begin this tutorial, you should review the [prerequisites](tutorials-email-prefs-prereqs.md)\.

## Step 1\.1: Create an Amazon Pinpoint Project<a name="tutorials-email-prefs-part-1-create-project"></a>

To get started, you need to create an Amazon Pinpoint project\. In Amazon Pinpoint, a project consists of segments, campaigns, configurations, and data that are united by a common purpose\. For example, you could use a project to contain all of the content that's related to a particular app, or to a specific brand or marketing initiative\. When you add customer information to Amazon Pinpoint, that information is associated with a project\.

The steps involved in creating a new project differ depending on whether you've created a project in Amazon Pinpoint previously\.

### Creating a Project \(New Amazon Pinpoint Users\)<a name="tutorials-email-prefs-part-1-create-project-opt-1"></a>

These steps describe the process of creating a new Amazon Pinpoint project if you've never created a project in the current AWS Region\.

**To create a project**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Use the Region selector to choose the AWS Region that you want to use, as shown in the following image\. If you're unsure, choose the Region that's located closest to you\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Region_Selector.png)

1. Under **Get started**, for **Name**, enter a name for the campaign, and then choose **Create project**\.

1. On the **Configure features** page, choose **Skip this step**\.

1. In the navigation pane, choose **All projects**\.

1. On the **All projects** page, next to the project you just created, copy the value that's shown in the **Project ID** column\.
**Tip**  
You need to use this ID in a few different places in this tutorial\. Keep the project ID in a convenient place so that you can copy it later\.

### Creating a Project \(Existing Amazon Pinpoint Users\)<a name="tutorials-email-prefs-part-1-create-project-opt-2"></a>

These steps describe the process of creating a new Amazon Pinpoint project if you've already created projects in the current AWS Region\.

**To create a project**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Use the Region selector to choose the AWS Region that you want to use, as shown in the following image\. If you're unsure, choose the Region that's located closest to you\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Region_Selector.png)

1. On the **All projects** page, choose **Create a project**\.

1. In the **Create a project** window, for **Project name**, enter a name for the project\. Choose **Create**\.

1. On the **Configure features** page, choose **Skip this step**\.

1. In the navigation pane, choose **All projects**\.

1. On the **All projects** page, next to the project that you just created, copy the value that's shown in the **Project ID** column\.
**Tip**  
You need to use this ID in a few different places in this tutorial\. Keep the project ID in a convenient place so that you can copy it later\.

## Step 1\.2: Enable the Email Channel<a name="tutorials-email-prefs-part-1-enable-channel"></a>

After you create a project, you can start to configure features within that project\. In this section, you enable the email channel\.

1. In the navigation pane, under **Settings**, choose **Email**\.

1. Next to **Identity details**, choose **Edit**\.

1. On the **Edit email** page, under **Identity details**, choose **Enable the email channel for this project**\.

1. Complete the steps in the next section to verify an identity\.

## Step 1\.3: Verify an Identity<a name="tutorials-email-prefs-part-1-verify-identity"></a>

In this section, you verify an identity\. In Amazon Pinpoint, an *identity* is an email address or domain that you use for sending email\. When you verify an email address, you can send email from that address\. When you verify a domain, you can send email from any address on that domain\.

This section includes procedures for verifying an email address, as well as procedures for verifying an identity\. You only need to verify one type of identity\. In most cases, the process of verifying an email address is easier and faster\. However, verifying a domain gives you more flexibility, because it allows you to send email using any address from that domain\. For example, if you want to use a Reply\-To address that's different from the From address, you might find it easier to verify the entire domain\.

### Verifying an Email Address<a name="tutorials-email-prefs-part-1-verify-identity-email-address"></a>

This section includes procedures for verifying an email address in Amazon Pinpoint\. You don't need to complete these steps if you've already [verified the domain](#tutorials-email-prefs-part-1-verify-identity-domain) that you want to use for sending email\.

1. Under **Identity type**, choose **Email address**, and then choose **Verify a new email address**\.

1. For **Email address**, enter the email address that you want to verify\. The email address must be an address that you can access, and it has to be able to receive mail\.

1. Choose **Verify email address**\.

1. Choose **Save**\.

1. Check the inbox of the address that you entered and look for an email from *no\-reply\-aws@amazon\.com*\. Open the email and click the link in the email to complete the verification process for the email address\.
**Note**  
You should receive the verification email within five minutes\. If you don't receive the email, do the following:  
Make sure you typed the address that you want to verify correctly\.
Make sure the email address that you're attempting to verify is able to receive email\. You can test this by using another email address to send a test email to the address that you want to verify\.
Check your junk mail folder\.
The link in the verification email expires after 24 hours\. To resend the verification email, choose **Send verification email again**\.

When you verify an email address, consider the following:
+ Amazon Pinpoint has endpoints in multiple AWS Regions\. The verification status of an email address is separate for each Region\. If you want to send email from the same identity in more than one Region, you must verify that identity in each Region\. You can verify as many as 10,000 identities \(email addresses and domains, in any combination\) in each AWS Region\.
+ The *local part* of the email address, which is the part that precedes the at sign \(@\), is case sensitive\. For example, if you verify *user@example\.com*, you can't send email from *USER@example\.com* unless you verify that address too\.
+ Domain names are case insensitive\. For example, if you verify *user@example\.com*, you can also send email from *user@EXAMPLE\.com*\.
+ You can apply labels to verified email addresses by adding a plus sign \(\+\) followed by a string of text after the local part of the address and before the at sign \(@\)\. For example, to apply *label1* to the address *user@example\.com*, use *user\+label1@example\.com*\. You can use as many labels as you want for each verified address\. You can also use labels in the "From" and "Return\-Path" fields to implement Variable Envelope Return Path \(VERP\)\. 
**Note**  
When you verify an unlabeled address, you are verifying all addresses that could be formed by adding a label to the address\. However, if you verify a labeled address, you can't use other labels with that address\.

### Verifying a Domain<a name="tutorials-email-prefs-part-1-verify-identity-domain"></a>

This section includes procedures for verifying a domain in Amazon Pinpoint\. You don't need to complete these steps if you've already [verified the email address](#tutorials-email-prefs-part-1-verify-identity-email-address) that you want to use for sending email\.

**Note**  
To complete the steps in this section, you need to be able to modify the DNS settings for your domain\. The exact steps required for modifying the DNS settings vary depending on which DNS provider you use\. If you're unable to change the DNS settings for your domain, or you're not comfortable making these changes, contact your system administrator\.

1. Complete the steps in the previous section to enable the email channel\.

1. Under **Identity type**, choose **Domain**\.

1. Choose **Verify a new domain**\. Then, for **Domain**, enter the name of the domain that you want to verify\.

1. For **Default sender address**, enter the default address that you want to use to send email from this domain\.

   When you send email from this domain, you can send it from any address on the domain\. However, if you don't specify a sender address, Amazon Pinpoint sends the email from the address that you specify in this section\.

1. Choose **Verify domain**\. Copy the three DNS name and record values that are shown under **Record set**\. Alternatively, you can choose **Download record set** to download a spreadsheet that contains these records\.

   When you finish, choose **Save**\.

1. Log in to the management console for your DNS or web hosting provider, and then create three new CNAME records that contain the values that you saved in the previous step\. See the next section for links to the documentation for several common providers\.

   It usually takes 24–48 hours for DNS settings to propagate\. As soon as Amazon Pinpoint detects all three of these CNAME records in the DNS configuration of your domain, the verification process is complete\.
**Note**  
You can't send email from a domain until the verification process is complete\.

#### Instructions for Configuring DNS Records for Various Providers<a name="tutorials-email-prefs-part-1-verify-identity-domain-providers"></a>

The procedures for updating the DNS records for a domain vary depending on which DNS or web hosting provider you use\. The following table lists links to the documentation for several common providers\. This list isn't exhaustive, and inclusion in this list isn’t an endorsement or recommendation of any company’s products or services\. If your provider isn't listed in the table, you can probably use the domain with Amazon Pinpoint\.


| DNS/Hosting Provider | Documentation Link | 
| --- | --- | 
|  Amazon Route 53  |  [Creating Records by Using the Amazon Route 53 Console](Amazon Route 53 Developer Guideresource-record-sets-creating.html)  | 
|  GoDaddy  |  [Add a CNAME record](https://www.godaddy.com/help/add-a-cname-record-19236) \(external link\)  | 
|  Dreamhost  |  [How do I add custom DNS records?](https://help.dreamhost.com/hc/en-us/articles/215414867-How-do-I-add-custom-DNS-records-) \(external link\)  | 
|  Cloudflare  |  [Managing DNS records in Cloudflare](https://support.cloudflare.com/hc/en-us/articles/360019093151) \(external link\)  | 
|  HostGator  |  [Manage DNS Records with HostGator/eNom](https://support.hostgator.com/articles/hosting-guide/lets-get-started/dns-name-servers/manage-dns-records-with-hostgatorenom) \(external link\)  | 
|  Namecheap  |  [How do I add TXT/SPF/DKIM/DMARC records for my domain? ](https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain) \(external link\)  | 
|  Names\.co\.uk  |  [Changing your domains DNS Settings](https://www.names.co.uk/support/1156-changing_your_domains_dns_settings.html) \(external link\)  | 
|  Wix  |  [Adding or Updating CNAME Records in Your Wix Account](https://support.wix.com/en/article/adding-or-updating-cname-records-in-your-wix-account) \(external link\)  | 

#### Domain Verification Tips and Troubleshooting<a name="tutorials-email-prefs-part-1-verify-identity-domain-troubleshooting"></a>

If you completed the preceding steps but your domain isn’t verified after 72 hours, check the following:
+ Make sure that you entered the values for the DNS records in the correct fields\. Some providers refer to the **Name/host** field as *Host* or *Hostname*\. In addition, some providers refer to the **Record value** field as *Points to* or *Result*\.
+ Make sure that your provider didn’t automatically append your domain name to the **Name/host** value that you entered in the DNS record\. Some providers append the domain name without indicating that they’ve done so\. If your provider appended your domain name to the **Name/host** value, remove the domain name from the end of the value\. You can also try adding a period to the end of the value in the DNS record\. This period indicates to the provider that the domain name is fully qualified\.
+ The underscore character \(\_\) is required in the **Name/host** value of each DNS record\. If your provider doesn't allow underscores in DNS record names, contact the provider's customer support department for additional assistance\.
+ The validation records that you have to add to the DNS configuration for your domain are different for each AWS Region\. If you want to use a domain to send email from multiple AWS Regions, you have to verify the domain in each of those Regions\.

**Next**: [Add or Configure Endpoints](tutorials-email-prefs-part-2.md)
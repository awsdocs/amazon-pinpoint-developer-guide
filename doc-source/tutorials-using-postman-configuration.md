# Step 2: Set up Postman<a name="tutorials-using-postman-configuration"></a>

Now that you've created an IAM user account that's able to access the Amazon Pinpoint API, you can set up Postman\. In this section, you create one or more environments in Postman\. Next, you import a collection that contains a request template for each of the operations in the Amazon Pinpoint API\.

## Step 2\.1: Create Postman environments<a name="tutorials-using-postman-configuration-create-environments"></a>

In Postman, an *environment* is a set of variables that are stored as key\-value pairs\. You can use environments to quickly change the configuration of the requests that you make through Postman, without having to change the API requests themselves\.

In this section, you create at least one environment to use with Amazon Pinpoint\. Each environment that you create contains a set of variables that are specific to your account in a single AWS Region\. If you use the procedures in this section to create more than one environment, you can easily change between Regions by choosing a different environment from the **Environment** menu in Postman\.

**To create an environment**

1. In Postman, on the **File** menu, choose **New**\.

1. On the **Create New** window, choose **Environment**\.

1. On the **MANAGE ENVIRONMENTS** window, for **Environment Name**, enter **Amazon Pinpoint \- *Region Name***\. Replace *Region Name* with one of the following values:
   + US East \(N\. Virginia\)
   + US West \(Oregon\)
   + Asia Pacific \(Mumbai\)
   + Asia Pacific \(Sydney\)
   + Europe \(Frankfurt\)
   + Europe \(Ireland\)

1. Create six new variables: `endpoint`, `region`, `serviceName`, `accountId`, `accessKey`, and `secretAccessKey`\. Use the following table to determine which value to enter in the **Initial Value** column for each variable\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/tutorials-using-postman-configuration.html)

   After you create these variables, the **MANAGE ENVIRONMENTS** window resembles the example shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_2.1_4.png)

   When you finish, choose **Add**\.
**Important**  
The access keys shown in the preceding image are fictitious\. Never share your IAM access keys with others\.  
Postman includes features that enable you to share and export environments\. If you use these features, be careful to not share your access key ID and secret access key with anybody who shouldn't have access to these credentials\.  
For more information, see [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

1. \(Optional\) Repeat steps 1–4 for each additional environment that you want to create\.
**Tip**  
In Postman, you can create as many environments as you need\. You can use environments in several ways\. For example, you can do all of the following:  
Create a separate environment for every Region where you need to test the Amazon Pinpoint API\.
Create environments that are associated with different AWS accounts\.
Create environments that use credentials that are associated with other IAM users\.

1. When you finish creating environments, proceed to the next section\.

## Step 2\.2: Create an Amazon Pinpoint collection in Postman<a name="tutorials-using-postman-configuration-create-pinpoint-collection"></a>

In Postman, a *collection* is a group of API requests\. Requests in a collection are typically united by a common purpose\. In this section, you create a new collection that contains a request template for each operation in the Amazon Pinpoint API\.

**To create the Amazon Pinpoint collection**

1. In Postman, on the **File** menu, choose **Import**\.

1. On the **Import** window, choose **Import From Link**, and then enter the following URL: [https://raw\.githubusercontent\.com/awsdocs/amazon\-pinpoint\-developer\-guide/master/Amazon%20Pinpoint\.postman\_collection\.json](https://raw.githubusercontent.com/awsdocs/amazon-pinpoint-developer-guide/master/Amazon%20Pinpoint.postman_collection.json)\. 

   Choose **Import**\. Postman imports the Amazon Pinpoint collection, which contains 120 example requests\.

## Step 2\.3: Test your Postman configuration<a name="tutorials-using-postman-configuration-test-operation"></a>

After you import the Amazon Pinpoint collection, you should perform a quick test to make sure that all of the components are properly configured\. You can test your configuration by submitting a `GetApps` request\. This request returns a list of all of the projects that exist in your Amazon Pinpoint account in the current AWS Region\. This request doesn't require any additional configuration, so it's a good way to quickly test your configuration\.

**To test the configuration of the Amazon Pinpoint collection**

1. In the navigation pane, expand the **Amazon Pinpoint** collection, and then expand the **Apps** folder\.

1. In the list of requests, choose **GetApps**\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_2.3_2.png)

1. Use the **Environment** selector to choose the environment that you created in [Step 2\.1](#tutorials-using-postman-configuration-create-environments), as shown in the following image\.  
![\[\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_Environments.png)

1. Choose **Send**\. If the request is sent successfully, the response pane shows a status of `200 OK`\. You see a response that resembles the example in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Postman_Tutorial_2.3_3.png)

   This response shows a list of all of the Amazon Pinpoint projects that exist in your account in the Region that you chose in step 3\.

### Troubleshooting<a name="tutorials-using-postman-configuration-test-operation-troubleshooting"></a>

When you submit your request, you might see an error\. See the following list for several common errors that you might encounter, and for steps that you can take to resolve them\.


| Error message | Problem | Resolution | 
| --- | --- | --- | 
|  Could not get any response There was an error connecting to https://%7B%7Bendpoint%7D%7D/v1/apps\.  |  There is no current value for the `{{endpoint}}` variable, which is set when you choose an environment\.  | Use the environment selector to choose an environment\. | 
|  The security token included in the request is invalid\.  |  Postman wasn't able to find the current value of your access key ID or secret access key\.  |  Choose the gear icon near the environment selector, and then choose the current environment\. Make sure that the `accessKey` and `secretAccessKey` values appear in both the **INITIAL VALUE** and **CURRENT VALUE** columns, and that you entered the credentials correctly\.  | 
|  "Message": "User: arn:aws:iam::123456789012:user/PinpointPostmanUser is not authorized to perform: mobiletargeting:GetApps on resource: arn:aws:mobiletargeting:us\-west\-2:123456789012:\*"  |  The IAM policy associated with your user doesn't include the appropriate permissions\.  |  Make sure that your IAM user has the permissions that are described in [Step 1\.1](tutorials-using-postman-iam-user.md#tutorials-using-postman-iam-user-create-policy), and that you provided the correct credentials when you created the environment in [Step 2\.1](#tutorials-using-postman-configuration-create-environments)\.  | 

**Next**: [Send additional requests](tutorials-using-postman-sample-requests.md)
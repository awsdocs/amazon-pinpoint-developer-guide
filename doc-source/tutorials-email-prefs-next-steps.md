# Next steps<a name="tutorials-email-prefs-next-steps"></a>

By completing this tutorial, you've done the following:
+ Created an Amazon Pinpoint project, enabled the email channel, and verified an identity for sending email\.
+ Created a IAM policy that uses the principal of least privilege to grant access rights, and have associated that policy with a role\.
+ Created a Lambda function that uses the UpdateEndpoint operation in the Amazon Pinpoint API\.
+ Created a REST API using API Gateway\.
+ Created and deployed a web\-based form that collects email recipients' subscription preferences\.
+ Created an email campaign that contains a link to the web form that is personalized for each recipient\.
+ Created segments that contain endpoints that are opted in to your topics\.
+ Performed tests on the solution to make sure it works as expected\.

This section discusses a few ways that you can use the information that you collect by using this solution\. It also includes some suggestions of ways that you can customize this solution to fit your unique use case\.

## Use the form to collect additional information<a name="tutorials-email-prefs-next-steps-collect-additional"></a>

You can modify this solution to collect additional information on the registration form\. For example, you could ask the customer to provide their address, and then use the address data to populate the `Location.City`, `Location.Country`, `Location.Region`, and `Location.PostalCode` fields in the `Endpoint` resource\. If you collect this data, you can use it to create targeted segments\. 

For example, if you offer different products to customers based on their country, you could create a segment of users who are opted in to a topic, and who are located in a specific country\. After that, you could send a campaign to that segment that contains products that are tailored to that country or region\. To make this change, you need to add the appropriate fields to the web form\. You also have to modify the JavaScript code in the form handler to pass the new values\. Finally, you have to modify the Lambda function that updates the endpoint to handle the new incoming information\.

You can also modify the form so that it collects contact information in other channels\. For example, you could use the form to collect the customers' phone numbers so that you can send them SMS alerts\. To make this change, you need to modify the HTML for the web form, and the JavaScript code in the form handler\. You also have to modify the Lambda function so that it creates or updates two separate endpoints \(one for the email endpoint, and one for the SMS endpoint\)\. Finally, you should modify the Lambda function so that it generates a unique value for the `User.UserId` attribute, and then associates that value with both endpoints\.

## Record additional attributes for auditing purposes<a name="tutorials-email-prefs-next-steps-auditing"></a>

This solution records two valuable attributes when it creates and updates endpoints\. First, when the first Lambda function updates the endpoint, it records the location of the form in the `Attributes.OptSource` attribute\. When a customer submits the form, the Lambda function creates or updates the `Attributes.OptStatusLastChanged` attribute\. This attribute contains the exact date and time when the customer last updated the opt status for all of your topics\.

Both of these fields can be useful if you're ever asked an email provider or regulatory agency to provide evidence of a customer's consent\. You can retrieve this information at any time by using the [GetEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#GetEndpoint) API operation\.

You can also modify the Lambda functions to record additional data that might be useful for auditing purposes, such as the IP address that the registration request was submitted from\.

## Collect new customer information<a name="tutorials-email-prefs-next-steps-preference-center"></a>

The solution in this tutorial uses the [UpdateEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#UpdateEndpoint) API operation, which creates new endpoints and also updates existing ones\. You can use the web form to capture information about new customers\. If you do, you have to generate a unique endpoint ID for each customer\. When a new endpoint enters your system in this way, you should set the OptOut value for it to "ALL"\. Next, you should send a message to the endpoint that asks the customer to click a link to confirm their subscription\. When the customer confirms their subscription, you can change the OptOut value to "NONE"\.

This practice is called "double opt\-in\." By using a double opt\-in system, you can prevent malicious users from registering endpoints that aren't their own\. Implementing this kind of system helps to protect your reputation as a sender\. Additionally, capturing this type of explicit opt information is required in several countries and regions around the world\.
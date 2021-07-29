# Next steps<a name="tutorials-two-way-sms-next-steps"></a>

By completing this tutorial, you've done the following:
+ Created an Amazon Pinpoint project, configured the SMS channel, and obtained a dedicated long code\.
+ Created a IAM policy that uses the principal of least privilege to grant access rights, and associated that policy with a role\.
+ Created two Lambda functions that use the PhoneNumberValidate, UpdateEndpoint, and SendMessages operations in the Amazon Pinpoint API\.
+ Created a REST API using API Gateway\.
+ Created and deployed a web\-based form that collects customers' contact information\.
+ Performed tests on the solution to make sure it works\.

This section discusses a few ways that you can use the customer information that you collect by using this solution\. It also includes some suggestions of ways that you can customize this solution to fit your unique use case\.

## Create customer segments<a name="tutorials-two-way-sms-next-steps-create-segments"></a>

All of the customer details that you collect through this form are stored as endpoints\. This solution creates endpoints that contain several attributes that you can use for segmentation purposes\.

For example, this solution captures an endpoint attribute called `Source`\. This attribute contains the full path to the location where the form was hosted\. When you create a segment, you can filter the segment by endpoint, and then further refine the filter by choosing a `Source` attribute\.

Creating segments based on the `Source` attribute can be useful in several ways\. First, it allows you to quickly create a segment of customers who signed up to receive SMS messages from you\. Additionally, the segmentation tool in Amazon Pinpoint automatically excludes endpoints that aren't opted in to receive messages\.

The `Source` attribute is also useful if you decide to host the registration form in several different locations\. For example, your marketing material could refer to a form that's hosted in one location, while customers who encounter the form while browsing your website could see a version that's hosted somewhere else\. When you do this, the Source attributes for customers who complete the form after seeing your marketing materials are different from those who complete the form after finding it on your website\. You can use this difference to create distinct segments, and then send tailored communications to each of those audiences\.

## Send personalized campaign messages<a name="tutorials-two-way-sms-next-steps-send-campaigns"></a>

After you create segments, you can start sending campaigns to those segments\. When you create campaign messages, you can personalize them by specifying which endpoint attributes you want to include in the message\. For example, the web form used in this solution requires the customer to enter their first and last names\. These values are stored in the user record that's associated with the endpoint\.

For example, if you use the `GetEndpoint` API operation to retrieve information about an endpoint that was created using this solution, you see a section that resembles the following example:

```
  ...
  "User": {
    "UserAttributes": {
      "FirstName": [
        "Carlos"
      ],
      "LastName": [
        "Salazar"
      ]
    }
  }
  ...
```

If you want to include the values of these attributes in your campaign message, you can use dot notation to refer to the attribute\. Then enclose the entire reference in double curly braces\. For example, to include each recipient's first name in a campaign message, include the following string in the message: `{{User.UserAttributes.FirstName}}`\. When Amazon Pinpoint sends the message, it replaces the string with the value of the `FirstName` attribute\.

## Use the form to collect additional information<a name="tutorials-two-way-sms-next-steps-collect-additional"></a>

You can modify this solution to collect additional information on the registration form\. For example, you could ask the customer to provide their address, and then use the address data to populate the `Location.City`, `Location.Country`, `Location.Region`, and `Location.PostalCode` fields in the `Endpoint` resource\. Collecting address information on the registration form might result in the endpoint containing more accurate information\. To make this change, you need to add the appropriate fields to the web form\. You also have to modify the JavaScript code for the form to pass the new values\. Finally, you have to modify the Lambda function that creates the endpoint to handle the new incoming information\.

You can also modify the form so that it collects contact information in other channels\. For example, you could use the form to collect the customer's email address in addition to their phone number\. To make this change, you need to modify the HTML and JavaScript for the web form\. You also have to modify the Lambda function that creates the endpoint so that it creates two separate endpoints \(one for the email endpoint, and one for the SMS endpoint\)\. You should also modify the Lambda function so that it generates a unique value for the `User.UserId` attribute, and then associates that value with both endpoints\.

## Record additional attributes for auditing purposes<a name="tutorials-two-way-sms-next-steps-auditing"></a>

This solution records two valuable attributes when it creates and updates endpoints\. First, when the first Lambda function initially creates the endpoint, it records the URL of the form itself in the `Attributes.Source` attribute\. If the customer responds to the message, the second Lambda function creates an `Attributes.OptInTimestamp` attribute\. This attribute contains the exact date and time when the customer provided their consent to receive messages from you\.

Both of these fields can be useful if you're ever asked by a mobile carrier or regulatory agency to provide evidence of a customer's consent\. You can retrieve this information at any time by using the [GetEndpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#GetEndpoint) API operation\.

You can also modify the Lambda functions to record additional data that might be useful for auditing purposes, such as the IP address that the registration request was submitted from\.
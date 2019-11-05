# Step 6: Create and Deploy the Web Form<a name="tutorials-email-prefs-part-6"></a>

All of the components of this solution that use AWS services are now in place\. The last step is to create and deploy the web form that captures customer's data\.

## Step 6\.1: Create the JavaScript Form Handler<a name="tutorials-email-prefs-part-6-create-form-handler"></a>

In this section, you create a JavaScript function that parses the content of the web form that you create in the next section\. After parsing the content, this function sends the data to the API that you created in [Part 4](tutorials-email-prefs-part-4.md)\.

**To create the form handler**

1. In a text editor, create a new file\.

1. In the editor, paste the following code\.

   ```
   $(document).ready(function() {
   
     // Function that parses URL parameters.
     function getParameterByName(name) {
       name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
       var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
         results = regex.exec(location.search);
       return results == null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
     }
   
     // Pre-fill form data.
     $("#firstName").val(getParameterByName("firstName"));
     $("#lastName").val(getParameterByName("lastName"));
     $("#email").val(getParameterByName("email"));
     $("#endpointId").val(getParameterByName("endpointId"));
     if (getParameterByName("t1") == "OptIn") {
       $("#topic1In").prop('checked', true);
     }
     if (getParameterByName("t2") == "OptIn") {
       $("#topic2In").prop('checked', true);
     }
     if (getParameterByName("t3") == "OptIn") {
       $("#topic3In").prop('checked', true);
     }
     if (getParameterByName("t4") == "OptIn") {
       $("#topic4In").prop('checked', true);
     }
   
     // Handle form submission.
     $("#submit").click(function(e) {
   
       // Get endpoint ID from URL parameter.
       var endpointId = getParameterByName("endpointId");
   
       var firstName = $("#firstName").val(),
         lastName = $("#lastName").val(),
         email = $("#email").val(),
         source = window.location.pathname,
         optTimestamp = undefined,
         utcSeconds = Date.now() / 1000,
         timestamp = new Date(0);
   
       var topicOptIn = [
         $('#topic1In').is(':checked'),
         $('#topic2In').is(':checked'),
         $('#topic3In').is(':checked'),
         $('#topic4In').is(':checked')
       ];
   
       e.preventDefault();
   
       $('#submit').prop('disabled', true);
       $('#submit').html('<span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>  Saving your preferences</button>');
   
       // If customer unchecks a box, or leaves a box unchecked, set the opt
       // status to "OptOut".
       for (i = 0; i < topicOptIn.length; i++) {
         if (topicOptIn[i] == true) {
           topicOptIn[i] = "OptIn";
         } else {
           topicOptIn[i] = "OptOut";
         }
       }
   
       timestamp.setUTCSeconds(utcSeconds);
   
       var data = JSON.stringify({
         'endpointId': endpointId,
         'firstName': firstName,
         'lastName': lastName,
         'topic1': topicOptIn[0],
         'topic2': topicOptIn[1],
         'topic3': topicOptIn[2],
         'topic4': topicOptIn[3],
         'source': source,
         'optTimestamp': timestamp.toString(),
         'optOutAll': false
       });
   
       $.ajax({
         type: 'POST',
         url: 'https://example.execute-api.us-east-1.amazonaws.com/v1/prefs',
         contentType: 'application/json',
         data: data,
         success: function(res) {
           $('#form-response').html('<div class="mt-3 alert alert-success" role="alert">Your preferences have been saved!</div>');
           $('#submit').prop('hidden', true);
           $('#unsubAll').prop('hidden', true);
           $('#submit').text('Preferences saved!');
         },
         error: function(jqxhr, status, exception) {
           $('#form-response').html('<div class="mt-3 alert alert-danger" role="alert">An error occurred. Please try again later.</div>');
           $('#submit').text('Save preferences');
           $('#submit').prop('disabled', false);
         }
       });
     });
   
     // Handle the case when the customer clicks the "Unsubscribe from all"
     // button.
     $("#unsubAll").click(function(e) {
       var firstName = $("#firstName").val(),
         lastName = $("#lastName").val(),
         source = window.location.pathname,
         optTimestamp = undefined,
         utcSeconds = Date.now() / 1000,
         timestamp = new Date(0);
   
       // Get endpoint ID from URL parameter.
       var endpointId = getParameterByName("endpointId");
   
       e.preventDefault();
   
       $('#unsubAll').prop('disabled', true);
       $('#unsubAll').html('<span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>  Saving your preferences</button>');
   
       // Uncheck all boxes to give user a visual representation of the opt-out action.
       $("#topic1In").prop('checked', false);
       $("#topic2In").prop('checked', false);
       $("#topic3In").prop('checked', false);
       $("#topic4In").prop('checked', false);
   
       timestamp.setUTCSeconds(utcSeconds);
   
       var data = JSON.stringify({
         'endpointId': endpointId,
         'source': source,
         'optTimestamp': timestamp.toString(),
         'optOutAll': true
       });
   
       $.ajax({
         type: 'POST',
         url: 'https://example.execute-api.us-east-1.amazonaws.com/v1/prefs',
         contentType: 'application/json',
         data: data,
         success: function(res) {
           $('#form-response').html('<div class="mt-3 alert alert-info" role="alert">Successfully opted you out of all future email communications.</div>');
           $('#submit').prop('hidden', true);
           $('#unsubAll').prop('hidden', true);
         },
         error: function(jqxhr, status, exception) {
           $('#form-response').html('<div class="mt-3 alert alert-danger" role="alert">An error occurred. Please try again later.</div>');
         }
       });
     });
   });
   ```

1. In the preceding example, replace *https://example\.execute\-api\.us\-east\-1\.amazonaws\.com/v1/prefs* with the **Invoke URL** that you obtained in [Step 5\.3](tutorials-email-prefs-part-5.md#tutorials-email-prefs-part-5-deploy-api)\.

1. Save the file\.

## Step 6\.2: Create the Form File<a name="tutorials-email-prefs-part-6-create-form"></a>

In this section, you create an HTML file that contains the form that customers use to manage their email preferences\. This file uses the JavaScript form handler that you created in the previous section to transmit the form data to your Lambda function\.

**Important**  
When a user submits this form, it triggers a Lambda function that updates endpoints in your Amazon Pinpoint project\. Malicious users could launch an attack on your form that could impact your data or cause a large number of requests to be made\. If you plan to use this solution for a production use case, you should secure it by using a system such as [Google reCAPTCHA](https://www.google.com/recaptcha/intro/v3.html)\.

**To create the form**

1. In a text editor, create a new file\.

1. In the editor, paste the following code\.

   ```
   <!doctype html>
   <html lang="en">
   
   <head>
     <!-- Required meta tags -->
     <meta charset="utf-8">
     <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
     <!-- Bootstrap CSS -->
     <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
     <!-- Bootstrap and AJAX scripts -->
     <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
     <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
     <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
   
     <!-- Reference to JavaScript form handler. -->
     <script type="text/javascript" src="formHandler.js"></script>
   
     <title>Manage your email preferences</title>
   </head>
   
   <body>
     <div class="container">
       <h1>Manage your email subscriptions</h1>
       <h2 class="mt-3">Contact information</h2>
       <form>
   
         <div class="form-row mt-3">
           <div class="form-group col-md-6">
             <label for="email" class="font-weight-bold">Email address</label>
             <input type="email" class="form-control-plaintext" id="email" readonly>
           </div>
           <div class="form-group col-md-6">
             <label for="email" class="font-weight-bold">ID</label>
             <input type="email" class="form-control-plaintext" id="endpointId" readonly>
           </div>
         </div>
         <div class="form-row">
           <div class="form-group col-md-6">
             <label for="firstName" class="font-weight-bold">First name</label>
             <input type="text" class="form-control" id="firstName">
           </div>
           <div class="form-group col-md-6">
             <label for="lastName" class="font-weight-bold">Last name</label>
             <input type="text" class="form-control" id="lastName">
           </div>
         </div>
   
         <div class="form-row">
           <div class="col-md-12">
             <p class="font-weight-bold mt-3">Subscriptions</p>
           </div>
         </div>
   
         <!-- Change topic names here, if necessary -->
         <div class="form-row">
           <div class="col-md-6">
             <div class="custom-control custom-switch">
               <input type="checkbox" class="custom-control-input" id="topic1In">
               <label class="custom-control-label font-weight-bold" for="topic1In">Special offers</label>
               <p>Get exclusive offers available only to subscribers.</p>
             </div>
           </div>
           <div class="col-md-6">
             <div class="custom-control custom-switch">
               <input type="checkbox" class="custom-control-input" id="topic2In">
               <label class="custom-control-label font-weight-bold" for="topic2In">Coming soon</label>
               <p>Learn about our newest products before anyone else!</p>
             </div>
           </div>
         </div>
   
         <div class="form-row mt-3">
           <div class="col-md-6">
             <div class="custom-control custom-switch">
               <input type="checkbox" class="custom-control-input" id="topic3In">
               <label class="custom-control-label font-weight-bold" for="topic3In">New products</label>
               <p>Get the inside scoop on products that haven't been released yet.</p>
             </div>
           </div>
           <div class="col-md-6">
             <div class="custom-control custom-switch">
               <input type="checkbox" class="custom-control-input" id="topic4In">
               <label class="custom-control-label font-weight-bold" for="topic4In">Deal of the Day</label>
               <p>Get special deals in your inbox every day!</p>
             </div>
           </div>
         </div>
   
         <div id="form-response"></div>
   
         <div class="row mt-3">
           <div class="col-md-12 text-center">
             <button type="submit" id="submit" class="btn btn-primary">Update preferences</button>
           </div>
         </div>
   
         <div class="row mt-3">
           <div class="col-md-12 text-center">
             <button type="button" class="btn btn-link" id="unsubAll">Unsubscribe from all email communications</button>
           </div>
         </div>
       </form>
   
       <div class="row mt-3">
         <div class="col-md-12 text-center">
           <small class="text-muted">Copyright © 2019, ExampleCorp or its affiliates.</small>
         </div>
       </div>
   
     </div>
   </body>
   
   </html>
   ```

1. In the preceding example, replace *formHandler\.js* with the full path to the form handler JavaScript file that you created in the previous section\.

1. Save the file\.

## Step 6\.3: Upload the Form Files<a name="tutorials-email-prefs-part-6-upload-form"></a>

Now that you've created the HTML form and the JavaScript form handler, the last step is to publish these files to the internet\. This section assumes that you have an existing web hosting provider\. If you don't have an existing hosting provider, you can launch a website by using Amazon Route 53, Amazon Simple Storage Service \(Amazon S3\), and Amazon CloudFront\. For more information, see [Host a Static Website](https://aws.amazon.com/getting-started/projects/host-static-website/)\.

If you use another web hosting provider, consult the provider's documentation for more information about publishing webpages\.

## Step 6\.4: Test the Form<a name="tutorials-email-prefs-part-6-test-form"></a>

After you publish the form, you should test it to confirm that it works properly\.

### Step 6\.4\.1: Use the Form to Submit Test Data<a name="tutorials-email-prefs-part-6-test-form-data"></a>

The first step in testing the preferences form is to use it to submit some test data\. In this case, you opt in to all of the subscription topics that are listed on the form\.

**To submit test data**

1. In a web browser, go to the location where you uploaded the preferences page\. If you used the code example from [Step 6\.2](#tutorials-email-prefs-part-6-create-form), you see a form that resembles the example in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_Test_Step6.3.1_1.png)

1. Add the following string to the end of the URL\. Replace the values for *firstName*, *lastName*, and *endpointId* with the values that you specified in Step 2\. 

   ```
   ?firstName=Carlos&lastName=Salazar&email=recipient%40example.com&endpointId=12345
   ```

   When you add these attributes to the end of the URL and press Enter, the page updates to include the information in the attribute string\. The form should resemble the example in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_Test_Step6.3.1_2.png)

1. On the form, use the toggle switches to opt the endpoint into all of the topics, and then choose **Update preferences**\.

1. Wait for approximately one minute, and then proceed to the next section\.

### Step 6\.4\.2: Check the Opt Status of the Test Endpoint<a name="tutorials-email-prefs-part-6-test-form-segment"></a>

Now that you've submitted some test data, you can use the segmentation tool in the Amazon Pinpoint console to make sure that the test data was handled properly\.

**To check the endpoint opt status**

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\.

1. Choose **Create a segment**\.

1. Under **Segment group 1**, choose **Add a filter**, and then choose **Filter by endpoint**\.

1. Choose **Choose an endpoint attribute**, and then choose **ComingSoonOptStatus**\. Set the value of the filter to **OptIn**\.

1. Choose **Add an attribute or metric**\. Add the **DealOfTheDay** attribute to the filter, and set the value to **OptIn**\.

1. Repeat the previous step for the two remaining opt topics: **NewProductsOptStatus** and **SpecialOffersOptStatus**\.

   When you finish, the **Segment estimate** section should indicate that the number of **Eligible endpoints** and **Total endpoints** are both 1\. The page should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_Test_Step6.3.2_8.png)

### Step 6\.4\.3: Test the "Unsubscribe From All" Functionality<a name="tutorials-email-prefs-part-6-test-form-unsub-all"></a>

Also make sure that the **Unsubscribe from all email communications** button on the preferences form works properly\. When a customer chooses this button, their opt status for all topics is set to "OptOut"\. Additionally, the OptOut attribute for the customer's endpoint record is set to "ALL"\. This change prevents the endpoint from being included in segments that you create in this project in the future\.

**To test the unsubscribe button**

1. In a web browser, go to the location where you uploaded the preferences page\.

1. Add the following string to the end of the URL\. Replace the values for *firstName*, *lastName*, and *endpointId* with the values that you specified in Step 2\. 

   ```
   ?firstName=Carlos&lastName=Salazar&email=recipient%40example.com&endpointId=12345&t1=OptIn&t2=OptIn&t3=OptIn&t4=OptIn
   ```

   When you add these attributes to the end of the URL and press Enter, the page updates to include the information in the attribute string\.

1. On the form, choose **Unsubscribe from all email communications**\.

1. Wait for approximately one minute, and then proceed to the next section\.

### Step 6\.3\.4: Check the Opt Status of the Test Endpoint<a name="tutorials-email-prefs-part-6-test-form-unsub-all-test"></a>

The final step in testing the preference form is to confirm that unsubscribe requests are handled properly\. In this section, you use the segmentation tool in the Amazon Pinpoint console to ensure that your test endpoint is opted out of email communications that are sent from the current Amazon Pinpoint project\.

**To check the endpoint opt status**

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose the project that you created in [Step 1\.1](tutorials-email-prefs-part-1.md#tutorials-email-prefs-part-1-create-project)\.

1. In the navigation pane, choose **Segments**\.

1. Choose **Create a segment**\.

1. Note the values in the **Segment estimate** section\. This section should indicate that the number of **Eligible endpoints** is 0, and the number of **Total endpoints** is 1\. The page should resemble the example shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/Email_Prefs_Tutorial_Test_Step6.3.4_5.png)

### Troubleshooting the Form<a name="tutorials-email-prefs-part-6-test-form-troubleshooting"></a>

If you perform these test steps, but the data in your Amazon Pinpoint project doesn't change, there are a few steps that you can take to troubleshoot the issue:
+ In the Amazon Pinpoint console, make sure that you're using the correct AWS Region\. Segments and endpoints aren't shared between Regions\.
+ In the Lambda console, open the function that you created in [Step 4](tutorials-email-prefs-part-4.md)\. On the **Monitoring** tab, choose **View logs in CloudWatch**\.

  Under **Log Streams**, choose the most recently logged event\. Check the output of the event for more information about the issue\. For example, if you see an "AccessDeniedException" error in the logs, make sure that the Amazon Pinpoint project ID and AWS Region that you entered in [Step 4](tutorials-email-prefs-part-4.md) are equal to the project ID and Region values that you specified in the IAM policy in [Step 3\.1](tutorials-email-prefs-part-3.md#tutorials-email-prefs-part-3-create-policy)\. If you recently changed these values, wait for a few minutes, and then try again\.
+ If CloudWatch doesn't contain any logged information for the function, make sure that CORS is enabled in API Gateway, and that the API is deployed\. If you're unsure, repeat the steps in [Step 5\.3](tutorials-email-prefs-part-5.md#tutorials-email-prefs-part-5-deploy-api)\.

**Next**: [Create and Send Amazon Pinpoint Campaigns](tutorials-email-prefs-part-7.md)
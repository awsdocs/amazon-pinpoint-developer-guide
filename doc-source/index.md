# Amazon Pinpoint Developer Guide

-----
*****Copyright &copy; 2019 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is Amazon Pinpoint?](welcome.md)
+ [Integrating Amazon Pinpoint with Your Application](integrate.md)
   + [AWS SDK Support for Amazon Pinpoint](integrate-supported-sdks.md)
   + [Integrating the AWS Mobile SDKs or JavaScript Library with Your Application](integrate-sdk.md)
   + [Registering Endpoints in Your Application](integrate-endpoints.md)
   + [Reporting Events in Your Application](integrate-events.md)
   + [Handling Push Notifications](integrate-push.md)
      + [Setting Up Push Notifications for Amazon Pinpoint](mobile-push.md)
         + [Setting Up iOS Push Notifications](apns-setup.md)
         + [Setting Up Android Push Notifications](mobile-push-android.md)
         + [Create a Project in Amazon Pinpoint](mobile-push-create-project.md)
      + [Handling Push Notifications](integrate-push-services.md)
+ [Defining Your Audience to Amazon Pinpoint](audience-define.md)
   + [Adding Endpoints to Amazon Pinpoint](audience-define-endpoints.md)
   + [Associating Users with Amazon Pinpoint Endpoints](audience-define-user.md)
   + [Adding a Batch of Endpoints to Amazon Pinpoint](audience-define-endpoints-batch.md)
   + [Importing Endpoints into Amazon Pinpoint](audience-define-import.md)
   + [Deleting Endpoints from Amazon Pinpoint](audience-define-remove.md)
+ [Accessing Audience Data in Amazon Pinpoint](audience-data.md)
   + [Looking Up Endpoints with Amazon Pinpoint](audience-data-endpoints.md)
   + [Exporting Endpoints from Amazon Pinpoint](audience-data-export.md)
   + [Listing Endpoint IDs with Amazon Pinpoint](audience-data-list-ids.md)
+ [Creating Segments](segments.md)
   + [Building Segments](segments-dimensional.md)
   + [Importing Segments](segments-importing.md)
   + [Customizing Segments with AWS Lambda](segments-dynamic.md)
+ [Creating Campaigns](campaigns.md)
+ [Streaming Amazon Pinpoint Events to Kinesis](analytics-streaming.md)
+ [Logging Amazon Pinpoint API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Deleting Data from Amazon Pinpoint](deleting-data.md)
+ [Permissions](permissions.md)
   + [IAM Policies for Amazon Pinpoint Users](permissions-actions.md)
   + [User Authentication in Amazon Pinpoint Apps](permissions-authentication.md)
   + [AWS Mobile Hub Service Role](permissions-mobilehub.md)
   + [IAM Role for Importing Endpoints or Segments](permissions-import-segment.md)
   + [IAM Role for Exporting Endpoints or Segments](permissions-export-endpoints.md)
   + [IAM Role for Streaming Events to Kinesis](permissions-streams.md)
+ [Limits in Amazon Pinpoint](limits.md)
+ [Creating Custom Channels with AWS Lambda](channels-custom.md)
+ [Document History for Amazon Pinpoint](doc-history.md)
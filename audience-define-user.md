# Associating Users with Amazon Pinpoint Endpoints<a name="audience-define-user"></a>

An endpoint can include attributes that define a *user*, which represents a person in your audience\. For example, a user might represent someone who installed your mobile app, or someone who has an account on your website\. 

You define a user by specifying a unique user ID and, optionally, custom user attributes\. If someone uses your app on multiple devices, or if that person can be messaged at multiple addresses, you can assign the same user ID to multiple endpoints\. In this case, Amazon Pinpoint synchronizes user attributes across the endpoints\. So, if you add a user attribute to one endpoint, Amazon Pinpoint adds that attribute to each endpoint that includes the same user ID\.

You can add user attributes to track data that applies to an individual and doesn't vary based on which device the person is using\. For example, you can add attributes for a person's name, age, or account status\. 

**Tip**  
If your application uses Amazon Cognito user pools to handle user authentication, Amazon Cognito can add user IDs and attributes to your endpoints automatically\. For the endpoint user ID value, Amazon Cognito assigns the `sub` value that's assigned to the user in the user pool\. To learn about adding users with Amazon Cognito, see [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-pinpoint-integration.html) in the *Amazon Cognito Developer Guide*\.

After you add user definitions to your endpoints, you have more options for how you segment your audience\. You can define a segment based on user attributes, or you can define a segment by importing a list of user IDs\. When you send a message to a segment that's based on users, the potential destinations include each endpoint that's associated with each user in the segment\.

You also have more options for how you message your audience\. You can use a campaign to message a segment of users, or you can send a message directly to a list of user IDs\. To personalize your message, you can include message variables that are substituted with user attribute values\.

## Examples<a name="audience-define-user-example"></a>

The following examples show you how to add a user definition to an endpoint\.

------
#### [ AWS CLI ]

You can use Amazon Pinpoint by running commands with the AWS CLI\.

**Example Update Endpoint Command**  
To add a user to an endpoint, use the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/pinpoint/update-endpoint.html) command\. For the `--endpoint-request` parameter, you can define a new endpoint, which can include a user\. Or, to update an existing endpoint, you can provide just the attributes that you want to change\. The following example adds a user to an existing endpoint by providing only the user attributes:  

```
$ aws pinpoint update-endpoint \
> --application-id application-id \
> --endpoint-id endpoint-id \
> --endpoint-request file://endpoint-request-file.json
```
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating an endpoint\.
+ *endpoint\-id* is the ID that you're assigning to a new endpoint, or it's the ID of an existing endpoint that you're updating\.
+ *endpoint\-request\-file\.json* is the file path to a local JSON file that contains the input for the `--endpoint-request` parameter\.

**Example Endpoint Request File**  
The example `update-endpoint` command uses a JSON file as the argument for the `--endpoint-request` parameter\. This file contains a user definition like the following:  

```
{ 
    "User":{ 
        "UserId":"example_user",
        "UserAttributes":{ 
            "FirstName":"Wang",
            "LastName":"Xiulan",
            "Gender":"Female",
            "Age":"39"
        }
    }
}
```
For the attributes that you can use to define a user, see the `User` object in the [EndpointRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#apps-application-id-endpoints-endpoint-id-schemas) schema in the *Amazon Pinpoint API Reference*\.

------
#### [ AWS SDK for Java ]

You can use the Amazon Pinpoint API in your Java applications by using the client that's provided by the AWS SDK for Java\.

**Example Code**  
To add a user to an endpoint, initialize an [EndpointRequest](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointRequest.html) object, and pass it to the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpoint-com.amazonaws.services.pinpoint.model.UpdateEndpointRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/AmazonPinpointClient.html#updateEndpoint-com.amazonaws.services.pinpoint.model.UpdateEndpointRequest-) method of the `AmazonPinpoint` client\. You can use this object to define a new endpoint, which can include a user\. Or, to update an existing endpoint, you can update just the properties that you want to change\. The following example adds a user to an existing endpoint by adding an [EndpointUser](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/pinpoint/model/EndpointUser.html) object to the EndpointRequest object:  

```
import com.amazonaws.regions.Regions;
import com.amazonaws.services.pinpoint.AmazonPinpoint;
import com.amazonaws.services.pinpoint.AmazonPinpointClientBuilder;
import com.amazonaws.services.pinpoint.model.EndpointRequest;
import com.amazonaws.services.pinpoint.model.EndpointUser;
import com.amazonaws.services.pinpoint.model.UpdateEndpointRequest;
import com.amazonaws.services.pinpoint.model.UpdateEndpointResult;

import java.util.Arrays;
import java.util.Collections;

public class AddExampleUser {

    public static void main(String[] args) {

        final String USAGE = "\n" +
                "AddExampleUser - Adds a user definition to the specified Amazon Pinpoint endpoint." +
                "Usage: AddExampleUser <endpointId> <applicationId>" +
                "Where:\n" +
                "  endpointId - The ID of the endpoint to add the user to." +
                "  applicationId - The ID of the Amazon Pinpoint application that contains the endpoint.";

        if (args.length < 1) {
            System.out.println(USAGE);
            System.exit(1);
        }

        String endpointId = args[0];
        String applicationId = args[1];

        // Creates a new user definition.
        EndpointUser wangXiulan = new EndpointUser().withUserId("example_user");

        // Assigns custom user attributes.
        wangXiulan.addUserAttributesEntry("name", Arrays.asList("Wang", "Xiulan"));
        wangXiulan.addUserAttributesEntry("gender", Collections.singletonList("female"));
        wangXiulan.addUserAttributesEntry("age", Collections.singletonList("39"));

        // Adds the user definition to the EndpointRequest that is passed to the Amazon Pinpoint client.
        EndpointRequest wangXiulansIphone = new EndpointRequest()
                .withUser(wangXiulan);

        // Initializes the Amazon Pinpoint client.
        AmazonPinpoint pinpointClient = AmazonPinpointClientBuilder.standard()
                .withRegion(Regions.US_EAST_1).build();

        // Updates the specified endpoint with Amazon Pinpoint.
        UpdateEndpointResult result = pinpointClient.updateEndpoint(new UpdateEndpointRequest()
                .withEndpointRequest(wangXiulansIphone)
                .withApplicationId(applicationId)
                .withEndpointId(endpointId));

        System.out.format("Update endpoint result: %s\n", result.getMessageBody().getMessage());

    }
}
```

------
#### [ HTTP ]

You can use Amazon Pinpoint by making HTTP requests directly to the REST API\.

**Example Put Endpoint Request with User Definition**  
To add a user to an endpoint, issue a `PUT` request to the [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) resource at the following URI:  
`/v1/apps/application-id/endpoints/endpoint-id`  
Where:  
+ *application\-id* is the ID of the Amazon Pinpoint project in which you're adding or updating an endpoint\.
+ *endpoint\-id* is the ID that you're assigning to a new endpoint, or it's the ID of an existing endpoint that you're updating\.
In your request, include the required headers, and provide the [EndpointRequest](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html#apps-application-id-endpoints-endpoint-id-schemas) JSON as the body\. The request body can define a new endpoint, which can include a user\. Or, to update an existing endpoint, you can provide just the attributes that you want to change\. The following example adds a user to an existing endpoint by providing only the user attributes:  

```
PUT /v1/apps/application_id/endpoints/example_endpoint HTTP/1.1
Host: pinpoint.us-east-1.amazonaws.com
X-Amz-Date: 20180415T182538Z
Content-Type: application/json
Accept: application/json
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20180501/us-east-1/mobiletargeting/aws4_request, SignedHeaders=accept;content-length;content-type;host;x-amz-date, Signature=c25cbd6bf61bd3b3667c571ae764b9bf2d8af61b875cacced95d1e68d91b4170
Cache-Control: no-cache

{ 
    "User":{ 
        "UserId":"example_user",
        "UserAttributes":{ 
            "FirstName":"Wang",
            "LastName":"Xiulan",
            "Gender":"Female",
            "Age":"39"
        }
    }
}
```
If the request succeeds, you receive a response like the following:  

```
{
    "RequestID": "67e572ed-41d5-11e8-9dc5-db288f3cbb72",
    "Message": "Accepted"
}
```

------

## Related Information<a name="audience-define-user-related"></a>

For more information about the Endpoint resource in the Amazon Pinpoint API, including supported HTTP methods and request parameters, see [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) in the *Amazon Pinpoint API Reference\.*

For more information about personalizing messages with variables, see [Message Variables](https://docs.aws.amazon.com/pinpoint/latest/userguide/campaigns-message.html#campaigns-message-variables.html) in the *Amazon Pinpoint User Guide*\.

To learn how to define a segment by importing a list of user IDs, see [Importing Segments](https://docs.aws.amazon.com/pinpoint/latest/userguide/segments-importing.html) in the *Amazon Pinpoint User Guide*\.

For information about sending a direct message to up to 100 user IDs, see [Users Messages](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-users-messages.html) in the *Amazon Pinpoint API Reference*\.

For information about the quotas that apply to endpoints, including the number of user attributes that you can assign, see [Endpoint Quotas](quotas.md#quotas-endpoint)\.
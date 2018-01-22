# Adding Endpoints<a name="endpoints"></a>

An *endpoint* uniquely identifies a user device to which you can send push notifications with Amazon Pinpoint\. You can assign attributes to endpoints to describe your users\.

After you add endpoints to Amazon Pinpoint, you can segment your audience based on endpoint attributes, and you can engage these segments with tailored messaging campaigns\.

While the endpoint uniquely identifies the mobile device, you can assign user IDs to endpoints to uniquely identify users\. You can assign a single user ID to multiple endpoints\. In this case, the user ID would represent an individual who uses your app on multiple devices, such as an iPhone and an iPad\.

If your app is enabled with Amazon Pinpoint support, your app automatically registers an endpoint with Amazon Pinpoint when a new user opens the app\. You can also add endpoints programmatically with the Amazon Pinpoint API or the AWS SDK for Java\.

For more information, see [Endpoints](http://docs.aws.amazon.com/pinpoint/latest/apireference/rest-api-endpoints.html) in the *Amazon Pinpoint API Reference*\.

## Adding Endpoints with the AWS SDK for Java<a name="endpoints-example-java"></a>

The following example demonstrates how to add endpoints to Amazon Pinpoint with the AWS SDK for Java\.

```
import com.amazonaws.services.pinpoint.AmazonPinpointClient;
import com.amazonaws.services.pinpoint.model.EndpointDemographic;
import com.amazonaws.services.pinpoint.model.EndpointLocation;
import com.amazonaws.services.pinpoint.model.EndpointRequest;
import com.amazonaws.services.pinpoint.model.EndpointResponse;
import com.amazonaws.services.pinpoint.model.EndpointUser;
import com.amazonaws.services.pinpoint.model.GetEndpointRequest;
import com.amazonaws.services.pinpoint.model.GetEndpointResult;
import com.amazonaws.services.pinpoint.model.UpdateEndpointRequest;
import com.amazonaws.services.pinpoint.model.UpdateEndpointResult;

import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.UUID;

public class PinpointEndpointSample {

    public EndpointResponse createEndpoint(AmazonPinpointClient client, String appId) {
        String endpointId = UUID.randomUUID().toString();
        System.out.println("Endpoint ID: " + endpointId);

        EndpointRequest endpointRequest = createEndpointRequestData();

        UpdateEndpointRequest updateEndpointRequest = new UpdateEndpointRequest()
                .withApplicationId(appId)
                .withEndpointId(endpointId)
                .withEndpointRequest(endpointRequest);

        UpdateEndpointResult updateEndpointResponse = client.updateEndpoint(updateEndpointRequest);
        System.out.println("Update Endpoint Response: " + updateEndpointResponse.getMessageBody());

        GetEndpointRequest getEndpointRequest = new GetEndpointRequest()
                .withApplicationId(appId)
                .withEndpointId(endpointId);
        GetEndpointResult getEndpointResult = client.getEndpoint(getEndpointRequest);

        System.out.println("Got Endpoint: " + getEndpointResult.getEndpointResponse().getId());
        return getEndpointResult.getEndpointResponse();
    }

    private EndpointRequest createEndpointRequestData() {

        HashMap<String, List<String>> customAttributes = new HashMap<>();
        List<String> favoriteTeams = new ArrayList<>();
        favoriteTeams.add("Lakers");
        favoriteTeams.add("Warriors");
        customAttributes.put("team", favoriteTeams);


        EndpointDemographic demographic = new EndpointDemographic()
                .withAppVersion("1.0")
                .withMake("apple")
                .withModel("iPhone")
                .withModelVersion("7")
                .withPlatform("ios")
                .withPlatformVersion("10.1.1")
                .withTimezone("Americas/Los_Angeles");

        EndpointLocation location = new EndpointLocation()
                .withCity("Los Angeles")
                .withCountry("US")
                .withLatitude(34.0)
                .withLongitude(-118.2)
                .withPostalCode("90068")
                .withRegion("CA");

        Map<String,Double> metrics = new HashMap<>();
        metrics.put("health", 100.00);
        metrics.put("luck", 75.00);

        EndpointUser user = new EndpointUser()
                .withUserId(UUID.randomUUID().toString());

        EndpointRequest endpointRequest = new EndpointRequest()
                .withAddress(UUID.randomUUID().toString())
                .withAttributes(customAttributes)
                .withChannelType("APNS")
                .withDemographic(demographic)
                .withEffectiveDate(new Date().toString())
                .withLocation(location)
                .withMetrics(metrics)
                .withOptOut("NONE")
                .withRequestId(UUID.randomUUID().toString())
                .withUser(user);

        return endpointRequest;
    }
}
```

When you run this example, the following is printed to the console window of your IDE\.

```
Endpoint ID: 37d321e8-5419-4fa8-86ba-698905f262a4
Update Endpoint Response: {Message: Accepted,RequestID: 74ef5959-b4d7-11e6-ae27-25eb3a23dee7}
Get Endpoint ID: 37d321e8-5419-4fa8-86ba-698905f262a4
```
# Update an Amazon Pinpoint channel<a name="example_pinpoint_GetSmsChannel_section"></a>

The following code example shows how to update channels\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
  

```
    private static SMSChannelResponse getSMSChannel(PinpointClient client, String appId) {

        try {
            GetSmsChannelRequest request = GetSmsChannelRequest.builder()
                .applicationId(appId)
                .build();

            SMSChannelResponse response = client.getSmsChannel(request).smsChannelResponse();
            System.out.println("Channel state is " + response.enabled());
            return response;

        } catch ( PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return null;
    }

    private static void toggleSmsChannel(PinpointClient client, String appId, SMSChannelResponse getResponse) {
        boolean enabled = !getResponse.enabled();

        try {
            SMSChannelRequest request = SMSChannelRequest.builder()
                .enabled(enabled)
                .build();

            UpdateSmsChannelRequest updateRequest = UpdateSmsChannelRequest.builder()
                .smsChannelRequest(request)
                .applicationId(appId)
                .build();

            UpdateSmsChannelResponse result = client.updateSmsChannel(updateRequest);
            System.out.println("Channel state: " + result.smsChannelResponse().enabled());

        } catch ( PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [GetSmsChannel](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/GetSmsChannel) in *AWS SDK for Java 2\.x API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.
# Create an Amazon Pinpoint campaign<a name="example_pinpoint_CreateCampaign_section"></a>

The following code examples show how to create a campaign\.

------
#### [ Java ]

**SDK for Java 2\.x**  
Create a campaign\.  

```
    public static void createPinCampaign(PinpointClient pinpoint, String appId, String segmentId) {

        CampaignResponse result = createCampaign(pinpoint, appId, segmentId);
        System.out.println("Campaign " + result.name() + " created.");
        System.out.println(result.description());

    }

    public static CampaignResponse createCampaign(PinpointClient client, String appID, String segmentID) {

        try {
            Schedule schedule = Schedule.builder()
                    .startTime("IMMEDIATE")
                    .build();

            Message defaultMessage = Message.builder()
                    .action(Action.OPEN_APP)
                    .body("My message body.")
                    .title("My message title.")
                    .build();

            MessageConfiguration messageConfiguration = MessageConfiguration.builder()
                    .defaultMessage(defaultMessage)
                    .build();

            WriteCampaignRequest request = WriteCampaignRequest.builder()
                    .description("My description")
                    .schedule(schedule)
                    .name("MyCampaign")
                    .segmentId(segmentID)
                    .messageConfiguration(messageConfiguration)
                    .build();

            CreateCampaignResponse result = client.createCampaign(
                    CreateCampaignRequest.builder()
                            .applicationId(appID)
                            .writeCampaignRequest(request).build()
            );

            System.out.println("Campaign ID: " + result.campaignResponse().id());

            return result.campaignResponse();

        } catch (PinpointException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }

        return null;
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/pinpoint#readme)\. 
+  For API details, see [CreateCampaign](https://docs.aws.amazon.com/goto/SdkForJavaV2/pinpoint-2016-12-01/CreateCampaign) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun createPinCampaign(appId: String, segmentIdVal: String) {

            val scheduleOb = Schedule {
                startTime = "IMMEDIATE"
            }

            val defaultMessageOb = Message {
                action = Action.OpenApp
                body = "My message body"
                title = "My message title"
            }

            val messageConfigurationOb = MessageConfiguration {
                defaultMessage = defaultMessageOb
            }

            val writeCampaign = WriteCampaignRequest {
                description = "My description"
                schedule = scheduleOb
                name = "MyCampaign"
                segmentId = segmentIdVal
                messageConfiguration= messageConfigurationOb
            }

            PinpointClient { region = "us-west-2" }.use { pinpoint ->
             val result: CreateCampaignResponse = pinpoint.createCampaign(CreateCampaignRequest {
                 applicationId = appId
                 writeCampaignRequest = writeCampaign
             })
             println("Campaign ID is ${result.campaignResponse?.id}")
        }
 }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/pinpoint#code-examples)\. 
+  For API details, see [CreateCampaign](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using Amazon Pinpoint with an AWS SDK](sdk-general-information-section.md)\.
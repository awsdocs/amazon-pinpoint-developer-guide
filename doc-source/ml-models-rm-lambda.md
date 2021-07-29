# Customizing recommendations with AWS Lambda<a name="ml-models-rm-lambda"></a>

In Amazon Pinpoint, you can retrieve personalized recommendations from a recommender model and add them to messages that you send from campaigns and journeys\. A *recommender model* is a type of machine learning \(ML\) model that finds patterns in data and generates predictions and recommendations based on the patterns that it finds\. It predicts what a particular user will prefer from a given set of products or items, and it provides that information as a set of recommendations for the user\.

By using recommender models with Amazon Pinpoint, you can send personalized recommendations to message recipients based on each recipient’s attributes and behavior\. With AWS Lambda, you can also customize and enhance these recommendations\. For example, you can dynamically transform a recommendation from a single text value \(such as a product name or ID\) to more sophisticated content \(such as a product name, description, and image\)\. And you can do it in real time, when Amazon Pinpoint sends the message\.

This feature is available in the following AWS Regions: US East \(N\. Virginia\); US West \(Oregon\); Asia Pacific \(Mumbai\); Asia Pacific \(Sydney\); and, Europe \(Ireland\)\.

**Topics**
+ [Using recommendations in messages](#ml-models-rm-lambda-overview)
+ [Creating the Lambda function](#ml-models-rm-lambda-create-function)
+ [Assigning a Lambda function policy](#ml-models-rm-lambda-trust-policy)
+ [Authorizing Amazon Pinpoint to invoke the function](#ml-models-rm-lambda-trust-policy-assign)
+ [Configuring the Recommender Model](#ml-models-rm-lambda-configure)

## Using recommendations in messages<a name="ml-models-rm-lambda-overview"></a>

To use a recommender model with Amazon Pinpoint, you start by creating an Amazon Personalize solution and deploying that solution as an Amazon Personalize campaign\. Then, you create a configuration for the recommender model in Amazon Pinpoint\. In the configuration, you specify settings that determine how to retrieve and process recommendation data from the Amazon Personalize campaign\. This includes whether to invoke an AWS Lambda function to perform additional processing of the data that's retrieved\.

Amazon Personalize is an AWS service that's designed to help you create ML models that provide real\-time, personalized recommendations for customers who use your applications\. Amazon Personalize guides you through the process of creating and training an ML model, and then preparing and deploying the model as an Amazon Personalize campaign\. You can then retrieve real\-time, personalized recommendations from the campaign\. To learn more about Amazon Personalize, see the [Amazon Personalize Developer Guide](https://docs.aws.amazon.com/personalize/latest/dg/what-is-personalize.html)\. 

AWS Lambda is a compute service that you can use to run code without provisioning or managing servers\. You package your code and upload it to AWS Lambda as a *Lambda function*\. AWS Lambda then runs the function when the function is invoked\. A function can be invoked manually by you, automatically in response to events, or in response to requests from applications or services, including Amazon Pinpoint\. For information about creating and invoking Lambda functions, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.

After you create an Amazon Pinpoint configuration for a recommender model, you can add recommendations from the model to messages that you send from campaigns and journeys\. You do this by using message templates that contain message variables for recommended attributes\. A *recommended attribute* is a dynamic endpoint or user attribute that's designed to store recommendation data\. You define these attributes when you create the configuration for a recommender model\.

You can use variables for recommended attributes in the following types of message templates:
+ Email templates, for email messages that you send from campaigns or journeys\.
+ Push notification templates, for push notifications that you send from campaigns\.
+ SMS templates, for SMS text messages that you send from campaigns\.

For more information about using recommender models with Amazon Pinpoint, see [Machine Learning Models](https://docs.aws.amazon.com/pinpoint/latest/userguide/ml-models.html) in the *Amazon Pinpoint User Guide*\.

If you configure Amazon Pinpoint to invoke a Lambda function that processes recommendation data, Amazon Pinpoint performs the following general tasks each time it sends personalized recommendations in a message for a campaign or journey:

1. Evaluates and processes the configuration settings and contents of the message and message template\.

1. Determines that the message template is connected to a recommender model\.

1. Evaluates the configuration settings for connecting to and using the model\. These are defined by the [Recommender Model](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders-recommender-id.html) resource for the model\.

1. Detects one or more message variables for recommended attributes that are defined by the configuration settings for the model\.

1. Retrieves recommendation data from the Amazon Personalize campaign that's specified in the configuration settings for the model\. It uses the [GetRecommendations](https://docs.aws.amazon.com/personalize/latest/dg/API_RS_GetRecommendations.html) operation of the Amazon Personalize Runtime API to perform this task\.

1. Adds the appropriate recommendation data to a dynamic recommended attribute \(`RecommendationItems`\) for each message recipient\.

1. Invokes your Lambda function and sends the recommendation data for each recipient to that function for processing\.

   The data is sent as a JSON object that contains the endpoint definition for each recipient\. Each endpoint definition includes a `RecommendationItems` field that contains an ordered array of 1–5 values\. The number of values in the array depends on the configuration settings for the model\.

1. Waits for your Lambda function to process the data and return the results\.

   The results are a JSON object that contains an updated endpoint definition for each recipient\. Each updated endpoint definition contains a new `Recommendations` object\. This object contains 1–10 fields, one for each custom recommended attribute that you defined in the configuration settings for the model\. Each of these fields stores enhanced recommendation data for the endpoint\.

1. Uses the updated endpoint definition for each recipient to replace each message variable with the appropriate value for that recipient\.

1. Sends a version of the message that contains the personalized recommendations for each message recipient\.

To customize and enhance recommendations in this way, start by creating a Lambda function that processes the endpoint definitions sent by Amazon Pinpoint, and returns updated endpoint definitions\. Next, assign a Lambda function policy to the function and authorize Amazon Pinpoint to invoke the function\. Then, configure the recommender model in Amazon Pinpoint\. When you configure the model, specify the function to invoke and define the recommended attributes to use\.

## Creating the Lambda function<a name="ml-models-rm-lambda-create-function"></a>

To learn how to create a Lambda function, see [Getting Started](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\. When you design and develop your function, keep the following requirements and guidelines in mind\. 

### Input event data<a name="ml-models-rm-lambda-create-function-input"></a>

When Amazon Pinpoint invokes a Lambda function for a recommender model, it sends a payload that contains the configuration and other settings for the campaign or journey that's sending the message\. The payload includes an `Endpoints` object, which is a map that associates endpoint IDs with endpoint definitions for message recipients\.

The endpoint definitions use the structure defined by the [Endpoint](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-endpoints-endpoint-id.html) resource of the Amazon Pinpoint API\. However, they also include a field for a dynamic recommended attribute named `RecommendationItems`\. The `RecommendationItems` field contains one or more recommended items for the endpoint, as returned from the Amazon Personalize campaign\. The value for this field is an ordered array of 1–5 recommended items \(as strings\)\. The number of items in the array depends on the number of recommended items that you configured Amazon Pinpoint to retrieve for each endpoint or user\. 

For example:

```
"Endpoints": {
    "endpointIDexample-1":{
        "ChannelType":"EMAIL",
        "Address":"sofiam@example.com",
        "EndpointStatus":"ACTIVE",
        "OptOut":"NONE",
        "EffectiveDate":"2020-02-26T18:56:24.875Z",
        "Attributes":{
            "AddressType":[
                "primary"
            ]
        },
        "User":{
            "UserId":"SofiaMartínez",
            "UserAttributes":{
                "LastName":[
                    "Martínez"
                ],
                "FirstName":[
                    "Sofia"
                ],
                "Neighborhood":[
                    "East Bay"
                ]
            }
        },
        "RecommendationItems":[
            "1815",
            "2009",
            "1527"
        ],
        "CreationDate":"2020-02-26T18:56:24.875Z"
    },
    "endpointIDexample-2":{
        "ChannelType":"EMAIL",
        "Address":"alejandror@example.com",
        "EndpointStatus":"ACTIVE",
        "OptOut":"NONE",
        "EffectiveDate":"2020-02-26T18:56:24.897Z",
        "Attributes":{
            "AddressType":[
                "primary"
            ]
        },
        "User":{
            "UserId":"AlejandroRosalez",
            "UserAttributes":{
                "LastName ":[
                    "Rosalez"
                ],
                "FirstName":[
                    "Alejandro"
                ],
                "Neighborhood":[
                    "West Bay"
                ]
            }
        },
        "RecommendationItems":[
            "1210",
            "6542",
            "4582"
        ],
        "CreationDate":"2020-02-26T18:56:24.897Z"
    }
}
```

In the preceding example, the relevant Amazon Pinpoint settings are:
+ The recommender model is configured to retrieve three recommended items for each endpoint or user\. \(The value for the `RecommendationsPerMessage` property is set to `3`\.\) With this setting, Amazon Pinpoint retrieves and adds only the first, second, and third recommended items for each endpoint or user\.
+ The project is configured to use custom user attributes that store each user's first name, last name, and the neighborhood where they live\. \(The `UserAttributes` object contains the values for these attributes\.\)
+ The project is configured to use a custom endpoint attribute \(`AddressType`\) that indicates whether the endpoint is the user's preferred address \(channel\) for receiving messages from the project\. \(The `Attributes` object contains the value for this attribute\.\)

When Amazon Pinpoint invokes the Lambda function and sends this payload as the event data, AWS Lambda passes the data to the Lambda function for processing\.

Each payload can contain data for up to 50 endpoints\. If a segment contains more than 50 endpoints, Amazon Pinpoint invokes the function repeatedly, for up to 50 endpoints at a time, until the function processes all the data\. 

### Response data and requirements<a name="ml-models-rm-lambda-create-function-response"></a>

As you design and develop your Lambda function, keep the [quotas for machine learning models](quotas.md#quotas-ML-models) in mind\. If the function doesn't meet the conditions defined by these quotas, Amazon Pinpoint won't be able to process and send the message\.

Also keep the following requirements in mind:
+ The function must return updated endpoint definitions in the same format that was provided by the input event data\.
+ Each updated endpoint definition can contain 1–10 custom recommended attributes for the endpoint or user\. The names of these attributes must match the attribute names that you specify when you configure the recommender model in Amazon Pinpoint\.
+ All custom recommended attributes have to be returned in a single `Recommendations` object for each endpoint or user\. This requirement helps ensure that naming conflicts don't occur\. You can add the `Recommendations` object to any location in an endpoint definition\. 
+ The value for each custom recommended attribute has to be a string \(single value\) or an array of strings \(multiple values\)\. If the value is an array of strings, we recommend that you maintain the order of the recommended items that Amazon Personalize returned, as indicated in the `RecommendationItems` field\. Otherwise, your content might not reflect the model's predictions for an endpoint or user\.
+ The function shouldn't modify other elements in the event data, including other attribute values for an endpoint or user\. It should only add and return values for custom recommended attributes\. Amazon Pinpoint won't accept updates to any other values in the function's response\.
+ The function has to be hosted in the same AWS Region as the Amazon Pinpoint project that's invoking the function\. If the function and the project aren't in the same Region, Amazon Pinpoint can't send event data to the function\.

If any of the preceding requirements isn't met, Amazon Pinpoint won't be able to process and send the message to one or more endpoints\. This might cause a campaign or journey activity to fail\. 

Finally, we recommend that you reserve 256 concurrent executions for the function\.

Overall, your Lambda function should process the event data that's sent by Amazon Pinpoint and return modified endpoint definitions\. It can do this by iterating through each endpoint in the `Endpoints` object and, for each endpoint, creating and setting values for the custom recommended attributes that you want to use\. The following example handler, written in Python and continuing with the preceding example of input event data, shows this:

```
import json
from random import randrange
import random
import string
 
def lambda_handler(event, context):
    print("Received event: " + json.dumps(event))
    print("Received context: " +  str(context))
    segment_endpoints = event["Endpoints"]
    new_segment = dict()
    for endpoint_id in segment_endpoints.keys():
        endpoint = segment_endpoints[endpoint_id]
        if supported_endpoint(endpoint):
            new_segment[endpoint_id] = add_recommendation(endpoint)
 
    print("Returning endpoints: " + json.dumps(new_segment))
    return new_segment
 
def supported_endpoint(endpoint):
    return True
 
def add_recommendation(endpoint):
    endpoint["Recommendations"] = dict()
 
    customTitleList = list()
    customGenreList = list()
    for i,item in enumerate(endpoint["RecommendationItems"]):
        item = int(item)
        if item = 1210:
            customTitleList.insert(i, "Hanna")
            customGenreList.insert(i, "Action")
        elif item = 1527:
            customTitleList.insert(i, "Catastrophe")
            customGenreList.insert(i, "Comedy")
        elif item = 1815:
            customTitleList.insert(i, "Fleabag")
            customGenreList.insert(i, "Comedy")
        elif item = 2009:
            customTitleList.insert(i, "Late Night")
            customGenreList.insert(i, "Drama")
        elif item = 4582:
            customTitleList.insert(i, "Agatha Christie\'s The ABC Murders")
            customGenreList.insert(i, "Crime")
        elif item = 6542:
            customTitleList.insert(i, "Hunters")
            customGenreList.insert(i, "Drama")
        
    endpoint["Recommendations"]["Title"] = customTitleList
    endpoint["Recommendations"]["Genre"] = customGenreList
    
    return endpoint
```

In the preceding example, AWS Lambda passes the event data to the handler as the `event` parameter\. The handler iterates through each endpoint in the `Endpoints` object and sets values for custom recommended attributes named `Recommendations.Title` and `Recommendations.Genre`\. The `return` statement returns each updated endpoint definition to Amazon Pinpoint\.

Continuing with the earlier example of input event data, the updated endpoint definitions are:

```
"Endpoints":{
    "endpointIDexample-1":{
        "ChannelType":"EMAIL",
        "Address":"sofiam@example.com",
        "EndpointStatus":"ACTIVE",
        "OptOut":"NONE",
        "EffectiveDate":"2020-02-26T18:56:24.875Z",
        "Attributes":{
            "AddressType":[
                "primary"
            ]
        },
        "User":{
            "UserId":"SofiaMartínez",
            "UserAttributes":{
                "LastName":[
                    "Martínez"
                ],
                "FirstName":[
                    "Sofia"
                ],
                "Neighborhood":[
                    "East Bay"
                ]
            }
        },
        "RecommendationItems":[
            "1815",
            "2009",
            "1527"
        ],
        "CreationDate":"2020-02-26T18:56:24.875Z",
        "Recommendations":{
            "Title":[
                "Fleabag",
                "Late Night",
                "Catastrophe"
            ],
            "Genre":[
                "Comedy",
                "Comedy",
                "Comedy"
            ]
        }
    },
    "endpointIDexample-2":{
        "ChannelType":"EMAIL",
        "Address":"alejandror@example.com",
        "EndpointStatus":"ACTIVE",
        "OptOut":"NONE",
        "EffectiveDate":"2020-02-26T18:56:24.897Z",
        "Attributes":{
            "AddressType":[
                "primary"
            ]
        },
        "User":{
            "UserId":"AlejandroRosalez",
            "UserAttributes":{
                "LastName ":[
                    "Rosalez"
                ],
                "FirstName":[
                    "Alejandro"
                ],
                "Neighborhood":[
                    "West Bay"
                ]
            }
        },
        "RecommendationItems":[
            "1210",
            "6542",
            "4582"
        ],
        "CreationDate":"2020-02-26T18:56:24.897Z",
        "Recommendations":{
            "Title":[
                "Hanna",
                "Hunters",
                "Agatha Christie\'s The ABC Murders"
            ],
            "Genre":[
                "Action",
                "Drama",
                "Crime"
            ]
        }
    }
}
```

In the preceding example, the function modified the `Endpoints` object that it received and returned the results\. The `Endpoint` object for each endpoint now contains a new `Recommendations` object, which contains `Title` and `Genre` fields\. Each of these fields stores an ordered array of three values \(as strings\), where each value provides enhanced content for a corresponding recommended item in the `RecommendationItems` field\.

## Assigning a Lambda function policy<a name="ml-models-rm-lambda-trust-policy"></a>

Before you can use your Lambda function to process recommendation data, you must authorize Amazon Pinpoint to invoke the function\. To grant invocation permission, assign a Lambda function policy to the function\. A *Lambda function policy* is a resource\-based permissions policy that designates which entities can use a function and what actions those entities can take\. For more information, see [Using Resource\-Based Policies for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html) in the *AWS Lambda Developer Guide*\.

The following example policy allows the Amazon Pinpoint service principal to use the `lambda:InvokeFunction` action for a particular Amazon Pinpoint campaign \(*campaignId*\) in a particular Amazon Pinpoint project \(*projectId*\):

```
{
  "Sid": "sid",
  "Effect": "Allow",
  "Principal": {
    "Service": "pinpoint.us-east-1.amazonaws.com"
  },
  "Action": "lambda:InvokeFunction",
  "Resource": "{arn:aws:lambda:us-east-1:accountId:function:function-name}",
  "Condition": {
    "ArnLike": {
      "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:accountId:recommenders/*"
    }
  }
}
```

The function policy requires a `Condition` block that includes an `AWS:SourceArn` key\. This key specifies which resource is allowed to invoke the function\. In the preceding example, the policy allows one particular campaign to invoke the function\.

You can also write a policy that allows the Amazon Pinpoint service principal to use the `lambda:InvokeFunction` action for all the campaigns and journeys in a specific Amazon Pinpoint project \(*projectId*\)\. The following example policy shows this:

```
{
  "Sid": "sid",
  "Effect": "Allow",
  "Principal": {
    "Service": "pinpoint.us-east-1.amazonaws.com"
  },
  "Action": "lambda:InvokeFunction",
  "Resource": "{arn:aws:lambda:us-east-1:accountId:function:function-name}",
  "Condition": {
    "ArnLike": {
      "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:accountId:recommenders/*"
    }
  }
}
```

Unlike the first example, the `AWS:SourceArn` key in the `Condition` block of this example allows one particular project to invoke the function\. This permission applies to all the campaigns and journeys in the project\.

To write a more generic policy, you can use a multicharacter match wildcard \(\*\)\. For example, you can use the following `Condition` block to allow any Amazon Pinpoint project to invoke the function:

```
"Condition": {
  "ArnLike": {
    "AWS:SourceArn": "arn:aws:mobiletargeting:us-east-1:accountId:recommenders/*"
  }
}
```

If you want to use the Lambda function with all the projects for your Amazon Pinpoint account, we recommend that you configure the `Condition` block of the policy in the preceding way\. However, as a best practice, you should create policies that include only the permissions that are required to perform a specific action on a specific resource\.

## Authorizing Amazon Pinpoint to invoke the function<a name="ml-models-rm-lambda-trust-policy-assign"></a>

After you assign a Lambda function policy to the function, you can add permissions that allow Amazon Pinpoint to invoke the function for a specific project, campaign, or journey\. You can do this using the AWS Command Line Interface \(AWS CLI\) and the Lambda [https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) command\. The following example shows how to do this for a specific project \(*projectId*\):

```
$ aws lambda add-permission \
--function-name function-name \
--statement-id sid \
--action lambda:InvokeFunction \
--principal pinpoint.us-east-1.amazonaws.com \
--source-arn arn:aws:mobiletargeting:us-east-1:accountId:recommenders/*
```

The preceding example is formatted for Unix, Linux, and macOS\. For Microsoft Windows, replace the backslash \(\\\) line\-continuation character with a caret \(^\)\.

If the command runs successfully, you see output similar to the following:

```
{
  "Statement": "{\"Sid\":\"sid\",
    \"Effect\":\"Allow\",
    \"Principal\":{\"Service\":\"pinpoint.us-east-1.amazonaws.com\"},
    \"Action\":\"lambda:InvokeFunction\",
    \"Resource\":\"arn:aws:lambda:us-east-1:111122223333:function:function-name\",
    \"Condition\":
      {\"ArnLike\":
        {\"AWS:SourceArn\":
         \"arn:aws:mobiletargeting:us-east-1:111122223333:recommenders/*\"}}}"
}
```

The `Statement` value is a JSON string version of the statement that was added to the Lambda function policy\.

## Configuring the Recommender Model<a name="ml-models-rm-lambda-configure"></a>

To configure Amazon Pinpoint to invoke the Lambda function for a recommender model, specify the following Lambda\-specific configuration settings for the model:
+ `RecommendationTransformerUri` – This property specifies the name or Amazon Resource Name \(ARN\) of the Lambda function\.
+ `Attributes` – This object is a map that defines the custom recommended attributes that the function adds to each endpoint definition\. Each of these attributes can be used as a message variable in a message template\.

You can specify these settings by using the [Recommender Models](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders.html) resource of the Amazon Pinpoint API \(when you create the configuration for a model\) or the [Recommender Model](https://docs.aws.amazon.com/pinpoint/latest/apireference/recommenders-recommender-id.html) resource of the Amazon Pinpoint API \(if you update the configuration for a model\)\. You can also define these settings by using the Amazon Pinpoint console\.

For more information about using recommender models with Amazon Pinpoint, see [Machine Learning Models](https://docs.aws.amazon.com/pinpoint/latest/userguide/ml-models.html) in the *Amazon Pinpoint User Guide*\.
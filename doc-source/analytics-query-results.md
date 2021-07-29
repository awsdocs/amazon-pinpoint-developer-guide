# Using Amazon Pinpoint analytics query results<a name="analytics-query-results"></a>

When you use Amazon Pinpoint Analytics APIs to query analytics data, Amazon Pinpoint returns the results in a JSON response\. For application metrics, campaign metrics, and journey engagement metrics, the data in the response adheres to a standard JSON schema for reporting Amazon Pinpoint analytics data\. 

This means that you can use the programming language or tool of your choice to implement a custom solution that queries the data for one or more of these metrics, captures the results of each query, and then writes the results to a table, object, or other location\. You can then work with the query results in that location by using another service or application\.

For example, you can:
+ Build a custom dashboard that queries a set of metrics on a regular basis and displays the results by using your preferred data visualization framework\.
+ Create a report that tracks engagement rates by querying the appropriate metrics and displaying the results in a chart or other type of report that you design\.
+ Parse and write analytics data to a particular storage format, and then port the results to a long\-term storage solution\.

Note that Amazon Pinpoint Analytics APIs aren't designed to create or store any persistent objects that you can subsequently read or use in an Amazon Pinpoint project or your Amazon Pinpoint account\. Instead, the APIs are designed to help you retrieve analytics data and transfer that data to other services and applications for further analysis, storage, or reporting\. They do this partly by using the same JSON response structure and schema for all the analytics data that you can query programmatically for application metrics, campaign metrics, and journey engagement metrics\.

This topic explains the structure, objects, and fields in a JSON response to a query for an application metric, campaign metric, or journey engagement metric\. For information about the fields in a JSON response to a query for a journey execution metric or journey activity execution metric, see [Standard Amazon Pinpoint analytics metrics](analytics-standard-metrics.md)\. 

## JSON structure<a name="analytics-query-results-structure"></a>

To help you parse and use query results, Amazon Pinpoint Analytics APIs use the same JSON response structure for all Amazon Pinpoint analytics data that you can query programmatically for application metrics, campaign metrics, and journey engagement metrics\. Each JSON response specifies the values that defined the query, such as the project ID \(`ApplicationId`\)\. The response also includes one \(and only one\) `KpiResult` object\. The `KpiResult` object contains the overall result set for a query\.

Each `KpiResult` object contains a `Rows` object\. This is an array of objects that contain query results and relevant metadata about the values in those results\. The structure and content of a `Rows` object has the following general characteristics:
+ Each row of query results is a separate JSON object, named `Values`, in the `Rows` object\. For example, if a query returns three values, the `Rows` object contains three `Values` objects\. Each `Values` object contains an individual result for the query\.
+ Each column of query results is a property of the `Values` object that it applies to\. The name of the column is stored in the `Key` field of the `Values` object\.
+ For grouped query results, each `Values` object has an associated `GroupedBys` object\. The `GroupedBys` object indicates which field was used to group the results\. It also provides the grouping value for the associated `Values` object\.
+ If the query results for a metric is null, the `Rows` object is empty\.

Beyond these general characteristics, the structure and contents of the `Rows` object varies depending on the metric\. This is because Amazon Pinpoint supports two kinds of metrics, *single\-value metrics* and *multiple\-value metrics*\. 

A *single\-value metric* provides only one cumulative value\. An example is the percentage of messages that were delivered to recipients by all runs of a campaign\. A *multiple\-value metric* provides more than one value and groups those values by a relevant field\. An example is the percentage of messages that were delivered to recipients for each run of a campaign, grouped by campaign run\. 

You can quickly determine whether a metric is a single\-value metric or multiple\-value metric by referring to the name of the metric\. If the name doesn't contain `grouped-by`, it's a single\-value metric\. If it does, it's a multiple\-value metric\. For a complete list of metrics that you can query programmatically, see [Standard Amazon Pinpoint analytics metrics](analytics-standard-metrics.md)\. 

### Single\-value metrics<a name="analytics-query-results-structure-single"></a>

For a single\-value metric, the `Rows` object contains a `Values` object that:
+ Specifies the friendly name of the metric that was queried\.
+ Provides the value for the metric that was queried\.
+ Identifies the data type of the value that was returned\.

For example, the following JSON response contains the query results for a single\-value metric\. This metric reports the number of unique endpoints that messages were delivered to by all the campaigns that are associated with a project, from August 1, 2019 through August 31, 2019:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "EndTime":"2019-08-31T23:59:59Z",
        "KpiName":"unique-deliveries",
        "KpiResult":{
            "Rows":[
                {
                    "Values":[
                        {
                            "Key":"UniqueDeliveries",
                            "Type":"Double",
                            "Value":"1368.0"
                        }
                    ]
                }
            ]
        },
        "StartTime":"2019-08-01T00:00:00Z"
    }
}
```

In this example, the response indicates that all the project's campaigns delivered messages to 1,368 unique endpoints from August 1, 2019 through August 31, 2019, where:
+ `Key` is the friendly name of the metric whose value is specified in the `Value` field \(`UniqueDeliveries`\)\.
+ `Type` is the data type of the value specified in the `Value` field \(`Double`\)\.
+ `Value` is the actual value for the metric that was queried, including any filters that were applied \(`1368.0`\)\.

If the query results for a single\-value metric is null \(not greater than or equal to zero\), the `Rows` object is empty\. Amazon Pinpoint returns a null value for a metric if there isn't any data to return for the metric\. For example:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"2345678901234567890123456example",
        "EndTime":"2019-08-31T23:59:59Z",
        "KpiName":"unique-deliveries",
        "KpiResult":{
            "Rows":[

            ]
        },
        "StartTime":"2019-08-01T00:00:00Z"
    }
}
```

### Multiple\-value metrics<a name="analytics-query-results-structure-multiple"></a>

The structure and contents of the `Rows` object for a multiple\-value metric are mostly the same as a single\-value metric\. The `Rows` object for a multiple\-value metric also contains a `Values` object\. The `Values` object specifies the friendly name of the metric that was queried, provides the value for that metric, and identifies the data type of that value\.

However, the `Rows` object for a multiple\-value metric also contains one or more `GroupedBy` objects\. There is one `GroupedBy` object for each `Values` object in the query results\. The `GroupedBy` object indicates which field was used to group the data in the results and the data type of that field\. It also indicates the grouping value for that field \(for the associated `Values` object\)\. 

For example, the following JSON response contains the query results for a multiple\-value metric that reports the number of unique endpoints that messages were delivered to, for each campaign that's associated with a project, from August 1, 2019 through August 31, 2019:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "EndTime":"2019-08-31T23:59:59Z",
        "KpiName":"unique-deliveries-grouped-by-campaign",
        "KpiResult":{
            "Rows":[
                {
                    "GroupedBys":[
                        {
                            "Key":"CampaignId",
                            "Type":"String",
                            "Value":"80b8efd84042ff8d9c96ce2f8example"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"UniqueDeliveries",
                            "Type":"Double",
                            "Value":"123.0"
                        }
                    ]
                },
                {
                    "GroupedBys":[
                        {
                            "Key":"CampaignId",
                            "Type":"String",
                            "Value":"810c7aab86d42fb2b56c8c966example"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"UniqueDeliveries",
                            "Type":"Double",
                            "Value":"456.0"
                        }
                    ]
                },
                {
                    "GroupedBys":[
                        {
                            "Key":"CampaignId",
                            "Type":"String",
                            "Value":"42d8c7eb0990a57ba1d5476a3example"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"UniqueDeliveries",
                            "Type":"Double",
                            "Value":"789.0"
                        }
                    ]
                }
            ]
        },
        "StartTime":"2019-08-01T00:00:00Z"
    }
}
```

In this example, the response indicates that three of the project's campaigns delivered messages to unique endpoints from August 1, 2019 through August 31, 2019\. For each of those campaigns, the breakdown of delivery counts is:
+ Campaign `80b8efd84042ff8d9c96ce2f8example` delivered messages to 123 unique endpoints\.
+ Campaign `810c7aab86d42fb2b56c8c966example` delivered messages to 456 unique endpoints\.
+ Campaign `42d8c7eb0990a57ba1d5476a3example` delivered messages to 789 unique endpoints\.

Where the general structure of the objects and fields is:
+ `GroupedBys.Key` – The name of the property or field that stores the grouping value specified in the `GroupedBys.Value` field \(`CampaignId`\)\.
+ `GroupedBys.Type` – The data type of the value specified in the `GroupedBys.Value` field \(`String`\)\.
+ `GroupedBys.Value` – The actual value for the field that was used to group the data, as specified in the `GroupedBys.Key` field \(campaign ID\)\.
+ `Values.Key` – The friendly name of the metric whose value is specified in the `Values.Value` field \(`UniqueDeliveries`\)\.
+ `Values.Type` – The data type of the value specified in the `Values.Value` field \(`Double`\)\.
+ `Values.Value` – The actual value for the metric that was queried, including any filters that were applied\.

If the query results for a multiple\-value metric is null \(not greater than or equal to zero\) for a specific project, campaign, or other resource, Amazon Pinpoint doesn't return any objects or fields for the resource\. If the query results for a multiple\-value metric is null for all resources, Amazon Pinpoint returns an empty `Rows` object\.

## JSON objects and fields<a name="analytics-query-results-schema"></a>

In addition to specifying the values that defined a query, such as the project ID \(`ApplicationId`\), each JSON response to a query for an application metric, campaign metric, or journey engagement metric includes a `KpiResult` object\. This object contains the overall result set for a query, which you can parse to send analytics data to another service or application\. Each `KpiResult` object contains some or all of the following standard objects and fields, depending on the metric\.


| Object or field | Description | 
| --- | --- | 
| Rows | An array of objects that contains the result set for a query\. | 
| Rows\.GroupedBys | For a multiple\-value metric, an array of fields that defines the field and values that were used to group data in query results\.  | 
| Rows\.GroupedBys\.Key | For a multiple\-value metric, the name of the property or field that stores the value specified in the GroupedBys\.Value field\. | 
| Rows\.GroupedBys\.Type | For a multiple\-value metric, the data type of the value specified in the GroupedBys\.Value field\. | 
| Rows\.GroupedBys\.Value | For a multiple\-value metric, the actual value for the field that was used to group data in query results\. This value correlates to an associated Values object\. | 
| Rows\.Values | An array of fields that contains query results\. | 
| Rows\.Values\.Key | The friendly name of the metric that was queried\. The metric's value is specified in the Values\.Value field\. | 
| Rows\.Values\.Type | The data type of the value specified in the Values\.Value field\. | 
| Rows\.Values\.Value | The actual value for the metric that was queried, including any filters that were applied\. | 

 For information about the fields in a JSON response to a query for a journey execution metric or journey activity execution metric, see [Standard Amazon Pinpoint analytics metrics](analytics-standard-metrics.md)\.
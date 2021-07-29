# Querying Amazon Pinpoint analytics data for campaigns<a name="analytics-query-campaigns"></a>

In addition to using the analytics pages on the Amazon Pinpoint console, you can use Amazon Pinpoint Analytics APIs to query analytics data for a subset of standard metrics that provide insight into delivery and engagement trends for campaigns\.

Each of these metrics is a measurable value, also referred to as a *key performance indicator \(KPI\)*, that can help you monitor and assess the performance of one or more campaigns\. For example, you can use a metric to find out how many endpoints a campaign message was sent to, or how many of those messages were delivered to the intended endpoints\.

Amazon Pinpoint automatically collects and aggregates this data for all of your campaigns\. It stores the data for 90 days\. If you integrated a mobile app with Amazon Pinpoint by using an AWS Mobile SDK, Amazon Pinpoint extends this support to include additional metrics, such as the percentage of push notifications that were opened by recipients\. For information about integrating a mobile app, see [Integrating Amazon Pinpoint with your application](integrate.md)\.

If you use Amazon Pinpoint Analytics APIs to query data, you can choose various options that define the scope, data, grouping, and filters for your query\. You do this by using parameters that specify the project, campaign, and metric that you want to query, in addition to any date\-based filters that you want to apply\. 

This topic explains and provides examples of how to choose these options and query the data for one or more campaigns\.

## Prerequisites<a name="analytics-query-campaigns-prerequisites"></a>

Before you query analytics data for one or more campaigns, it helps to gather the following information, which you use to define your query:
+ **Project ID** – The unique identifier for the project that’s associated with the campaign or campaigns\. In the Amazon Pinpoint API, this value is stored in the `application-id` property\. On the Amazon Pinpoint console, this value is displayed as the **Project ID** on the **All projects** page\.
+ **Campaign ID** – The unique identifier for the campaign, if you want to query the data for only one campaign\. In the Amazon Pinpoint API, this value is stored in the `campaign-id` property\. This value is not displayed on the console\.
+ **Date range** – Optionally, the first and last date and time of the date range to query data for\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, they must start fewer than 90 days from the current day\. If you don’t specify a date range, Amazon Pinpoint automatically queries the data for the preceding 31 calendar days\.
+ **Metric type** – The type of metric to query\. There are two types, *application metrics* and *campaign metrics*\. An *application metric* provides data for all the campaigns that are associated with a project, also referred to as an *application*\. A *campaign metric* provides data for only one campaign\.
+ **Metric** – The name of the metric to query—more specifically, the `kpi-name` value for the metric\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

It also helps to determine whether you want to group the data by a relevant field\. If you do, you can simplify your analysis and reporting by choosing a metric that’s designed to group data for you automatically\. For example, Amazon Pinpoint provides several standard metrics that report the percentage of messages that were delivered to recipients of a campaign\. One of these metrics automatically groups the data by date \(`successful-delivery-rate-grouped-by-date`\)\. Another metric automatically groups the data by campaign run \(`successful-delivery-rate-grouped-by-campaign-activity`\)\. A third metric simply returns a single value—the percentage of messages that were delivered to recipients by all campaign runs \(`successful-delivery-rate`\)\. 

If you can't find a standard metric that groups data the way that you want, you can develop a series of queries that return the data that you want\. You can then manually break down or combine the query results into custom groups that you design\.

Finally, it’s important to verify that you’re authorized to access the data that you want to query\. For more information, see [IAM policies for querying Amazon Pinpoint analytics data](analytics-permissions.md)\.

## Querying data for one campaign<a name="analytics-query-campaigns-single"></a>

To query the data for one campaign, you use the [Campaign Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html) API and specify values for the following required parameters:
+ **application\-id** – The project ID, which is the unique identifier for the project that’s associated with the campaign\. In Amazon Pinpoint, the terms *project* and *application* have the same meaning\. 
+ **campaign\-id** – The unique identifier for the campaign\.
+ **kpi\-name** – The name of the metric to query\. This value describes the associated metric and consists of two or more terms, which are comprised of lowercase alphanumeric characters, separated by a hyphen\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

You can also apply a filter that queries the data for a specific date range\. If you don’t specify a date range, Amazon Pinpoint returns the data for the preceding 31 calendar days\. To filter the data by different dates, use the supported date range parameters to specify the first and last date and time of the date range\. The values should be in extended ISO 8601 format and use Coordinated Universal Time \(UTC\)—for example, `2019-07-19T20:00:00Z` for 8:00 PM UTC July 19, 2019\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, the first date and time must be fewer than 90 days from the current day\.

The following examples show how to query analytics data for a campaign by using the Amazon Pinpoint REST API, the AWS CLI, and the AWS SDK for Java\. You can use any supported AWS SDK to query analytics data for a campaign\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\.

------
#### [ REST API ]

To query analytics data for a campaign by using the Amazon Pinpoint REST API, send an HTTP\(S\) GET request to the [Campaign Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html) URI\. In the URI, specify the appropriate values for the required path parameters:

```
https://endpoint/v1/apps/application-id/campaigns/campaign-id/kpis/daterange/kpi-name
```

Where:
+ *endpoint* is the Amazon Pinpoint endpoint for the AWS Region that hosts the project associated with the campaign\.
+ *application\-id* is the unique identifier for the project that’s associated with the campaign\.
+ *campaign\-id* is the unique identifier for the campaign\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

All the parameters should be URL encoded\.

To apply a filter that queries the data for a specific date range, append the `start-time` and `end-time` query parameters and values to the URI\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. Use an ampersand \(&\) to separate the parameters\.

For example, the following request retrieves the number of unique endpoints that messages were delivered to, by all runs of a campaign, from July 19, 2019 through July 26, 2019:

```
https://pinpoint.us-east-1.amazonaws.com/v1/apps/1234567890123456789012345example/campaigns/80b8efd84042ff8d9c96ce2f8example/kpis/daterange/unique-deliveries?start-time=2019-07-19T00:00:00Z&end-time=2019-07-26T23:59:59Z
```

Where:
+ `pinpoint.us-east-1.amazonaws.com` is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaign\.
+ `80b8efd84042ff8d9c96ce2f8example` is the unique identifier for the campaign\.
+ `unique-deliveries` is the `kpi-name` value for the *endpoint deliveries* campaign metric, which is the metric that reports the number of unique endpoints that messages were delivered to, by all runs of a campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ AWS CLI ]

To query analytics data for a campaign by using the AWS CLI, use the get\-campaign\-date\-range\-kpi command and specify the appropriate values for the required parameters:

```
C:\> aws pinpoint get-campaign-date-range-kpi ^
    --application-id application-id ^
    --campaign-id campaign-id ^
    --kpi-name kpi-name
```

Where:
+ *application\-id* is the unique identifier for the project that’s associated with the campaign\.
+ *campaign\-id* is the unique identifier for the campaign\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

To apply a filter that queries the data for a specific date range, add the `start-time` and `end-time` parameters and values to your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of unique endpoints that messages were delivered to, by all runs of a campaign, from July 19, 2019 through July 26, 2019:

```
C:\> aws pinpoint get-campaign-date-range-kpi ^
    --application-id 1234567890123456789012345example ^
    --campaign-id 80b8efd84042ff8d9c96ce2f8example ^
    --kpi-name unique-deliveries ^
    --start-time 2019-07-19T00:00:00Z ^
    --end-time 2019-07-26T23:59:59Z
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaign\.
+ `80b8efd84042ff8d9c96ce2f8example` is the unique identifier for the campaign\.
+ `unique-deliveries` is the `kpi-name` value for the *endpoint deliveries* campaign metric, which is the metric that reports the number of unique endpoints that messages were delivered to, by all runs of a campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ SDK for Java ]

To query analytics data for a campaign by using the AWS SDK for Java, use the GetCampaignDateRangeKpiRequest method of the [Campaign Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-campaigns-campaign-id-kpis-daterange-kpi-name.html) API\. Specify the appropriate values for the required parameters:

```
GetCampaignDateRangeKpiRequest request = new GetCampaignDateRangeKpiRequest()
        .withApplicationId("applicationId")
        .withCampaignId("campaignId")
        .withKpiName("kpiName")
```

Where:
+ *applicationId* is the unique identifier for the project that’s associated with the campaign\.
+ *campaignId* is the unique identifier for the campaign\.
+ *kpiName* is the `kpi-name` value for the metric to query\.

To apply a filter that queries the data for a specific date range, include the `startTime` and `endTime` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of unique endpoints that messages were delivered to, by all runs of a campaign, from July 19, 2019 through July 26, 2019:

```
GetCampaignDateRangeKpiRequest request = new GetCampaignDateRangeKpiRequest()
        .withApplicationId("1234567890123456789012345example")
        .withCampaignId("80b8efd84042ff8d9c96ce2f8example")
        .withKpiName("unique-deliveries")
        .withStartTime(Date.from(Instant.parse("2019-07-19T00:00:00Z")))
        .withEndTime(Date.from(Instant.parse("2019-07-26T23:59:59Z")));
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaign\.
+ `80b8efd84042ff8d9c96ce2f8example` is the unique identifier for the campaign\.
+ `unique-deliveries` is the `kpi-name` value for the *endpoint deliveries* campaign metric, which is the metric that reports the number of unique endpoints that messages were delivered to, by all runs of a campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------

After you send your query, Amazon Pinpoint returns the query results in a JSON response\. The structure of the results varies depending on the metric that you queried\. Some metrics return only one value\. For example, the *endpoint deliveries* \(`unique-deliveries`\) campaign metric, which is used in the preceding examples, returns one value—the number of unique endpoints that messages were delivered to, by all runs of a campaign\. In this case, the JSON response is the following:

```
{
    "CampaignDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "CampaignId":"80b8efd84042ff8d9c96ce2f8example",
        "EndTime":"2019-07-26T23:59:59Z",
        "KpiName":"unique-deliveries",
        "KpiResult":{
            "Rows":[
                {
                    "Values":[
                        {
                            "Key":"UniqueDeliveries",
                            "Type":"Double",
                            "Value":"123.0"
                        }
                    ]
                }
            ]
        },
        "StartTime":"2019-07-19T00:00:00Z"
    }
}
```

Other metrics return multiple values, and group the values by a relevant field\. If a metric returns multiple values, the JSON response includes a field that indicates which field was used to group the data\.

To learn more about the structure of query results, see [Using query results](analytics-query-results.md)\.

## Querying data for multiple campaigns<a name="analytics-query-campaigns-multiple"></a>

There are two ways to query the data for multiple campaigns\. The best way depends on whether you want to query the data for campaigns that are all associated with the same project\. If you do, it also depends on whether you want to query the data for all or only or subset of those campaigns\.

To query the data for campaigns that are associated with different projects or for only a subset of the campaigns that are associated with the same project, the best approach is to create and run a series of individual queries, one for each campaign that you want to query the data for\. The preceding section explains how to query the data for only one campaign\.

To query the data for all the campaigns that are associated with the same project, you can use the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API\. Specify values for the following required parameters:
+ **application\-id** – The project ID, which is the unique identifier for the project\. In Amazon Pinpoint, the terms *project* and *application* have the same meaning\.
+ **kpi\-name** – The name of the metric to query\. This value describes the associated metric and consists of two or more terms, which are comprised of lowercase alphanumeric characters, separated by a hyphen\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

You can also filter the data by date range\. If you don’t specify a date range, Amazon Pinpoint returns the data for the preceding 31 calendar days\. To filter the data by different dates, use the supported date range parameters to specify the first and last date and time of the date range\. The values should be in extended ISO 8601 format and use Coordinated Universal Time \(UTC\)—for example, `2019-07-19T20:00:00Z` for 8:00 PM UTC July 19, 2019\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, the first date and time must be fewer than 90 days from the current day\.

The following examples show how to query analytics data for a campaign by using the Amazon Pinpoint REST API, the AWS CLI, and the AWS SDK for Java\. You can use any supported AWS SDK to query analytics data for a campaign\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\.

------
#### [ REST API ]

To query analytics data for multiple campaigns by using the Amazon Pinpoint REST API, send an HTTP\(S\) GET request to the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) URI\. In the URI, specify the appropriate values for the required path parameters:

```
https://endpoint/v1/apps/application-id/kpis/daterange/kpi-name
```

Where:
+ *endpoint* is the Amazon Pinpoint endpoint for the AWS Region that hosts the project associated with the campaigns\.
+ *application\-id* is the unique identifier for the project that’s associated with the campaigns\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

All the parameters should be URL encoded\.

To apply a filter that retrieves the data for a specific date range, append the `start-time` and `end-time` query parameters and values to the URI\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. Use an ampersand \(&\) to separate the parameters\.

For example, the following request retrieves the number of unique endpoints that messages were delivered to, by each of a project's campaigns, from July 19, 2019 through July 26, 2019:

```
https://pinpoint.us-east-1.amazonaws.com/v1/apps/1234567890123456789012345example/kpis/daterange/unique-deliveries-grouped-by-campaign?start-time=2019-07-19T00:00:00Z&end-time=2019-07-26T23:59:59Z
```

Where:
+ `pinpoint.us-east-1.amazonaws.com` is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaigns\.
+ `unique-deliveries-grouped-by-campaign` is the `kpi-name` value for the *endpoint deliveries, grouped by campaign* application metric, which is the metric that returns the number of unique endpoints that messages were delivered to, by each campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ AWS CLI ]

To query analytics data for multiple campaigns by using the AWS CLI, use the get\-application\-date\-range\-kpi command and specify the appropriate values for the required parameters:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id application-id ^
    --kpi-name kpi-name
```

Where:
+ *application\-id* is the unique identifier for the project that’s associated with the campaigns\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

To apply a filter that retrieves the data for a specific date range, include the `start-time` and `end-time` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of unique endpoints that messages were delivered to, by each of a project's campaigns, from July 19, 2019 through July 26, 2019:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id 1234567890123456789012345example ^
    --kpi-name unique-deliveries-grouped-by-campaign ^
    --start-time 2019-07-19T00:00:00Z ^
    --end-time 2019-07-26T23:59:59Z
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaign\.
+ `unique-deliveries-grouped-by-campaign` is the `kpi-name` value for the *endpoint deliveries, grouped by campaign* application metric, which is the metric that returns the number of unique endpoints that messages were delivered to, by each campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ SDK for Java ]

To query analytics data for multiple campaigns by using the AWS SDK for Java, use the GetApplicationDateRangeKpiRequest method of the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API\. Specify the appropriate values for the required parameters:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("applicationId")
        .withKpiName("kpiName")
```

Where:
+ *applicationId* is the unique identifier for the project that’s associated with the campaigns\.
+ *kpiName* is the `kpi-name` value for the metric to query\.

To apply a filter that retrieves the data for a specific date range, include the `startTime` and `endTime` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of unique endpoints that messages were delivered to, by each of a project's campaigns, from July 19, 2019 through July 26, 2019:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("1234567890123456789012345example")
        .withKpiName("unique-deliveries-grouped-by-campaign")
        .withStartTime(Date.from(Instant.parse("2019-07-19T00:00:00Z")))
        .withEndTime(Date.from(Instant.parse("2019-07-26T23:59:59Z")));
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project that’s associated with the campaigns\.
+ `unique-deliveries-grouped-by-campaign` is the `kpi-name` value for the *endpoint deliveries, grouped by campaign* application metric, which is the metric that returns the number of unique endpoints that messages were delivered to, by each campaign\.
+ `2019-07-19T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-07-26T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------

After you send your query, Amazon Pinpoint returns the query results in a JSON response\. The structure of the results varies depending on the metric that you queried\. Some metrics return only one value\. Other metrics return multiple values, and those values are grouped by a relevant field\. If a metric returns multiple values, the JSON response includes a field that indicates which field was used to group the data\.

For example, the *endpoint deliveries, grouped by campaign* \(`unique-deliveries-grouped-by-campaign`\) application metric, which is used in the preceding examples, returns multiple values—the number of unique endpoints that messages were delivered to, for each campaign that's associated with a project\. In this case, the JSON response is the following:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "EndTime":"2019-07-26T23:59:59Z",
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
        "StartTime":"2019-07-19T00:00:00Z"
    }
}
```

In this case, the `GroupedBys` field indicates that the values are grouped by campaign ID \(`CampaignId`\)\.

To learn more about the structure of query results, see [Using query results](analytics-query-results.md)\.
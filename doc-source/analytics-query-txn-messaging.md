# Querying Amazon Pinpoint analytics data for transactional messages<a name="analytics-query-txn-messaging"></a>

In addition to using the analytics pages on the Amazon Pinpoint console, you can use Amazon Pinpoint Analytics APIs to query analytics data for a subset of standard metrics that provide insight into delivery and engagement trends for the transactional messages that were sent for a project\. 

Each of these metrics is a measurable value, also referred to as a *key performance indicator \(KPI\)*, that can help you monitor and assess the performance of transactional messages\. For example, you can use a metric to find out how many transactional email or SMS messages you sent, or how many of those messages were delivered to recipients\. Amazon Pinpoint automatically collects and aggregates this data for all the transactional email and SMS messages that you send for a project\. It stores the data for 90 days\.

If you use Amazon Pinpoint Analytics APIs to query data, you can choose various options that define the scope, data, grouping, and filters for your query\. You do this by using parameters that specify the project and metric that you want to query, in addition to any date\-based filters that you want to apply\. 

This topic explains and provides examples of how to choose these options and query transactional messaging data for a project\.

## Prerequisites<a name="analytics-query-txn-messaging-prerequisites"></a>

Before you query analytics data for transactional messages, it helps to gather the following information, which you use to define your query:
+ **Project ID** – The unique identifier for the project that the messages were sent from\. In the Amazon Pinpoint API, this value is stored in the `application-id` property\. On the Amazon Pinpoint console, this value is displayed as the **Project ID** on the **All projects** page\.
+ **Date range** – Optionally, the first and last date and time of the date range to query data for\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, they must start fewer than 90 days from the current day\. If you don’t specify a date range, Amazon Pinpoint automatically queries the data for the preceding 31 calendar days\.
+ **Metric** – The name of the metric to query—more specifically, the `kpi-name` value for the metric\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

It also helps to determine whether you want to group the data by a relevant field\. If you do, you can simplify your analysis and reporting by choosing a metric that’s designed to group data for you automatically\. For example, Amazon Pinpoint provides several standard metrics that report the number of transactional SMS messages that were delivered to recipients\. One of these metrics automatically groups the data by date \(`txn-sms-delivered-grouped-by-date`\)\. Another metric automatically groups the data by country or region \(`txn-sms-delivered-grouped-by-country`\)\. A third metric simply returns a single value—the number of messages that were delivered to recipients \(`txn-sms-delivered`\)\. If you can't find a standard metric that groups data the way that you want, you can develop a series of queries that return the data that you want\. You can then manually break down or combine the query results into custom groups that you design\.

Finally, it’s important to verify that you’re authorized to access the data that you want to query\. For more information, see [IAM policies for querying Amazon Pinpoint analytics data](analytics-permissions.md)\.

## Querying data for transactional email messages<a name="analytics-query-txn-messaging-email"></a>

To query the data for transactional email messages that were sent for a project, you use the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API and specify values for the following required parameters:
+ **application\-id** – The project ID, which is the unique identifier for the project\. In Amazon Pinpoint, the terms *project* and *application* have the same meaning\.
+ **kpi\-name** – The name of the metric to query\. This value describes the associated metric and consists of two or more terms, which are comprised of lowercase alphanumeric characters, separated by a hyphen\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

You can also apply a filter that queries the data for a specific date range\. If you don’t specify a date range, Amazon Pinpoint returns the data for the preceding 31 calendar days\. To filter the data by different dates, use the supported date range parameters to specify the first and last date and time of the date range\. The values should be in extended ISO 8601 format and use Coordinated Universal Time \(UTC\)—for example, `2019-09-06T20:00:00Z` for 8:00 PM UTC September 6, 2019\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, the first date and time must be fewer than 90 days from the current day\.

The following examples show how to query analytics data for transactional email messages by using the Amazon Pinpoint REST API, the AWS CLI, and the AWS SDK for Java\. You can use any supported AWS SDK to query analytics data for transactional messages\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\.

------
#### [ REST API ]

To query analytics data for transactional email messages by using the Amazon Pinpoint REST API, send an HTTP\(S\) GET request to the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) URI\. In the URI, specify the appropriate values for the required path parameters:

```
https://endpoint/v1/apps/application-id/kpis/daterange/kpi-name
```

Where:
+ *endpoint* is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ *application\-id* is the unique identifier for the project\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

All the parameters should be URL encoded\.

To apply a filter that queries the data for a specific date range, append the `start-time` and `end-time` query parameters and values to the URI\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. Use an ampersand \(&\) to separate the parameters\.

For example, the following request retrieves the number of transactional email messages that were sent for a project from September 6, 2019 through September 13, 2019:

```
https://pinpoint.us-east-1.amazonaws.com/v1/apps/1234567890123456789012345example/kpis/daterange/txn-emails-sent?start-time=2019-09-06T00:00:00Z&end-time=2019-09-13T23:59:59Z
```

Where:
+ `pinpoint.us-east-1.amazonaws.com` is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-emails-sent` is the `kpi-name` value for the *sends* application metric, which is the metric that reports the number of transactional email messages that were sent for a project\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-13T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ AWS CLI ]

To query analytics data for transactional email messages by using the AWS CLI, use the get\-application\-date\-range\-kpi command, and specify the appropriate values for the required parameters:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id application-id ^
    --kpi-name kpi-name
```

Where:
+ *application\-id* is the unique identifier for the project\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

To apply a filter that queries the data for a specific date range, add the `start-time` and `end-time` parameters and values to your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of transactional email messages that were sent for a project from September 6, 2019 through September 13, 2019:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id 1234567890123456789012345example ^
    --kpi-name txn-emails-sent ^
    --start-time 2019-09-06T00:00:00Z ^
    --end-time 2019-09-13T23:59:59Z
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-emails-sent` is the `kpi-name` value for the *sends* application metric, which is the metric that reports the number of transactional email messages that were sent for a project\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-13T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ SDK for Java ]

To query analytics data for transactional email messages by using the AWS SDK for Java, use the GetApplicationDateRangeKpiRequest method of the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API\. Specify the appropriate values for the required parameters:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("applicationId")
        .withKpiName("kpiName")
```

Where:
+ *applicationId* is the unique identifier for the project\.
+ *kpiName* is the `kpi-name` value for the metric to query\.

To apply a filter that queries the data for a specific date range, include the `startTime` and `endTime` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of transactional email messages that were sent for a project from September 6, 2019 through September 13, 2019:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("1234567890123456789012345example")
        .withKpiName("txn-emails-sent")
        .withStartTime(Date.from(Instant.parse("2019-09-06T00:00:00Z")))
        .withEndTime(Date.from(Instant.parse("2019-09-13T23:59:59Z")));
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-emails-sent` is the `kpi-name` value for the *sends* application metric, which is the metric that reports the number of transactional email messages that were sent for a project\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-13T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------

After you send your query, Amazon Pinpoint returns the query results in a JSON response\. The structure of the results varies depending on the metric that you queried\. Some metrics return only one value\. For example, the *sends* \(`txn-emails-sent`\) application metric, which is used in the preceding examples, returns one value—the number of transactional email messages that were sent from a project\. In this case, the JSON response is the following:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "EndTime":"2019-09-13T23:59:59Z",
        "KpiName":"txn-emails-sent",
        "KpiResult":{
            "Rows":[
                {
                    "Values":[
                        {
                            "Key":"TxnEmailsSent",
                            "Type":"Double",
                            "Value":"62.0"
                        }
                    ]
                }
            ]
        },
        "StartTime":"2019-09-06T00:00:00Z"
    }
}
```

Other metrics return multiple values and group the values by a relevant field\. If a metric returns multiple values, the JSON response includes a field that indicates which field was used to group the data\.

To learn more about the structure of query results, see [Using query results](analytics-query-results.md)\.

## Querying data for transactional SMS messages<a name="analytics-query-txn-messaging-sms"></a>

To query the data for transactional SMS messages that were sent for a project, you use the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API and specify values for the following required parameters:
+ **application\-id** – The project ID, which is the unique identifier for the project\. In Amazon Pinpoint, the terms *project* and *application* have the same meaning\.
+ **kpi\-name** – The name of the metric to query\. This value describes the associated metric and consists of two or more terms, which are comprised of lowercase alphanumeric characters, separated by a hyphen\. For a complete list of supported metrics and the `kpi-name` value for each one, see [Standard metrics](analytics-standard-metrics.md)\.

You can also apply a filter that queries the data for a specific date range\. If you don’t specify a date range, Amazon Pinpoint returns the data for the preceding 31 calendar days\. To filter the data by different dates, use the supported date range parameters to specify the first date and time and the last date and time of the date range\. The values should be in extended ISO 8601 format and use Coordinated Universal Time \(UTC\)—for example, `2019-09-06T20:00:00Z` for 8:00 PM UTC September 6, 2019\. Date ranges are inclusive and must be limited to 31 or fewer calendar days\. In addition, the first date and time must be fewer than 90 days from the current day\.

The following examples show how to query analytics data for transactional SMS messages by using the Amazon Pinpoint REST API, the AWS CLI, and the AWS SDK for Java\. You can use any supported AWS SDK to query analytics data for transactional messages\. The AWS CLI examples are formatted for Microsoft Windows\. For Unix, Linux, and macOS, replace the caret \(^\) line\-continuation character with a backslash \(\\\)\.

------
#### [ REST API ]

To query analytics data for transactional SMS messages by using the Amazon Pinpoint REST API, send an HTTP\(S\) GET request to the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) URI\. In the URI, specify the appropriate values for the required path parameters:

```
https://endpoint/v1/apps/application-id/kpis/daterange/kpi-name
```

Where:
+ *endpoint* is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ *application\-id* is the unique identifier for the project\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

All the parameters should be URL encoded\.

To apply a filter that retrieves the data for a specific date range, append the `start-time` and `end-time` query parameters and values to the URI\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. Use an ampersand \(&\) to separate the parameters\.

For example, the following request retrieves the number of transactional SMS messages that were sent each day from September 6, 2019 through September 8, 2019:

```
https://pinpoint.us-east-1.amazonaws.com/v1/apps/1234567890123456789012345example/kpis/daterange/txn-sms-sent-grouped-by-date?start-time=2019-09-06T00:00:00Z&end-time=2019-09-08T23:59:59Z
```

Where:
+ `pinpoint.us-east-1.amazonaws.com` is the Amazon Pinpoint endpoint for the AWS Region that hosts the project\.
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-sms-sent-grouped-by-date` is the `kpi-name` value for the *sends, grouped by date* application metric, which is the metric that returns the number of transactional SMS messages that were sent during each day of the date range\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-08T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ AWS CLI ]

To query analytics data for transactional SMS messages by using the AWS CLI, use the get\-application\-date\-range\-kpi command, and specify the appropriate values for the required parameters:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id application-id ^
    --kpi-name kpi-name
```

Where:
+ *application\-id* is the unique identifier for the project\.
+ *kpi\-name* is the `kpi-name` value for the metric to query\.

To apply a filter that retrieves the data for a specific date range, include the `start-time` and `end-time` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of transactional SMS messages that were sent each day from September 6, 2019 through September 8, 2019:

```
C:\> aws pinpoint get-application-date-range-kpi ^
    --application-id 1234567890123456789012345example ^
    --kpi-name txn-sms-sent-grouped-by-date ^
    --start-time 2019-09-06T00:00:00Z ^
    --end-time 2019-09-08T23:59:59Z
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-sms-sent-grouped-by-date` is the `kpi-name` value for the *sends, grouped by date* application metric, which is the metric that returns the number of transactional SMS messages that were sent during each day of the date range\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-08T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------
#### [ SDK for Java ]

To query analytics data for transactional SMS messages by using the AWS SDK for Java, use the GetApplicationDateRangeKpiRequest method of the [Application Metrics](https://docs.aws.amazon.com/pinpoint/latest/apireference/apps-application-id-kpis-daterange-kpi-name.html) API, and specify the appropriate values for the required parameters:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("applicationId")
        .withKpiName("kpiName")
```

Where:
+ *applicationId* is the unique identifier for the project\.
+ *kpiName* is the `kpi-name` value for the metric to query\.

To apply a filter that retrieves the data for a specific date range, include the `startTime` and `endTime` parameters and values in your query\. By using these parameters, you can specify the first and last date and time, in extended ISO 8601 format, of an inclusive date range to retrieve the data for\. For example, the following request retrieves the number of transactional SMS messages that were sent each day from September 6, 2019 through September 8, 2019:

```
GetApplicationDateRangeKpiRequest request = new GetApplicationDateRangeKpiRequest()
        .withApplicationId("1234567890123456789012345example")
        .withKpiName("txn-sms-sent-grouped-by-date")
        .withStartTime(Date.from(Instant.parse("2019-09-06T00:00:00Z")))
        .withEndTime(Date.from(Instant.parse("2019-09-08T23:59:59Z")));
```

Where:
+ `1234567890123456789012345example` is the unique identifier for the project\.
+ `txn-sms-sent-grouped-by-date` is the `kpi-name` value for the *sends, grouped by date* application metric, which is the metric that returns the number of transactional SMS messages that were sent during each day of the date range\.
+ `2019-09-06T00:00:00Z` is the first date and time to retrieve data for, as part of an inclusive date range\.
+ `2019-09-08T23:59:59Z` is the last date and time to retrieve data for, as part of an inclusive date range\.

------

After you send your query, Amazon Pinpoint returns the query results in a JSON response\. The structure of the results varies depending on the metric that you queried\. Some metrics return only one value\. Other metrics return multiple values and group those values by a relevant field\. If a metric returns multiple values, the JSON response includes a field that indicates which field was used to group the data\.

For example, the *sends, grouped by date* \(`txn-sms-sent-grouped-by-date`\) application metric, which is used in the preceding examples, returns multiple values—the number of transactional SMS messages that were sent during each day of the specified date range\. In this case, the JSON response is the following:

```
{
    "ApplicationDateRangeKpiResponse":{
        "ApplicationId":"1234567890123456789012345example",
        "EndTime":"2019-09-08T23:59:59Z",
        "KpiName":"txn-sms-sent-grouped-by-date",
        "KpiResult":{
            "Rows":[
                {
                    "GroupedBys":[
                        {
                            "Key":"Date",
                            "Type":"String",
                            "Value":"2019-09-06"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"TxnSmsSent",
                            "Type":"Double",
                            "Value":"29.0"
                        }
                    ]
                },
                {
                    "GroupedBys":[
                        {
                            "Key":"Date",
                            "Type":"String",
                            "Value":"2019-09-07"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"TxnSmsSent",
                            "Type":"Double",
                            "Value":"35.0"
                        }
                    ]
                },
                {
                    "GroupedBys":[
                        {
                            "Key":"Date",
                            "Type":"String",
                            "Value":"2019-09-08"
                        }
                    ],
                    "Values":[
                        {
                            "Key":"TxnSmsSent",
                            "Type":"Double",
                            "Value":"10.0"
                        }
                    ]
                }
            ]
        },
        "StartTime":"2019-09-06T00:00:00Z"
    }
}
```

In this case, the `GroupedBys` field indicates that the values are grouped by calendar day \(`Date`\)\. This means that: 
+ 29 messages were sent on September 6, 2019\.
+ 35 messages were sent on September 7, 2019\.
+ 10 messages were sent on September 8, 2019\.

To learn more about the structure of query results, see [Using query results](analytics-query-results.md)\.
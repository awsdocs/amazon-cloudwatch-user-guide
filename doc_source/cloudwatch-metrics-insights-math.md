# Use Metrics Insights queries with metric math<a name="cloudwatch-metrics-insights-math"></a>

You can use a Metrics Insights query as an input to a metric math function\. For more information about metric math, see [Use metric math](using-metric-math.md)\.

A Metrics Insights query that does not include a **GROUP BY** clause returns a single time series\. Therefore, its returned results can be used with any metric math function that takes a single time series as input\.

A Metrics Insights query that includes a **GROUP BY** clause returns multiple time series\. Therefore, its returned results can be used with any metric math function that takes an array of time series as input\.

For example, the following query returns the total number of bytes downloaded for each bucket in the Region, as an array of time series:

```
SELECT SUM(BytesDownloaded) 
FROM SCHEMA("AWS/S3", BucketName, FilterId) 
WHERE FilterId = 'EntireBucket'
GROUP BY BucketName
```

On a graph in the console or in a [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html) operation, the results of this query are `q1`\. This query returns the result in bytes, so if you want to see the result as MB instead, you can use the following math function:

```
q1/1024/1024
```
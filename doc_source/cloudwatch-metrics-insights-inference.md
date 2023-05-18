# SQL inference<a name="cloudwatch-metrics-insights-inference"></a>

CloudWatch Metrics Insights uses several mechanisms to infer the intention of a given SQL query\.

**Topics**
+ [Time bucketing](#cloudwatch-metrics-insights-inference-timebucketing)
+ [Fields projection](#cloudwatch-metrics-insights-inference-fieldsprojection)
+ [ORDER BY global aggregation](#cloudwatch-metrics-insights-inference-OrderBy)

## Time bucketing<a name="cloudwatch-metrics-insights-inference-timebucketing"></a>

Time series data points resulting from a query are rolled up into time buckets based on the requested period\. To aggregate values in standard SQL, an explicit GROUP BY clause must be defined to collect all the observations of a given period together\. Because this is the standard way to query time series data, CloudWatch Metrics Insights infers time bucketing without the need to express an explicit **GROUP BY** clause\. 

For example, when a query is performed with a period of one minute, all the observations belonging to that minute until the next \(excluded\) are rolled up to the start time of the time bucket\. This makes Metrics Insights SQL statements more concise and less verbose\. 

```
SELECT AVG(CPUUtilization)
FROM SCHEMA("AWS/EC2", InstanceId)
```

The previous query returns a single time series \(timestamp\-value pairs\), representing the average CPU utilization of all Amazon EC2 instances\. Assuming the requested period is one minute, each data point returned represents the average of all observations measured within a specific one\-minute interval \(start time inclusive, end time exclusive\)\. The timestamp related to the specific data point is the start time of the bucket

## Fields projection<a name="cloudwatch-metrics-insights-inference-fieldsprojection"></a>

Metrics Insights queries always return the timestamp projection\. You don’t need to specify a timestamp column in the **SELECT** clause to get the timestamp of each corresponding data point value\. For details about how timestamp is calculated, see [Time bucketing](#cloudwatch-metrics-insights-inference-timebucketing)\.

When using **GROUP BY**, each group name is also inferred and projected in the result, so that you can group the returned time series\. 

```
SELECT AVG(CPUUtilization)
FROM SCHEMA("AWS/EC2", InstanceId)
GROUP BY InstanceId
```

The previous query returns a time series for each Amazon EC2 instance\. Each time series is labelled after the value of the instance ID\.

## ORDER BY global aggregation<a name="cloudwatch-metrics-insights-inference-OrderBy"></a>

When using **ORDER BY**, **FUNCTION\(\)** infers which aggregate function that you want to order by \(the data point values of the queried metrics\)\. The aggregate operation is performed across all the matched data points of each individual time series across the queried time window\. 

```
SELECT AVG(CPUUtilization)
FROM SCHEMA("AWS/EC2", InstanceId)
GROUP BY InstanceId
ORDER BY MAX()
LIMIT 10
```

The previous query returns the CPU utilization for each Amazon EC2 instance, limiting the result set to 10 entries\. The results are ordered based on the maximum value of the individual time series within the requested time window\. The **ORDER BY** clause is applied before **LIMIT**, so that the ordering is calculated against more than 10 time series\.
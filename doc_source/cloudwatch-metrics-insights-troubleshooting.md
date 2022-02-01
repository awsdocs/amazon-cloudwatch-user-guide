# Troubleshooting Metrics Insights<a name="cloudwatch-metrics-insights-troubleshooting"></a>

****  
CloudWatch Metrics Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\.

## The results include "Other," but I don't have this as a dimension<a name="cloudwatch-metrics-insights-troubleshooting-other"></a>

This means that the query includes a **GROUP BY** clause that specifies a label key that is not used in some of the metrics that are returned by the query\. In this case, a null group named `Other` is returned\. The metrics that do not include that label key are probably aggregated metrics that return values aggregated across all values of that label key\.

 For example, suppose we have the following query:

```
SELECT AVG(Faults) 
FROM MyCustomNamespace 
GROUP BY Operation, ServiceName
```

If some of the returned metrics don't include `ServiceName` as a dimension, then those metrics are displayed as having `Other` as the value for `ServiceName`\.

To prevent seeing "Other" in your results, use **SCHEMA** in your **FROM** clause, as in the following example:

```
SELECT AVG(Faults) 
FROM SCHEMA(MyCustomNamespace, Operation)
GROUP BY Operation, ServiceName
```

This limits the returned results to only the metrics that have both the `Operation` and `ServiceName` dimensions\.

## The oldest timestamp in my graph has a lower metric value than the others<a name="cloudwatch-metrics-insights-troubleshooting-oldest"></a>

CloudWatch Metrics Insights currently supports the latest three hours of data only\. When you graph with a period larger than one minute, for example five minutes or one hour, there could be cases where the oldest data point differs from the expected value\. This is because the Metrics Insights queries return only the most recent 3 hours of data, so the oldest datapoint, being older than 3 hours, accounts only for observations that have been measured within the last three hours boundary\.
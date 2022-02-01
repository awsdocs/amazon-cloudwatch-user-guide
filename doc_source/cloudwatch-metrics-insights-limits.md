# Metrics Insights limits<a name="cloudwatch-metrics-insights-limits"></a>

****  
CloudWatch Metrics Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\.

CloudWatch Metrics Insights currently has the following limits:
+ Currently, you can query only the most recent three hours of data\.
+ A single query can process no more than 10,000 metrics\. This means that if the **SELECT**, **FROM**, and **WHERE** clauses match more than 10,000 metrics, the query only processes the first 10,000 of these metrics that it finds\.
+ A single query can return no more than 500 time series\. This means that if the query would return more than 500 metrics, not all metrics will be returned in the query results\. If you use an **ORDER BY** clause, then all the metrics being processed are sorted, and the 500 that have the highest or lowest values according to your **ORDER BY** clause are returned\.

  If you do not include an **ORDER BY** clause, you can't control which 500 matching metrics are returned\.
+ Each [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html) operation can have only one query, but you can have multiple widgets in a dashboard that each include a query\.
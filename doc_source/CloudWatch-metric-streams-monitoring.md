# Monitor your metric streams with CloudWatch metrics<a name="CloudWatch-metric-streams-monitoring"></a>

Metric streams emit CloudWatch metrics about their health and operation in the `AWS/CloudWatch/MetricStreams` namespace\. The following metrics are emitted\. These metrics are emitted with a `MetricStreamName` dimension and with no dimension\. You can use the metrics with no dimensions to see aggregated metrics for all of your metric streams\. You can use the metrics with the `MetricStreamName` dimension to see the metrics about only that metric stream\.

For all of these metrics, values are emitted only for metric streams that are in the **Running** state\.


| Metric | Description | 
| --- | --- | 
|  `MetricUpdate`  |  The number of metric updates sent to the metric stream\. If no metric updates are streamed during a time period, a value of 0 is emitted for this metric\. If you stop the metric stream, this metric stops being emitted until the metric stream is started again\. Valid Statistic: `Sum` Units: None | 
|  `TotalMetricUpdate`  |  This is calculated as **MetricUpdate \+ a number based on additional statistics that are being streamed**\. For each unique namespace and metric name combination, streaming 1\-5 additional statistics adds 1 to the `TotalMetricUpdate`, streaming 6\-10 additional statistics adds 2 to `TotalMetricUpdate`, and so on\.  Valid Statistic: `Sum` Units: None | 
|  `PublishErrorRate`  |  The number of unrecoverable errors that occur when putting data into the Kinesis Data Firehose delivery stream\. If no errors occur during a time period, a value of 0 is emitted for this metric\. If you stop the metric stream, this metric stops being emitted until the metric stream is started again\. Valid Statistic: `Average` to see the rate of metric updates unable to be written\. This value will be between 0\.0 and 1\.0\. Units: None  | 
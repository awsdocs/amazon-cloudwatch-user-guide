# Ingesting high\-cardinality logs and generating metrics with CloudWatch embedded metric format<a name="CloudWatch_Embedded_Metric_Format"></a>

The CloudWatch embedded metric format enables you to ingest complex high\-cardinality application data in the form of logs and to generate actionable metrics from them\. You can embed custom metrics alongside detailed log event data, and CloudWatch automatically extracts the custom metrics so that you can visualize and alarm on them, for real\-time incident detection\. Additionally, the detailed log events associated with the extracted metrics can be queried using CloudWatch Logs Insights to provide deep insights into the root causes of operational events\. 

Embedded metric format helps you to generate actionable custom metrics from ephemeral resources such as Lambda functions and containers\. By using the embedded metric format to send logs from these ephemeral resources, you can now easily create custom metrics without having to instrument or maintain separate code, while gaining powerful analytical capabilities on your log data\.

When using the embedded metric format, you can generate your logs using a client library— for more information, see [Using the client libraries to generate embedded metric format logs](CloudWatch_Embedded_Metric_Format_Libraries.md)\. Alternatively, you can manually construct the logs and submit them using the PutLogEvents API or the CloudWatch agent\.

Charges are incurred for logs ingestion and archival, and custom metrics that are generated\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Note**  
Be careful when configuring your metric extraction as it impacts your custom metric usage and corresponding bill\. If you unintentionally create metrics based on high\-cardinality dimensions \(such as `requestId`\), the embedded metric format will by design create a custom metric corresponding to each unique dimension combination\. For more information, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension)\.

**Topics**
+ [Generating logs using the embedded metric format](CloudWatch_Embedded_Metric_Format_Generation.md)
+ [Viewing your metrics and logs in the console](CloudWatch_Embedded_Metric_Format_View.md)
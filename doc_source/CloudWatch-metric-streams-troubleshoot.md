# Troubleshooting<a name="CloudWatch-metric-streams-troubleshoot"></a>

If you're not seeing metric data at your final destination, check the following:
+ Check that the metric stream is in the running state\. For steps on how to use the CloudWatch console to do this, see [Metric stream operation and maintenance](CloudWatch-metric-streams-operation.md)\.
+ Check the metrics emitted by the metric stream\. In the CloudWatch console, under **Metrics**, look at the **AWS/CloudWatch/MetricStreams ** namespace for the **MetricUpdate** and **PublishErrorRate** metrics\.
+ If the **PublishErrorRate** metric is high, confirm that the destination that is used by the Kinesis Data Firehose delivery stream exists and that the IAM role specified in the metric stream's configuration grants the `CloudWatch` service principal permissions to write to it\. For more information, see [Trust between CloudWatch and Kinesis Data Firehose](CloudWatch-metric-streams-trustpolicy.md)\.
+ Check that the Kinesis Data Firehose delivery stream has permission to write to the final destination\.
+ In the Kinesis Data Firehose console, view the Kinesis Data Firehose delivery stream that is used for the metric stream and check the **Monitoring** tab to see whether the Kinesis Data Firehose delivery stream is receiving data\.
+ Confirm that you have configured your Kinesis Data Firehose delivery stream with the correct details\.
+ Check any available logs or metrics for the final destination that the Kinesis Data Firehose delivery stream writes to\.
+ To get more detailed information, enable CloudWatch Logs error logging on the Kinesis Data Firehose delivery stream\. For more information, see [ Monitoring Kinesis Data Firehose Using CloudWatch Logs](https://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudwatch-logs.html)\.
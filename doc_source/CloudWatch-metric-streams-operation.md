# Metric stream operation and maintenance<a name="CloudWatch-metric-streams-operation"></a>

Metric streams are always in one of two states, **Running** or **Stopped**\.
+ **Running**— The metric stream is running correctly\. There might not be any metric data streamed to the destination because of the filters on the stream\.
+ **Stopped**— The metric stream has been explicitly stopped by someone, and not because of an error\. It might be useful to stop your stream to temporarily pause the streaming of data without deleting the stream\.

If you stop and restart a metric stream, the metric data that was published to CloudWatch while the metric stream was stopped is not backfilled to the metric stream\.

If you change the output format of a metric stream, in certain cases you might see a small amount of metric data written to the destination in both the old format and the new format\. To avoid this situation, you can create a new Kinesis Data Firehose delivery stream with the same configuration as your current one, then change to the new Kinesis Data Firehose delivery stream and change the output format at the same time\. This way, the Kinesis records with different output format are stored on Amazon S3 in separate objects\. Later, you can direct the traffic back to the original Kinesis Data Firehose delivery stream and delete the second delivery stream\. 

**To view, edit, stop, and start your metric streams**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Streams**\.

   The list of streams appears, and the **Status** column displays whether each stream is running or stopped\.

1. To stop or start a metric stream, select the stream and choose **Stop** or **Start**\.

1. To see the details about a metric stream, select the stream and choose **View details**\.

1. To change the stream's output format, filters, destination Kinesis Data Firehose stream, or roles, choose **Edit** and make the changes that you want\.

   If you change the filters, there might be some gaps in the metric data during the transition\.
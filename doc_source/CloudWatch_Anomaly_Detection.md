# Using CloudWatch Anomaly Detection<a name="CloudWatch_Anomaly_Detection"></a>

When you enable *anomaly detection* for a metric, CloudWatch applies statistical and machine learning algorithms\. These algorithms continuously analyze metrics of systems and applications, determine normal baselines, and surface anomalies with minimal user intervention\.

The algorithms generate an anomaly detection model\. The model generates a range of expected values that represent normal metric behavior\.

You can use the model of expected values in two ways:
+ Create anomaly detection alarms based on a metric's expected value\. These types of alarms don't have a static threshold for determining alarm state\. Instead, they compare the metric's value to the expected value based on the anomaly detection model\. 

  You can choose whether the alarm is triggered when the metric value is above the band of expected values, below the band, or both\.

  For more information, see [Creating a CloudWatch Alarm Based on Anomaly Detection](Create_Anomaly_Detection_Alarm.md)\.
+ When viewing a graph of metric data, overlay the expected values onto the graph as a band\. This makes it visually clear which values in the graph are out of the normal range\. For more information, see [Creating a Graph](graph_a_metric.md#create-metric-graph)\.

  You can enable anomaly detection using the AWS Management Console, the AWS CLI, AWS CloudFormation, or the AWS SDK\. You can enable anomaly detection on metrics vended by AWS and also on custom metrics\.

  You can also retrieve the upper and lower values of the model's band by using the `GetMetricData` API request with the `ANOMALY_DETECTION_BAND` metric math function\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.

In a graph with anomaly detection, the expected range of values is shown as a gray band\. If the metric's actual value goes beyond this band, it is shown as red during that time\.

Anomaly detection algorithms account for the seasonality and trend changes of metrics\. The seasonality changes could be hourly, daily, or weekly, as shown in the following examples\.

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph2.PNG)

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph5.png)

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph6.png)

The longer\-range trends could be downward or upward\.

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph3.PNG)

Anomaly detections also works well with metrics with flat patterns\.

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph7.png)

## How CloudWatch Anomaly Detection Works<a name="CloudWatch_Anomaly_Detection_Algorithm"></a>

When you enable anomaly detection for a metric, CloudWatch applies machine learning algorithms to the metric's past data to create a model of the metric's expected values\. The model assesses both trends and hourly, daily, and weekly patterns of the metric\. The algorithm trains on up to two weeks of metric data, but you can enable anomaly detection on a metric even if the metric does not have a full two weeks of data\.

You specify a value for the anomaly detection threshold that CloudWatch uses along with the model to determine the "normal" range of values for the metric\. A higher value for the anomaly detection threshold produces a thicker band of "normal" values\.

The machine learning model is specific to a metric and a statistic\. For example, if you enable anomaly detection for a metric using the `AVG` statistic, the model is specific to the `AVG` statistic\.

After you create a model, it constantly updates itself, using the latest data from the metric\.

After you enable anomaly detection on a metric, you can choose to exclude specified time periods of the metric from being used to train the model\. This way, you can exclude deployments or other unusual events from being used for model training, ensuring the most accurate model is created\.

Using anomaly detection models for alarms incurs charges on your AWS account\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.
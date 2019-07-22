# CloudWatch Anomaly Detection<a name="CloudWatch_Anomaly_Detection"></a>


****  

|  | 
| --- |
| CloudWatch anomaly detection is in open preview\. The preview is open to AWS accounts in all commercial AWS Regions except Asia Pacific \(Hong Kong\), AWS GovCloud \(US\), China \(Beijing\) Region, and China \(Ningxia\) Region\. You do not need to request access\. Features may be added or changed before announcing General Availability\. Please contact [anomalydetectionfeedback@amazon\.com](mailto:anomalydetectionfeedback@amazon.com) for any feedback, questions, or if you would like to be informed when updates are available\. | 

When you enable *anomaly detection* for a metric, CloudWatch applies machine\-learning algorithms to the metric's past data to create a model of the metric's expected values\. The model generates two metrics, one that represents the upper band of normal metric behavior, and another that represents the lower band\.

You can use the model of expected values in two ways:
+ You can create anomaly detection alarms based on a metric's expected value\. These types of alarms don't have a static threshold for determining alarm state, but instead compare the metric's value to the expected value based on the anomaly detection model\. You specify a number of standard deviations that CloudWatch uses along with the model to determine the "normal" range of values for the metric, where a higher number of standard deviations produces a thicker band of "normal" values\.

  You can choose whether the alarm is triggered when the metric value is above the band of expected values, below the band, or either above or below the band\.

  For more information, see [Creating a CloudWatch Alarm Based on Anomaly Detection](Create_Anomaly_Detection_Alarm.md)\.
+ When viewing a graph of metric data, you can overlay the expected values onto the graph as a band, instantly making it visually clear which values in the graph are out of the normal range\. For more information, see [Creating a Graph](graph_a_metric.md#create-metric-graph)\.

  You can enable anomaly detection using the AWS Management Console, the AWS CLI, or the AWS SDK\. You can also retrieve the upper and lower values of the model's band by using the `GetMetricData` API request with the `ANOMALY_DETECTION_BAND` metric math function\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.

The links in the previous list are for creating an anomaly detection model using the AWS Management Console\. You can also create anomaly detection models using the AWS API and the AWS CLI\.

![\[The metrics console showing anomaly detection enabled for the CPUUtilization metric.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/Anomaly_Detection_Graph.PNG)

## How CloudWatch Anomaly Detection Works<a name="CloudWatch_Anomaly_Detection_Algorithm"></a>

When you enable anomaly detection for a metric, CloudWatch applies machine\-learning algorithms to the metric's past data to create a model of the metric's expected values, taking into account trends and hourly, daily, and weekly patterns of the metric\. The machine\-learning algorithm trains on up to two weeks of metric data, but you can enable anomaly detection on a metric even if the metric does not have a full two weeks of data\.

After you enable anomaly detection, it may take 15 minutes before the anomaly detection bands are available for graphing, alarming, and retrieval\.

The machine\-learning model is specific to a metric and a statistic\. For example, if you enable anomaly detection for a metric using the `AVG` statistic, the model is specific to the `AVG` statistic\.

After you have created a model, it constantly updates itself, using the latest data from the metric\.

After you enable anomaly detection on a metric, you can optionally choose to exclude specified time periods of the metric from being used to train the model\. This way, you can exclude deployments or other unusual events from being used for model training, ensuring the most accurate model is created\.

Using anomaly detection models for alarms incurs charges on your AWS account\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.
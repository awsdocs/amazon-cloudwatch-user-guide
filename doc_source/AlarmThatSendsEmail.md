# Creating Amazon CloudWatch Alarms<a name="AlarmThatSendsEmail"></a>

You can create a CloudWatch alarm that watches a single metric\. The alarm performs one or more actions based on the value of the metric relative to a threshold over a number of time periods\. The action can be an Amazon EC2 action, an Auto Scaling action, or a notification sent to an Amazon SNS topic\.

Alarms invoke actions for sustained state changes only\. CloudWatch alarms do not invoke actions simply because they are in a particular state, the state must have changed and been maintained for a specified number of periods\. 

After an alarm invokes an action due to a change in state, its subsequent behavior depends on the type of action that you have associated with the alarm\. For Amazon EC2 and Auto Scaling actions, the alarm continues to invoke the action for every period that the alarm remains in the new state\. For Amazon SNS notifications, no additional actions are invoked\.

**Note**  
CloudWatch doesn't test or validate the actions that you specify, nor does it detect any Auto Scaling or Amazon SNS errors resulting from an attempt to invoke nonexistent actions\. Make sure that your actions exist\.

You can also add alarms to dashboards\. When an alarm is on a dashboard, it turns red when it is in the `ALARM` state, making it easier for you to monitor its status proactively\.

## Alarm States<a name="alarm-states"></a>

An alarm has the following possible states:
+ *`OK`*—The metric is within the defined threshold
+ *`ALARM`*—The metric is outside of the defined threshold
+ *`INSUFFICIENT_DATA`*—The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state

## Evaluating an Alarm<a name="alarm-evaluation"></a>

In the following figure, the alarm threshold is set to three units and the alarm is configured to trigger when all three datapoints in the most recent three consecutive periods are above the threshold\. That is, the alarm state changes to ALARM if the oldest of the three periods being evaluated is breaching, and the two subsequent periods are either breaching or missing\. In the figure, this happens in the third through fifth time periods\. At period six, the value dips below the threshold, and the alarm state changes to `OK`\. During the ninth time period, the threshold is breached again, but for only one period\. Consequently, the alarm state remains `OK`\.

![\[Alarm threshold trigger alarm\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/alarm_graph.png)

You can configure an alarm to trigger based on an "M out of N" datapoints in any alarm evaluation interval\. A datapoint is the value of a metric for a period\. If you choose one minute as the aggregation period for a metric, there is one datapoint every minute\. You must specify both the number of datapoints that must be breaching \("M"\) and the number of datapoints to consider \("N"\) when defining the alarm\. The evaluation interval is the number of datapoints multiplied by the period\. For example, if you configure 4 out of 5 datapoints with a period of 1 minute, the evaluation interval is 5 minutes\. If you configure 3 out of 3 datapoints with a period of 10 minutes, the evaluation interval is 30 minutes\. The "M" datapoints do not need to be consecutive during the evaluation interval, so you would be alerted even if the spikes in your metrics are intermittent\.

## Configuring How CloudWatch Alarms Treats Missing Data<a name="alarms-and-missing-data"></a>

Similar to how each alarm is always in one of three states, each specific data point reported to CloudWatch falls under one of three categories:
+ Not breaching \(within the threshold\)
+ Breaching \(violating the threshold\)
+ Missing

You can specify how alarms handle missing data points\. Choose whether to treat missing data points as:
+ `missing` \(The alarm looks back farther in time to find additional data points\)
+ `notBreaching` \(Treated as a data point that is within the threshold\)
+ `breaching` \(Treated as a data point that is breaching the threshold\)
+ `ignore` \(The current alarm state is maintained\)

The best choice depends on the type of metric\. For a metric that continually reports data, such as `CPUUtilization` of an instance, you might want to treat missing data points as `breaching`, because they may indicate something is wrong\. But for a metric that generates data points only when an error occurs, such as `ThrottledRequests` in Amazon DynamoDB, you would want to treat missing data as `notBreaching`\. The default behavior is `missing`\.

Choosing the best option for your alarm prevents unnecessary and misleading alarm condition changes, and also more accurately indicates the health of your system\.

**Note**  
If data points in the current evaluation window are missing or delayed, CloudWatch looks back extra periods to find existing data points to assess whether the alarm should change state\. This is true regardless of the setting for **Treat missing data as**\. For example, if you treat missing data as breaching and the furthest back period that is being considered is not breaching, the alarm state does not go to ALARM\.  
A particular case of this behavior is that CloudWatch alarms may repeatedly re\-evaluate the last set of data points for a period of time after the metric has stopped flowing\. This re\-evaluation may cause the alarm to change state and re\-execute actions, if it had changed state immediately prior to the metric stream stopping\. To mitigate this behavior, use shorter periods\.

## High\-Resolution Alarms<a name="high-resolution-alarms"></a>

 If you set an alarm on a high\-resolution metric, you can specify a high\-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds\. There is a higher charge for high\-resolution alarms\. For more information about high\-resolution metrics, see [Publish Custom Metrics](publishingMetrics.md)\.

## Percentile\-Based CloudWatch Alarms and Low Data Samples<a name="percentiles-with-low-samples"></a>

When you set a percentile as the statistic for an alarm, you can specify what to do when there is not enough data for a good statistical assessment\. You can choose to have the alarm evaluate the statistic anyway and possibly change the alarm state\. Or, you can have the alarm ignore the metric while the sample size is low, and wait to evaluate it until there is enough data to be statistically significant\.

For percentiles between 0\.5 and 1\.00, this setting is used when there are fewer than 10/\(1\-percentile\) data points during the evaluation period\. For example, this setting would be used if there were fewer than 1000 samples for an alarm on a p99 percentile\. For percentiles between 0 and 0\.5, the setting is used when there are fewer than 10/percentile data points\.

## Common Features of CloudWatch Alarms<a name="common-features-of-alarms"></a>

The following features apply to all CloudWatch alarms:
+ You can create up to 5000 alarms per region per AWS account\. To create or update an alarm, you use the `PutMetricAlarm` API action \(`mon-put-metric-alarm` command\)\.
+ Alarm names must contain only ASCII characters\.
+ You can list any or all of the currently configured alarms, and list any alarms in a particular state using `DescribeAlarms` \(`mon-describe-alarms`\)\. You can further filter the list by time range\. 
+ You can disable and enable alarms by using `DisableAlarmActions` and `EnableAlarmActions` \(`mon-disable-alarm-actions` and `mon-enable-alarm-actions`\)\.
+ You can test an alarm by setting it to any state using `SetAlarmState` \(`mon-set-alarm-state`\)\. This temporary state change lasts only until the next alarm comparison occurs\.
+ You can create an alarm using `PutMetricAlarm` \(`mon-put-metric-alarm`\) before you've created a custom metric\. For the alarm to be valid, you must include all of the dimensions for the custom metric in addition to the metric namespace and metric name in the alarm definition\.
+ You can view an alarm's history using `DescribeAlarmHistory` \(`mon-describe-alarm-history`\)\. CloudWatch preserves alarm history for two weeks\. Each state transition is marked with a unique time stamp\. In rare cases, your history might show more than one notification for a state change\. The time stamp enables you to confirm unique state changes\.
+ The number of evaluation periods for an alarm multiplied by the length of each evaluation period cannot exceed one day\.

**Note**  
Some AWS resources do not send metric data to CloudWatch under certain conditions\.  
For example, Amazon EBS may not send metric data for an available volume that is not attached to an Amazon EC2 instance, because there is no metric activity to be monitored for that volume\. If you have an alarm set for such a metric, you may notice its state change to Insufficient Data\. This may indicate that your resource is inactive, and may not necessarily mean that there is a problem\.
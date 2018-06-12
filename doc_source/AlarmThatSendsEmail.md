# Creating Amazon CloudWatch Alarms<a name="AlarmThatSendsEmail"></a>

You can create a CloudWatch alarm that watches a single metric\. The alarm performs one or more actions based on the value of the metric relative to a threshold over a number of time periods\. The action can be an Amazon EC2 action, an Auto Scaling action, or a notification sent to an Amazon SNS topic\. You can also add alarms to CloudWatch dashboards and monitor them visually\. When an alarm is on a dashboard, it turns red when it is in the `ALARM` state, making it easier for you to monitor its status proactively\.

Alarms invoke actions for sustained state changes only\. CloudWatch alarms do not invoke actions simply because they are in a particular state, the state must have changed and been maintained for a specified number of periods\. 

After an alarm invokes an action due to a change in state, its subsequent behavior depends on the type of action that you have associated with the alarm\. For Auto Scaling actions, the alarm continues to invoke the action for every period that the alarm remains in the new state\. For Amazon SNS notifications, no additional actions are invoked\.

**Note**  
CloudWatch doesn't test or validate the actions that you specify, nor does it detect any Auto Scaling or Amazon SNS errors resulting from an attempt to invoke nonexistent actions\. Make sure that your actions exist\.

## Alarm States<a name="alarm-states"></a>

An alarm has the following possible states:
+ *`OK`*—The metric is within the defined threshold\.
+ *`ALARM`*—The metric is outside of the defined threshold\.
+ *`INSUFFICIENT_DATA`*—The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\.

## Evaluating an Alarm<a name="alarm-evaluation"></a>

When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:
+ **Period** is the length of time to evaluate the metric to create each individual data point for a metric\. It is expressed in seconds\. If you choose one minute as the period, there is one datapoint every minute\.
+ **Evaluation Period** is the number of the most recent data points to evaluate when determining alarm state\.
+ **Datapoints to Alarm** is the number of data points within the evaluation period that must be breaching to cause the alarm to go to the `ALARM` state\. The breaching data points do not have to be consecutive, they just must all be within the last number of data points equal to **Evaluation Period**\.

In the following figure, the alarm threshold is set to three units\. The alarm is configured to go to the `ALARM` state and both **Evaluation Period** and **Datapoints to Alarm** are 3\. That is, when all three datapoints in the most recent three consecutive periods are above the threshold, the alarm goes to the `ALARM` state\. In the figure, this happens in the third through fifth time periods\. At period six, the value dips below the threshold, so one of the periods being evaluated is not breaching, and the alarm state changes to `OK`\. During the ninth time period, the threshold is breached again, but for only one period\. Consequently, the alarm state remains `OK`\.

![\[Alarm threshold trigger alarm\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/alarm_graph.png)

When you configure **Evaluation Period** and **Datapoints to Alarm** as different values, you are setting an "M out of N" alarm\. **Datapoints to Alarm** is \("M"\) and **Evaluation Period** is \("N"\)\. The evaluation interval is the number of datapoints multiplied by the period\. For example, if you configure 4 out of 5 datapoints with a period of 1 minute, the evaluation interval is 5 minutes\. If you configure 3 out of 3 datapoints with a period of 10 minutes, the evaluation interval is 30 minutes\.

## Configuring How CloudWatch Alarms Treat Missing Data<a name="alarms-and-missing-data"></a>

Sometimes some data points for a metric with an alarm do not get reported to CloudWatch\. For example, this can happen when a connection is lost, a server goes down, or when a metric reports data only intermittently by design\.

CloudWatch enables you to specify how to treat missing data points when evaluating an alarm\. This can help you configure your alarm to go to the `ALARM` state when appropriate for the type of data being monitored\. You can avoid false positives when missing data does not indicate a problem\.

Similar to how each alarm is always in one of three states, each specific data point reported to CloudWatch falls under one of three categories:
+ Not breaching \(within the threshold\)
+ Breaching \(violating the threshold\)
+ Missing

For each alarm, you can specify CloudWatch to treat missing data points as any of the following:
+ `missing`—The alarm does not consider missing data points when evaluating whether to change state
+ `notBreaching`—Missing data points are treated as being within the threshold
+ `breaching`—Missing data points are treated as breaching the threshold
+ `ignore`—The current alarm state is maintained

The best choice depends on the type of metric\. For a metric that continually reports data, such as `CPUUtilization` of an instance, you might want to treat missing data points as `breaching`, because they may indicate thatsomething is wrong\. But for a metric that generates data points only when an error occurs, such as `ThrottledRequests` in Amazon DynamoDB, you would want to treat missing data as `notBreaching`\. The default behavior is `missing`\.

Choosing the best option for your alarm prevents unnecessary and misleading alarm condition changes, and also more accurately indicates the health of your system\.

### How Alarm State is Evaluated When Data is Missing<a name="alarms-evaluating-missing-data"></a>

No matter what value you set for how to treat missing data, when an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than specified by **Evaluation Periods**\. The exact number of data points it attempts to retrieve depends on the length of the alarm period and whether it is based on a metric with standard resolution or high resolution\. The timeframe of the data points that it attempts to retrieve is the *evaluation range*\.

Once CloudWatch retrieves these data points, the following happens:
+ If no data points in the evaluation range are missing, CloudWatch evaluates the alarm based on the most recent data points collected\.
+ If some data points in the evaluation range are missing, but the number of existing data points retrieved is equal to or more than the alarm's **Evaluation Periods**, CloudWatch evaluates the alarm state based on the most recent existing data points that were successfully retrieved\. In this case, the value you set for how to treat missing data is not needed and is ignored\.
+ If some data points in the evaluation range are missing, and the number of existing data points that were retrieved is lower than the alarm's number of evaluation periods, CloudWatch fills in the missing data points with the result you specified for how to treat missing data, and then evaluates the alarm\. However, any real data points in the evaluation range, no matter when they were reported, are included in the evaluation\. CloudWatch uses missing data points only as few times as possible\. 

In all of these situations, the number of datapoints evaluated is equal to the value of **Evaluation Periods**\. If fewer than the value of **Datapoints to Alarm** are breaching, the alarm state is set to OK\. Otherwise, the state is set to ALARM\.

**Note**  
A particular case of this behavior is that CloudWatch alarms may repeatedly re\-evaluate the last set of data points for a period of time after the metric has stopped flowing\. This re\-evaluation may cause the alarm to change state and re\-execute actions, if it had changed state immediately prior to the metric stream stopping\. To mitigate this behavior, use shorter periods\.

The following tables illustrate examples of the alarm evaluation behavior\. In the first table, **Datapoints to Alarm** and **Evaluation Periods** are both 3\. CloudWatch retrieves the 5 most recent data points when evaluating the alarm\.

Column 2 shows how many of these 5 data points are missing and may need to be filled in using the setting for how to treat missing data\. Columns 3\-6 show the alarm state that would be set for each setting of how missing data should be treated, shown at the top of each column\. In the data points column, 0 is a non\-breaching data point, X is a breaching data point, and \- is a missing data point\.


| Data points | \# of missing data points | MISSING | IGNORE | BREACHING | NOT BREACHING | 
| --- | --- | --- | --- | --- | --- | 
|  0 \- X \- X  |  0  |  OK  |  OK  |  OK  |  OK  | 
|  0 \- \- \- \-  |  2  |  OK  |  OK  |  OK  |  OK  | 
|  \- \- \- \- \-  |  3  |  Insufficient data  |  Retain current state  |  ALARM  |  OK  | 
|  0 X X \- X  |  0  |  ALARM  |  ALARM  |  ALARM  |  ALARM  | 
|  \- \- \- \- X  |  2  |  Insufficient data  |  Retain current state  |  ALARM  |  OK  | 

In the second row of the preceding table, the alarm stays OK even if missing data is treated as breaching, because the one existing data point is not breaching, and this is evaluated along with two missing data points which are treated as breaching\. The next time this alarm is evaluated, if the data is still missing it will go to ALARM, as that non\-breaching data point will no longer be among the 5 most recent data points retrieved\. In the fourth row, the alarm goes to ALARM state in all cases because there are enough real data points so that the setting for how to treat missing data does not need to be considered\.

In the next table, the **Period** is again set to 5 minutes, and **Datapoints to Alarm** is only 2 while **Evaluation Periods** is 3\. This is a 2 out of 3, M out of N alarm\.


| Data points | \# of missing data points | MISSING | IGNORE | BREACHING | NOT BREACHING | 
| --- | --- | --- | --- | --- | --- | 
|  0 \- X \- X  |  0  |  ALARM  |  ALARM  |  ALARM  |  ALARM  | 
|  0 0 X 0 X  |  0  |  ALARM  |  ALARM  |  ALARM  |  ALARM  | 
|  0 \- X \- \-  |  1  |  OK  |  OK  |  ALARM  |  OK  | 
|  \- \- \- \- 0  |  2  |  OK  |  OK  |  ALARM  |  OK  | 
|  \- \- \- \- X  |  2  |  Insufficient data  |  Retain current state  |  ALARM  |  OK  | 

If data points are missing soon after you create an alarm, and the metric was being reported to CloudWatch before you created the alarm, CloudWatch retrieves the most recent data points from before the alarm was created when evaluating the alarm\.

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
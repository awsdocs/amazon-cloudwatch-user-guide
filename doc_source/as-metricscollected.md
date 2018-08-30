# Amazon EC2 Auto Scaling Metrics and Dimensions<a name="as-metricscollected"></a>

Amazon EC2 Auto Scaling sends metrics for instances and groups to CloudWatch\. For Auto Scaling instances, you can enable detailed \(one\-minute\) monitoring or basic \(five\-minute\) monitoring\. For Auto Scaling groups, you can enable group metrics\. For more information, see [Monitoring Your Auto Scaling Instances and Groups](http://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-monitoring.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Auto Scaling Group Metrics<a name="as-group-metrics"></a>

If you enable group metrics, Amazon EC2 Auto Scaling sends aggregated data to CloudWatch every minute\.

The `AWS/AutoScaling` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| GroupMinSize |  The minimum size of the Auto Scaling group\.  | 
| GroupMaxSize |  The maximum size of the Auto Scaling group\.  | 
| GroupDesiredCapacity |  The number of instances that the Auto Scaling group attempts to maintain\.  | 
| GroupInServiceInstances |  The number of instances that are running as part of the Auto Scaling group\. This metric does not include instances that are pending or terminating\.  | 
| GroupPendingInstances |  The number of instances that are pending\. A pending instance is not yet in service\. This metric does not include instances that are in service or terminating\.  | 
| GroupStandbyInstances |  The number of instances that are in a `Standby` state\. Instances in this state are still running but are not actively in service\.  | 
| GroupTerminatingInstances |  The number of instances that are in the process of terminating\. This metric does not include instances that are in service or pending\.   | 
| GroupTotalInstances |  The total number of instances in the Auto Scaling group\. This metric identifies the number of instances that are in service, pending, and terminating\.  | 

## Dimensions for Auto Scaling Group Metrics<a name="as-metric-dimensions"></a>

To filter the metrics for your Auto Scaling group by group name, use the `AutoScalingGroupName` dimension\.
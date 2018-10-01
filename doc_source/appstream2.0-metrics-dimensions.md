# AppStream 2\.0 Metrics and Dimensions<a name="appstream2.0-metrics-dimensions"></a>

The metrics and dimensions that AppStream 2\.0 sends to Amazon CloudWatch are listed below\. For more information, see [Monitor Amazon AppStream 2\.0 With Amazon CloudWatch](https://docs.aws.amazon.com/appstream2/latest/developerguide/monitoring.html) in the *Amazon AppStream 2\.0 Developer Guide*\.

## Amazon AppStream 2\.0 Metrics<a name="appstream-metrics"></a>

AppStream 2\.0 sends metrics to CloudWatch one time every minute\. The `AWS/AppStream` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| ActualCapacity |  The total number of instances that are available for streaming or are currently streaming\. <pre>ActualCapacity = AvailableCapacity + InUseCapacity</pre> Units: Count Valid statistics: Average, Minimum, Maximum  | 
|  AvailableCapacity  |  The number of idle instances currently available for user sessions\. <pre>AvailableCapacity = ActualCapacity - InUseCapacity</pre> Units: Count Valid statistics: Average, Minimum, Maximum  | 
| CapacityUtilization |  The percentage of instances in a fleet that are being used, using the following formula\. <pre>CapacityUtilization = (InUseCapacity/ActualCapacity) * 100</pre> Monitoring this metric helps with decisions about increasing or decreasing the value of a fleet's desired capacity\. Units: Percent Valid statistics: Average, Minimum, Maximum  | 
|  DesiredCapacity  |  The total number of instances that are either running or pending\. This represents the total number of concurrent streaming sessions your fleet can support in a steady state\. <pre>DesiredCapacity = ActualCapacity + PendingCapacity</pre> Units: Count Valid statistics: Average, Minimum, Maximum  | 
|  InUseCapacity  |  The number of instances currently being used for streaming sessions\. One `InUseCapacity` count represents one streaming session\. Units: Count Valid statistics: Average, Minimum, Maximum  | 
|  PendingCapacity  |  The number of instances being provisioned by AppStream 2\.0\. Represents the additional number of streaming sessions the fleet can support after provisioning is complete\. When provisioning starts, it usually takes 10\-20 minutes for an instance to become available for streaming\. Units: Count Valid statistics: Average, Minimum, Maximum  | 
| RunningCapacity |  The total number of instances currently running\. Represents the number of concurrent streaming sessions that can be supported by the fleet in its current state\. This metric is provided for Always\-On fleets only, and has the same value as the `ActualCapacity` metric\. Units: Count Valid statistics: Average, Minimum, Maximum  | 
|  InsufficientCapacityError  |  The number of session requests rejected due to lack of capacity\. You can set alarms to use this metric to be notified of users waiting for streaming sessions\. Units: Count Valid statistics: Average, Minimum, Maximum, Sum  | 

## Dimensions for Amazon AppStream 2\.0 Metrics<a name="appstream-dimensions"></a>

To filter the metrics provided by Amazon AppStream 2\.0, use the following dimension\.


| Dimension | Description | 
| --- | --- | 
|  Fleet  |  The name of the fleet\.  | 
# Amazon EC2 Spot Fleet Metrics and Dimensions<a name="ec2spot-metricscollected"></a>

Amazon Elastic Compute Cloud \(Amazon EC2\) sends information about your Spot fleet to CloudWatch\. For more information, see [CloudWatch Metrics for Spot Fleet](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-cloudwatch-metrics.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Amazon EC2 Spot Fleet Metrics<a name="ec2spot-metrics"></a>

The `AWS/EC2Spot` namespace includes the following metrics, plus the CloudWatch metrics for the Spot instances in your fleet\.

The `AWS/EC2Spot` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| `AvailableInstancePoolsCount` |  The Spot Instance pools specified in the Spot Fleet request\. Units: Count  | 
| `BidsSubmittedForCapacity` |  The capacity for which Amazon EC2 has submitted bids\. Units: Count  | 
| `EligibleInstancePoolCount` |  The Spot Instance pools specified in the Spot Fleet request where Amazon EC2 can fulfill bids\. Amazon EC2 will not fulfill bids in pools where your bid price is less than the Spot price or the Spot price is greater than the price for On\-Demand instances\. Units: Count  | 
| `FulfilledCapacity` |  The capacity that Amazon EC2 has fulfilled\. Units: Count  | 
| `MaxPercentCapacityAllocation` |  The maximum value of `PercentCapacityAllocation` across all Spot Instance pools specified in the Spot Fleet request\. Units: Percent  | 
| `PendingCapacity` |  The difference between `TargetCapacity` and `FulfilledCapacity`\. Units: Count  | 
| `PercentCapacityAllocation` |  The capacity allocated for the Spot Instance pool for the specified dimensions\. To get the maximum value recorded across all Spot Instance pools, use `MaxPercentCapacityAllocation`\. Units: Percent  | 
| `TargetCapacity` |  The target capacity of the Spot Fleet request\. Units: Count  | 
| `TerminatingCapacity` |  The capacity that is being terminated due to Spot Instance interruptions\. Units: Count  | 

If the unit of measure for a metric is `Count`, the most useful statistic is `Average`\.

## Dimensions for Amazon EC2 Spot Fleet Metrics<a name="ec2spot-metric-dimensions"></a>

You can filter the data using the following dimensions\.


| Dimensions | Description | 
| --- | --- | 
| `AvailabilityZone` |  Filter the data by Availability Zone\.  | 
| `FleetRequestId` |  Filter the data by Spot Fleet request\.  | 
| `InstanceType` |  Filter the data by instance type\.  | 
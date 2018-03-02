# Amazon CloudWatch Events Metrics and Dimensions<a name="cwe-metricscollected"></a>

CloudWatch Events sends metrics to Amazon CloudWatch every minute\.

## CloudWatch Events Metrics<a name="cwe-metrics"></a>

The `AWS/Events` namespace includes the following metrics\.

 All of these metrics use Count as the unit, so Sum and SampleCount are the most useful statistics\.


| Metric | Description | 
| --- | --- | 
|  `Invocations`  |  Measures the number of times a target is invoked for a rule in response to an event\. This includes successful and failed invocations, but does not include throttled or retried attempts until they fail permanently\.  CloudWatch Events only sends this metric to CloudWatch if it has a non\-zero value\.  Valid Dimensions: RuleName Units: Count  | 
|  `FailedInvocations`  |  Measures the number of invocations that failed permanently\. This does not include invocations that are retried or that succeeded after a retry attempt\. Valid Dimensions: RuleName Units: Count  | 
|  `TriggeredRules`  |  Measures the number of triggered rules that matched with any event\. Valid Dimensions: RuleName Units: Count  | 
|  `MatchedEvents`  |  Measures the number of events that matched with any rule\. Valid Dimensions: None Units: Count  | 
|  `ThrottledRules`  |  Measures the number of triggered rules that are being throttled\. Valid Dimensions: RuleName Units: Count  | 

## Dimensions for CloudWatch Events Metrics<a name="cwe-metric-dimensions"></a>

CloudWatch Events metrics have one dimension, which is listed below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  RuleName  |  Filters the available metrics by rule name\.  | 
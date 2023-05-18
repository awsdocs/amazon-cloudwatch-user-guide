# Create a CloudWatch alarm based on a Metrics Insights query<a name="Create_Metrics_Insights_Alarm"></a>

You can create an alarm on any Metrics Insights query that returns a single time series\. This can be especially useful to create dynamic alarms that watch aggregated metrics across a fleet of your infrastructure or applications\. Create the alarm once, and it adjusts as resources are added to or removed from the fleet\. For example, you can create an alarm that watches the CPU ulitization of all your instances, and the alarm dynamically adjusts as you add or remove instances\.

For complete instructions, see [Create alarms on Metrics Insights queries](cloudwatch-metrics-insights-alarms.md)\.
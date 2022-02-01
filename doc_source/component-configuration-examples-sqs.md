# Amazon Simple Queue Service \(SQS\)<a name="component-configuration-examples-sqs"></a>

The following example shows a component configuration in JSON format for Amazon Simple Queue Service\.

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "ApproximateAgeOfOldestMessage"
    }, {
      "alarmMetricName" : "NumberOfEmptyReceives"
    }
  ],
  "alarms" : [
    {
      "alarmName" : "my_sqs_alarm",
      "severity" : "MEDIUM"
    }
  ]
}
```
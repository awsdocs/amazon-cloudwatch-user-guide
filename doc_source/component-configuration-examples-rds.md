# Amazon Relational Database Service \(RDS\) instance<a name="component-configuration-examples-rds"></a>

The following example shows a component configuration in JSON format for an Amazon RDS instance\.

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "BurstBalance",
      "monitor" : true
    }, {
      "alarmMetricName" : "WriteThroughput",
      "monitor" : false
    }
  ],

  "alarms" : [
    {
      "alarmName" : "my_rds_instance_alarm",
      "severity" : "MEDIUM"
    }
  ]
}
```
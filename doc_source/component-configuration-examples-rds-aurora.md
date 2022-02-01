# Amazon Relational Database Service \(RDS\) Aurora MySQL<a name="component-configuration-examples-rds-aurora"></a>

The following example shows a component configuration in JSON format for Amazon RDS Aurora MySQL\.

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "CPUUtilization",
      "monitor": true
    },
    {
      "alarmMetricName": "CommitLatency",
      "monitor": true
    }
  ],
  "logs": [
    {
      "logType": "MYSQL",
      "monitor": true,
    },
    {
      "logType": "MYSQL_SLOW_QUERY",
      "monitor": false
    }
  ]
}
```
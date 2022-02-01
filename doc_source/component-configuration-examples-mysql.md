# RDS MariaDB and RDS MySQL<a name="component-configuration-examples-mysql"></a>

The following example shows a component configuration in JSON format for RDS MariaDB and RDS MySQL\.

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "CPUUtilization",
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
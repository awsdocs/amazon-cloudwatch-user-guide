# RDS PostgreSQL<a name="component-configuration-examples-rds-postgre-sql"></a>

The following example shows a component configurations in JSON format for RDS PostgreSQL\.

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
      "logType": "POSTGRESQL",
      "monitor": true
    }
  ]
}
```
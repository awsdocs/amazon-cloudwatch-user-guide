# Customer\-grouped Amazon EC2 instances<a name="component-configuration-examples-grouped-ec2"></a>

The following example shows a component configuration in JSON format for customer\-grouped Amazon EC2 instances\.

```
{
  "subComponents" : [
    {
      "subComponentType" : "AWS::EC2::Instance",
      "alarmMetrics" : [
        {
          "alarmMetricName" : "CPUUtilization",
        }, {
          "alarmMetricName" : "StatusCheckFailed"
        }
      ],
      "logs" : [
        {
          "logGroupName" : "my_log_group",
          "logPath" : "C:\\LogFolder\\*",
          "logType" : "APPLICATION",
        }
      ],
      "windowsEvents" : [
        {
          "logGroupName" : "my_log_group_2",
          "eventName" : "Application",
          "eventLevels" : [ "ERROR", "WARNING", "CRITICAL" ]
        }
      ]
    }, {
      "subComponentType" : "AWS::EC2::Volume",
      "alarmMetrics" : [
        {
          "alarmMetricName" : "VolumeQueueLength",
        }, {
          "alarmMetricName" : "BurstBalance"
        }
      ]
    }
  ],
  "alarms" : [
    {
      "alarmName" : "my_alarm",
      "severity" : "MEDIUM"
    }
  ]
}
```
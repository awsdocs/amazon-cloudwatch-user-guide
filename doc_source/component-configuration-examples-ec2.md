# Amazon Elastic Compute Cloud \(EC2\) instance<a name="component-configuration-examples-ec2"></a>

The following example shows a component configuration in JSON format for an Amazon EC2 instance\.

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "CPUUtilization",
      "monitor" : true
    }, {
      "alarmMetricName" : "StatusCheckFailed"
    }
  ],
  "logs" : [
    {
      "logGroupName" : "my_log_group",
      "logPath" : "C:\\LogFolder\\*",
      "logType" : "APPLICATION",
      "monitor" : true
    },
    {
      "logGroupName" : "my_log_group_2",
      "logPath" : "C:\\LogFolder2\\*",
      "logType" : "IIS",
      "encoding" : "utf-8"
    }
  ],
  "windowsEvents" : [
    {
      "logGroupName" : "my_log_group_3",
      "eventName" : "Application",
      "eventLevels" : [ "ERROR", "WARNING", "CRITICAL" ],
      "monitor" : true
    }, {
      "logGroupName" : "my_log_group_4",
      "eventName" : "System",
      "eventLevels" : [ "ERROR", "WARNING", "CRITICAL" ],
      "monitor" : true
  }],
   "alarms" : [
    {
      "alarmName" : "my_instance_alarm_1",
      "severity" : "HIGH"
    },
    {
      "alarmName" : "my_instance_alarm_2",
      "severity" : "LOW"
    }
  ],
   "subComponents" : [
   {
     "subComponentType" : "AWS::EC2::Volume",
     "alarmMetrics" : [
     {
       "alarmMetricName" : "VolumeQueueLength",
       "monitor" : "true"
     },
     {
       "alarmMetricName" : "VolumeThroughputPercentage",
       "monitor" : "true"
     },
     {
       "alarmMetricName" : "BurstBalance",
       "monitor" : "true"
     }
  }]
}
```
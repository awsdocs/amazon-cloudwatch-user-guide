# Elastic Load Balancing \(ELB\)<a name="component-configuration-examples-elb"></a>

The following example shows a component configuration in JSON format for Elastic Load Balancing\.

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "EstimatedALBActiveConnectionCount",
    }, {
      "alarmMetricName" : "HTTPCode_Backend_5XX"
    }
  ],
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
          "eventLevels" : [ "ERROR", "WARNING", "CRITICAL" ],
          "monitor" : true
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
      "alarmName" : "my_elb_alarm",
      "severity" : "HIGH"
    }
  ]
}
```
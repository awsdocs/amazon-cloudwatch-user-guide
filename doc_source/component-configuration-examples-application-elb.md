# Application Elastic Load Balancing<a name="component-configuration-examples-application-elb"></a>

The following example shows a component configuration in JSON format for Application Elastic Load Balancing\.

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "ActiveConnectionCount",
    }, {
      "alarmMetricName" : "TargetResponseTime"
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
      "alarmName" : "my_alb_alarm",
      "severity" : "LOW"
    }
  ]
}
```
# Component Configuration<a name="component-config"></a>

A component configuration is a text file in JSON format that describes the configuration settings of the component\. The following examples show a component configuration structure and its sections\.

## JSON<a name="component-config-json"></a>

The following example shows a template fragment in JSON format\.

```
{
  "alarmMetrics" : [
    list of alarm metrics
  ],
  "logs" : [
    list of logs
  ],
  "instances" : [
    component nested instances configuration
  ],
  "windowsEvents" : [
   list of windows events channels configurations
  ]
}
```

## Component Configuration Sections<a name="component-config-sections"></a>

Component configuration includes several major sections\. Sections in a component configuration can be in any order\.
+ **alarmMetrics \(optional\)**

  A list of [metrics](#component-config-metric) to monitor for the component\. All component types can have an alarmMetrics section\. 
+ **logs \(optional\)**

  A list of [logs](#component-configuration-log) to monitor for the component\. Only EC2 instances can have a logs section\. 
+ **instances \(optional\)**

  Nested instance configuration for the component\. The following types of component can have nested instances and an instances section: ELB, ASG, and custom\-grouped EC2 instances\.

The following example shows the instances section fragment in JSON format\.

```
{
  "alarmMetrics" : [
    list of alarm metrics
  ],
  "logs" : [
    list of logs
  ],
  "windowsEvents" : [
  list of windows events channels configurations
  ]
}
```

## Metric<a name="component-config-metric"></a>

Defines a metric to be monitored for the component\.

**JSON** 

```
{
  "alarmMetricName" : "monitoredMetricName",
  "monitor" : true/false
}
```

**Properties**
+ **alarmMetricName \(required\)**

  The name of the metric to be monitored for the component\. For metrics supported by Application Insights, see [Logs and Metrics Supported by Amazon CloudWatch Application Insights for \.NET and SQL Server](appinsights-logs-and-metrics.md)\. 
+ **monitor \(optional\)**

  Boolean to indicate whether to monitor the metric\. The default value is `true`\.

## Log<a name="component-configuration-log"></a>

Defines a log to be monitored for the component\.

**JSON** 

```
{
  "logGroupName" : "logGroupName",
  "logPath" : "logPath",
  "logType" : "logType",
  "encoding" : "encodingType",
  "monitor" : true/false
}
```

**Properties**
+ **logGroupName \(required\)**

  The CloudWatch log group name to be associated to the monitored log\. For the log group name constraints, see [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html)\.
+ **logPath \(required\)**

  The path of the logs to be monitored\. The log path must be an absolute Windows system file path\. For more information, see [CloudWatch Agent Configuration File: Logs Section](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html#CloudWatch-Agent-Configuration-File-Logssection)\. 
+ **logType \(required\)**

  The log type decides the log patterns against which Application Insights analyzes the log\. The log type is selected from the following: SQL\_SERVER/IIS/APPLICATION/DEFAULT\.
+ **encoding \(optional\)**

  The type of encoding of the logs to be monitored\. The specified encoding should be included in the list of [CloudWatch agent supported encodings](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\. If not provided, Application Insights uses the default encoding type for the log type:
  + For APPLICATION/DEFAULT: utf\-8 encoding
  + For SQL\_SERVER: utf\-16 encoding
  + For IIS: ascii encoding
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

## Windows Events<a name="windows-events-"></a>

Defines Windows Events to log\.

**JSON** 

```
{
  "logGroupName" : "logGroupName",
  "eventName" : "eventName",
  "eventLevels" : ["ERROR","WARNING","CRITICAL","INFORMATION","VERBOSE"],
  "monitor" : true/false
}
```

**Properties**
+ **logGroupName \(required\)**

  The CloudWatch log group name to be associated to the monitored log\. For the log group name constraints, see [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html)\.
+ **eventName \(required\)**

  The type of Windows Events to log\. It is equivalent to the Windows Event log channel name\. For example, System, Security, CustomEventName, etc\. This field is required for each type of Windows event to log\. 
+ **eventLevels \(required\)**

  The levels of event to log\. You must specify each level to log\. Possible values include `INFORMATION`, `WARNING`, `ERROR`, `CRITICAL`, and `VERBOSE`\. This field is required for each type of Windows Event to log\.
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

## Component Configuration Examples<a name="component-configuration-examples"></a>

The following examples show component configurations in JSON format for relevant AWS services\.

**Amazon Elastic Cloud Compute \(EC2\) Instance**

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
  }]
}
```

**Amazon Relational Database \(RDS\) Instance**

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
  ]
}
```

**Elastic Load Balancing \(ELB\)**

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "EstimatedALBActiveConnectionCount",
    }, {
      "alarmMetricName" : "HTTPCode_Backend_5XX"
    }
  ],
  "instances" : {
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
  }
}
```

**Application ELB**

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "ActiveConnectionCount",
    }, {
      "alarmMetricName" : "TargetResponseTime"
    }
  ],
  "instances" : {
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
  }
}
```

**Amazon EC2 Auto Scaling \(ASG\)**

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "CPUCreditBalance",
    }, {
      "alarmMetricName" : "EBSIOBalance%"
    }
  ],
  "instances" : {
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
  }
}
```

**Amazon Simple Queue Service \(SQS\)**

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "ApproximateAgeOfOldestMessage"
    }, {
      "alarmMetricName" : "NumberOfEmptyReceives"
    }
  ]
}
```

**Customer Grouped EC2 Instances**

```
{
  "instances" : {
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
  }
}
```
# Component configuration<a name="component-config"></a>

A component configuration is a text file in JSON format that describes the configuration settings of the component\. This section provides examples of a component configuration and its sections\.

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
  ],
  "alarms" : [
    list of CloudWatch alarms
  ]
}
```

## Component configuration sections<a name="component-config-sections"></a>

Component configuration includes several major sections\. Sections in a component configuration can be in any order\.
+ **alarmMetrics \(optional\)**

  A list of [metrics](#component-config-metric) to monitor for the component\. All component types can have an alarmMetrics section\. 
+ **logs \(optional\)**

  A list of [logs](#component-configuration-log) to monitor for the component\. Only EC2 instances can have a logs section\. 
+ **instances \(optional\)**

  Nested instance configuration for the component\. The following types of component can have nested instances and an instances section: ELB, ASG, and custom\-grouped EC2 instances\.
+ **alarms \(optional\)**

  A list of [alarms](#component-configuration-alarms) to monitor for the component\. All component types can have an alarm section\.

The following example shows the syntax for the **instances section fragment** in JSON format\.

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

### Metric<a name="component-config-metric"></a>

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

  The name of the metric to be monitored for the component\. For metrics supported by Application Insights, see [Logs and metrics supported by Amazon CloudWatch Application Insights for \.NET and SQL Server](appinsights-logs-and-metrics.md)\. 
+ **monitor \(optional\)**

  Boolean to indicate whether to monitor the metric\. The default value is `true`\.

### Log<a name="component-configuration-log"></a>

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
+ **logPath \(required for EC2 instance components; not required for components that do not use CloudWatch Agent, such as AWS Lambda\)**

  The path of the logs to be monitored\. The log path must be an absolute Windows system file path\. For more information, see [CloudWatch Agent Configuration File: Logs Section](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html#CloudWatch-Agent-Configuration-File-Logssection)\. 
+ **logType \(required\)**

  The log type decides the log patterns against which Application Insights analyzes the log\. The log type is selected from the following: SQL\_SERVER/IIS/APPLICATION/DEFAULT\.
+ **encoding \(optional\)**

  The type of encoding of the logs to be monitored\. The specified encoding should be included in the list of [CloudWatch agent supported encodings](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\. If not provided, CloudWatch Application Insights uses the default encoding type for the log type:
  + For APPLICATION/DEFAULT: utf\-8 encoding
  + For SQL\_SERVER: utf\-16 encoding
  + For IIS: ascii encoding
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

### Windows Events<a name="windows-events"></a>

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

### Alarm<a name="component-configuration-alarms"></a>

Defines a CloudWatch alarm to be monitored for the component\.

**JSON** 

```
{
  "alarmName" : "monitoredAlarmName",
  "severity" : HIGH/MEDIUM/LOW
}
```

**Properties**
+ **alarmName \(required\)**

  The name of the CloudWatch alarm to be monitored for the component\.
+ **severity \(optional\)**

  Indicates the degree of outage when the alarm goes off\. 

### Component configuration examples<a name="component-configuration-examples"></a>

The following examples show component configurations in JSON format for relevant services\.

**Amazon Elastic Compute Cloud \(EC2\) instance**

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
  ]
}
```

**Amazon Relational Database \(RDS\) instance**

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
  },

  "alarms" : [
    {
      "alarmName" : "my_elb_alarm",
      "severity" : "HIGH"
    }
  ]
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
  },

  "alarms" : [
    {
      "alarmName" : "my_alb_alarm",
      "severity" : "LOW"
    }
  ]
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
  },
  "alarms" : [
    {
      "alarmName" : "my_asg_alarm",
      "severity" : "LOW"
    }
  ]
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
  ],
  "alarms" : [
    {
      "alarmName" : "my_sqs_alarm",
      "severity" : "MEDIUM"
    }
  ]
}
```

**Customer Grouped EC2 instances**

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
  },
  "alarms" : [
    {
      "alarmName" : "my_alarm",
      "severity" : "MEDIUM"
    }
  ]
}
```

**AWS Lambda Function**

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "Errors",
      "monitor": true
    },
    {
      "alarmMetricName": "Throttles",
      "monitor": true
    },
    {
      "alarmMetricName": "IteratorAge",
      "monitor": true
    },
    {
      "alarmMetricName": "Duration",
      "monitor": true
    }
  ],
  "logs": [
    {
      "logType": "DEFAULT",
      "monitor": true
    }
  ]
}
```

**Amazon DynamoDB table**

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "SystemErrors",
      "monitor": false
    },
    {
      "alarmMetricName": "UserErrors",
      "monitor": false
    },
    {
      "alarmMetricName": "ConsumedReadCapacityUnits",
      "monitor": false
    },
    {
      "alarmMetricName": "ConsumedWriteCapacityUnits",
      "monitor": false
    },
    {
      "alarmMetricName": "ReadThrottleEvents",
      "monitor": false
    },
    {
      "alarmMetricName": "WriteThrottleEvents",
      "monitor": false
    },
    {
      "alarmMetricName": "ConditionalCheckFailedRequests",
      "monitor": false
    },
    {
      "alarmMetricName": "TransactionConflict",
      "monitor": false
    }
  ],
  "logs": []
}
```

**SQL Always On Availability Group**

```
{
  "instances" : {
    "alarmMetrics" : [ {
      "alarmMetricName" : "CPUUtilization",
      "monitor" : true
    }, {
      "alarmMetricName" : "StatusCheckFailed",
      "monitor" : true
    }, {
      "alarmMetricName" : "Processor % Processor Time",
      "monitor" : true
    }, {
      "alarmMetricName" : "Memory % Committed Bytes In Use",
      "monitor" : true
    }, {
      "alarmMetricName" : "Memory Available Mbytes",
      "monitor" : true
    }, {
      "alarmMetricName" : "Paging File % Usage",
      "monitor" : true
    }, {
      "alarmMetricName" : "System Processor Queue Length",
      "monitor" : true
    }, {
      "alarmMetricName" : "Network Interface Bytes Total/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "PhysicalDisk % Disk Time",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Buffer Manager Buffer cache hit ratio",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Buffer Manager Page life expectancy",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:General Statistics Processes blocked",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:General Statistics User Connections",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Locks Number of Deadlocks/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:SQL Statistics Batch Requests/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica File Bytes Received/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Log Bytes Received/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Log remaining for undo",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Log Send Queue",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Mirrored Write Transaction/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Recovery Queue",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Redo Bytes Remaining",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Redone Bytes/sec",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Total Log requiring undo",
      "monitor" : true
    }, {
      "alarmMetricName" : "SQLServer:Database Replica Transaction Delay",
      "monitor" : true
    } ],
    "windowsEvents" : [ {
      "logGroupName" : "WINDOWS_EVENTS-Application-<RESOURCE_GROUP_NAME>",
      "eventName" : "Application",
      "eventLevels" : [ "WARNING", "ERROR", "CRITICAL", "INFORMATION" ],
      "monitor" : true,
      "logType" : "SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP"
    }, {
      "logGroupName" : "WINDOWS_EVENTS-System-<RESOURCE_GROUP_NAME>",
      "eventName" : "System",
      "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
      "monitor" : true,
      "logType" : "DEFAULT"
    }, {
      "logGroupName" : "WINDOWS_EVENTS-Security-<RESOURCE_GROUP_NAME>",
      "eventName" : "Security",
      "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
      "monitor" : true,
      "logType" : "DEFAULT"
    } ],
    "logs" : [ {
      "logGroupName" : "SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP-<RESOURCE_GROUP_NAME>",
      "logPath" : "C:\\Program Files\\Microsoft SQL Server\\MSSQL**.MSSQLSERVER\\MSSQL\\Log\\ERRORLOG",
      "logType" : "SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP",
      "monitor" : true,
      "encoding" : "utf-8"
    } ]
  }
}
```

**RDS MySQL**

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

**Amazon S3 bucket**

```
{
    "alarmMetrics" : [
        {
            "alarmMetricName" : "ReplicationLatency",
            "monitor" : true
        }, {
            "alarmMetricName" : "5xxErrors",
            "monitor" : true
        }, {
            "alarmMetricName" : "BytesDownloaded"
            "monitor" : true
        }
    ]
}
```
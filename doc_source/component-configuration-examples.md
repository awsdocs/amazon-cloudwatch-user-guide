# Component configuration examples<a name="component-configuration-examples"></a>

The following examples show component configurations in JSON format for relevant services\.

**Topics**
+ [Amazon Elastic Compute Cloud \(EC2\) instance](#component-configuration-examples-ec2)
+ [Amazon Relational Database Service instance](#component-configuration-examples-rds)
+ [Amazon Relational Database Service \(RDS\) Aurora MySQL](#component-configuration-examples-rds-aurora)
+ [Elastic Load Balancing \(ELB\)](#component-configuration-examples-elb)
+ [Application Elastic Load Balancing](#component-configuration-examples-application-elb)
+ [Amazon EC2 Auto Scaling \(ASG\)](#component-configuration-examples-asg)
+ [Amazon Simple Queue Service \(SQS\)](#component-configuration-examples-sqs)
+ [Customer grouped EC2 instances](#component-configuration-examples-grouped-ec2)
+ [AWS Lambda Function](#component-configuration-examples-lambda)
+ [Amazon DynamoDB table](#component-configuration-examples-dynamo)
+ [SQL Always On Availability Group](#component-configuration-examples-sql)
+ [RDS MySQL](#component-configuration-examples-mysql)
+ [RDS PostgreSQL](#component-configuration-examples-mysql)
+ [Amazon S3 bucket](#component-configuration-examples-s3)
+ [AWS Step Functions](#component-configuration-examples-step-functions)
+ [API Gateway REST API stages](#component-configuration-examples-api-gateway)
+ [Java](#component-configuration-examples-java)
+ [RDS Oracle](#component-configuration-examples-oracle)

## Amazon Elastic Compute Cloud \(EC2\) instance<a name="component-configuration-examples-ec2"></a>

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

## Amazon Relational Database Service instance<a name="component-configuration-examples-rds"></a>

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

## Amazon Relational Database Service \(RDS\) Aurora MySQL<a name="component-configuration-examples-rds-aurora"></a>

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

## Elastic Load Balancing \(ELB\)<a name="component-configuration-examples-elb"></a>

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

## Application Elastic Load Balancing<a name="component-configuration-examples-application-elb"></a>

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

## Amazon EC2 Auto Scaling \(ASG\)<a name="component-configuration-examples-asg"></a>

```
{
  "alarmMetrics" : [
    {
      "alarmMetricName" : "CPUCreditBalance",
    }, {
      "alarmMetricName" : "EBSIOBalance%"
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
      "alarmName" : "my_asg_alarm",
      "severity" : "LOW"
    }
  ]
}
```

## Amazon Simple Queue Service \(SQS\)<a name="component-configuration-examples-sqs"></a>

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

## Customer grouped EC2 instances<a name="component-configuration-examples-grouped-ec2"></a>

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

## AWS Lambda Function<a name="component-configuration-examples-lambda"></a>

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

## Amazon DynamoDB table<a name="component-configuration-examples-dynamo"></a>

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

## SQL Always On Availability Group<a name="component-configuration-examples-sql"></a>

```
{
  "subComponents" : [ {
    "subComponentType" : "AWS::EC2::Instance",
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
      "monitor" : true
    }, {
      "logGroupName" : "WINDOWS_EVENTS-System-<RESOURCE_GROUP_NAME>",
      "eventName" : "System",
      "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
      "monitor" : true
    }, {
      "logGroupName" : "WINDOWS_EVENTS-Security-<RESOURCE_GROUP_NAME>",
      "eventName" : "Security",
      "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
      "monitor" : true
    } ],
    "logs" : [ {
      "logGroupName" : "SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP-<RESOURCE_GROUP_NAME>",
      "logPath" : "C:\\Program Files\\Microsoft SQL Server\\MSSQL**.MSSQLSERVER\\MSSQL\\Log\\ERRORLOG",
      "logType" : "SQL_SERVER",
      "monitor" : true,
      "encoding" : "utf-8"
    } ]
  }, {
    "subComponentType" : "AWS::EC2::Volume",
    "alarmMetrics" : [ {
      "alarmMetricName" : "VolumeReadBytes",
      "monitor" : true
    }, {
    "alarmMetricName" : "VolumeWriteBytes",
      "monitor" : true
    }, {
    "alarmMetricName" : "VolumeReadOps",
      "monitor" : true
    }, {
    "alarmMetricName" : "VolumeWriteOps",
      "monitor" : true
    }, {
    "alarmMetricName" : "VolumeQueueLength",
      "monitor" : true
    }, {
    "alarmMetricName" : "VolumeThroughputPercentage",
      "monitor" : true
    }, {
    "alarmMetricName" : "BurstBalance",
      "monitor" : true
    } ]
  } ]
}
```

## RDS MySQL<a name="component-configuration-examples-mysql"></a>

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

## RDS PostgreSQL<a name="component-configuration-examples-mysql"></a>

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

## Amazon S3 bucket<a name="component-configuration-examples-s3"></a>

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

## AWS Step Functions<a name="component-configuration-examples-step-functions"></a>

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "ExecutionsFailed",
      "monitor": true
    },
    {
      "alarmMetricName": "LambdaFunctionsFailed",
      "monitor": true
    },
    {
      "alarmMetricName": "ProvisionedRefillRate",
      "monitor": true
    }
  ],
  "logs": [
    {
     "logGroupName": "/aws/states/HelloWorld-Logs",
      "logType": "STEP_FUNCTION",
      "monitor": true,
    }
  ]
}
```

## API Gateway REST API stages<a name="component-configuration-examples-api-gateway"></a>

```
{ 
     "alarmMetrics" : [ 
         {
             "alarmMetricName" : "4XXError",   
             "monitor" : true
         }, 
         {
             "alarmMetricName" : "5XXError",   
             "monitor" : true
         } 
     ],
    "logs" : [
        { 
            "logType" : "API_GATEWAY_EXECUTION",   
            "monitor" : true  
        },
        { 
            "logType" : "API_GATEWAY_ACCESS",   
            "monitor" : true  
        },
    ]
}
```

## Java<a name="component-configuration-examples-java"></a>

```
{
  "alarmMetrics" : [ {
    "alarmMetricName" : "java_lang_threading_threadcount",
    "monitor" : true
  },
  {
    "alarmMetricName" : "java_lang_memory_heapmemoryusage_used",
    "monitor" : true
  },
  {
    "alarmMetricName" : "java_lang_memory_heapmemoryusage_committed",
    "monitor" : true
  }],
  "logs" : [ ],
  "JMXPrometheusExporter": {
      "hostPort":"8686",
      "prometheusPort":"9404"
  }
}
```

**Note**  
Application Insights does not support configuring authentication for Prometheus JMX exporter\. For information about how to set up authentication, see the [Prometheus JMX exporter example configuration](https://github.com/prometheus/jmx_exporter#configuration)\.

## RDS Oracle<a name="component-configuration-examples-oracle"></a>

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
      "logType": "ORACLE_ALERT",
      "monitor": true,
    },
    {
      "logType": "ORACLE_LISTENER",
      "monitor": false
    }
  ]
}
```
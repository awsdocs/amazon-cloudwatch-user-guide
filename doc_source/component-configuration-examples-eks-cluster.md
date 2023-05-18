# Amazon EKS cluster<a name="component-configuration-examples-eks-cluster"></a>

The following example shows a component configuration in JSON format for Amazon EKS cluster\.

```
{
    "alarmMetrics":[
       {
          "alarmMetricName": "cluster_failed_node_count",
          "monitor":true
       },
       {
          "alarmMetricName": "node_cpu_reserved_capacity",
          "monitor":true
       },
       {
          "alarmMetricName": "node_cpu_utilization",
          "monitor":true
       },
       {
          "alarmMetricName": "node_filesystem_utilization",
          "monitor":true
       },
       {
          "alarmMetricName": "node_memory_reserved_capacity",
          "monitor":true
       },
       {
          "alarmMetricName": "node_memory_utilization",
          "monitor":true
       },
       {
          "alarmMetricName": "node_network_total_bytes",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_cpu_reserved_capacity",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_cpu_utilization",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_cpu_utilization_over_pod_limit",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_memory_reserved_capacity",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_memory_utilization",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_memory_utilization_over_pod_limit",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_network_rx_bytes",
          "monitor":true
       },
       {
          "alarmMetricName": "pod_network_tx_bytes",
          "monitor":true
       }
    ],
    "logs":[
       {
          "logGroupName": "/aws/containerinsights/kubernetes/application",
          "logType":"APPLICATION",
          "monitor":true,
          "encoding":"utf-8"
       }
    ],
    "subComponents":[
       {
          "subComponentType":"AWS::EC2::Instance",
          "alarmMetrics":[
             {
                "alarmMetricName":"CPUUtilization",
                "monitor":true
             },
             {
                "alarmMetricName":"StatusCheckFailed",
                "monitor":true
             },
             {
                "alarmMetricName":"disk_used_percent",
                "monitor":true
             },
             {
                "alarmMetricName":"mem_used_percent",
                "monitor":true
             }
          ],
          "logs":[
             {
                "logGroupName":"APPLICATION-KubernetesClusterOnEC2-IAD",
                "logPath":"",
                "logType":"APPLICATION",
                "monitor":true,
                "encoding":"utf-8"
             }
          ],
          "processes" : [
            {
                "processName" : "my_process",
                "alarmMetrics" : [
                    {
                        "alarmMetricName" : "procstat cpu_usage",
                        "monitor" : true
                    }, {
                        "alarmMetricName" : "procstat memory_rss",
                        "monitor" : true
                    }
                ]
            }
        ],
          "windowsEvents":[
             {
                "logGroupName":"my_log_group_2",
                "eventName":"Application",
                "eventLevels":[
                   "ERROR",
                   "WARNING",
                   "CRITICAL"
                ],
                "monitor":true
             }
          ]
       },
       {
          "subComponentType":"AWS::AutoScaling::AutoScalingGroup",
          "alarmMetrics":[
             {
                "alarmMetricName":"CPUCreditBalance",
                "monitor":true
             },
             {
                "alarmMetricName":"EBSIOBalance%",
                "monitor":true
             }
          ]
       },
       {
          "subComponentType":"AWS::EC2::Volume",
          "alarmMetrics":[
             {
                "alarmMetricName":"VolumeReadBytes",
                "monitor":true
             },
             {
                "alarmMetricName":"VolumeWriteBytes",
                "monitor":true
             },
             {
                "alarmMetricName":"VolumeReadOps",
                "monitor":true
             },
             {
                "alarmMetricName":"VolumeWriteOps",
                "monitor":true
             },
             {
                "alarmMetricName":"VolumeQueueLength",
                "monitor":true
             },
             {
                "alarmMetricName":"BurstBalance",
                "monitor":true
             }
          ]
       }
    ]
 }
```

**Note**  
The `subComponents` section of `AWS::EC2::Instance`, `AWS::EC2::Volume`, and `AWS::AutoScaling::AutoScalingGroup` applies only to Amazon EKS cluster running on the EC2 launch type\.
The `windowsEvents` section of `AWS::EC2::Instance` in `subComponents` applies only to Windows running on Amazon EC2 instances\.
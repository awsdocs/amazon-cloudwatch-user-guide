# Container Insights Performance Log Events for Amazon ECS<a name="Container-Insights-reference-performance-logs-ECS"></a>

The following are examples of the performance log events that Container Insights collects from Amazon ECS\.

**Type: Container**

```
{
    "Version": "0",
    "Type": "Container",
    "ContainerName": "mysql",
    "TaskId": "7c7bce41-8f2f-4673-b1a3-0e57ef51168f",
    "TaskDefinitionFamily": "hello_world",
    "TaskDefinitionRevision": "2",
    "ContainerInstanceId": "12345678-db61-4b1b-b0ec-93ad21467cc9",
    "EC2InstanceId": "i-1234567890123456",
    "ServiceName": "myCIService",
    "ClusterName": "myCICluster",
    "Timestamp": 1561586749104,
    "CpuUtilized": 1.8381139895790504,
    "CpuReserved": 30.0,
    "MemoryUtilized": 295,
    "MemoryReserved": 400,
    "StorageReadBytes": 10817536,
    "StorageWriteBytes": 2085376000,
    "NetworkRxBytes": 6522,
    "NetworkRxDropped": 0,
    "NetworkRxErrors": 0,
    "NetworkRxPackets": 83,
    "NetworkTxBytes": 3904,
    "NetworkTxDropped": 0,
    "NetworkTxErrors": 0,
    "NetworkTxPackets": 44
}
```

**Type: Task**

```
{
    "Version": "0",
    "Type": "Task",
    "TaskId": "8b03d710-a0de-4458-b6ee-e9829e9d1440",
    "TaskDefinitionFamily": "hello_world",
    "TaskDefinitionRevision": "2",
    "ContainerInstanceId": "12345678-e797-496b-9d70-4edfda2700a4",
    "EC2InstanceId": "i-1234567890123456",
    "ServiceName": "myCIService",
    "ClusterName": "myCICluster",
    "Timestamp": 1561586269429,
    "CpuUtilized": 2.4353688568190526,
    "CpuReserved": 60.0,
    "MemoryUtilized": 314,
    "MemoryReserved": 800,
    "StorageReadBytes": 11124736,
    "StorageWriteBytes": 2100379648,
    "NetworkRxBytes": 12062,
    "NetworkRxDropped": 0,
    "NetworkRxErrors": 0,
    "NetworkRxPackets": 149,
    "NetworkTxBytes": 8064,
    "NetworkTxDropped": 0,
    "NetworkTxErrors": 0,
    "NetworkTxPackets": 96,
    "CloudWatchMetrics": [
        {
            "Namespace": "ECS/ContainerInsights",
            "Metrics": [
                {
                    "Name": "CpuUtilized",
                    "Unit": "None"
                },
                {
                    "Name": "CpuReserved",
                    "Unit": "None"
                },
                {
                    "Name": "MemoryUtilized",
                    "Unit": "Megabytes"
                },
                {
                    "Name": "MemoryReserved",
                    "Unit": "Megabytes"
                },
                {
                    "Name": "StorageReadBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "StorageWriteBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "NetworkRxBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "NetworkTxBytes",
                    "Unit": "Bytes/Second"
                }
            ],
            "Dimensions": [
                [
                    "ClusterName"
                ],
                [
                    "ServiceName",
                    "ClusterName"
                ],
                [
                    "ClusterName",
                    "TaskDefinitionFamily"
                ]
            ]
        }
    ]
}
```

**Type: Service**

```
{   
    "Version": "0",
    "Type": "Service",
    "ServiceName": "myCIService",
    "ClusterName": "myCICluster",
    "Timestamp": 1561586460000,
    "DesiredTaskCount": 2,
    "RunningTaskCount": 2,
    "PendingTaskCount": 0,
    "DeploymentCount": 1,
    "TaskSetCount": 0,
    "CloudWatchMetrics": [
        {
            "Namespace": "ECS/ContainerInsights",
            "Metrics": [
                {
                    "Name": "DesiredTaskCount",
                    "Unit": "Count"
                },
                {
                    "Name": "RunningTaskCount",
                    "Unit": "Count"
                },
                {
                    "Name": "PendingTaskCount",
                    "Unit": "Count"
                },
                {
                    "Name": "DeploymentCount",
                    "Unit": "Count"
                },
                {
                    "Name": "TaskSetCount",
                    "Unit": "Count"
                }
            ],
            "Dimensions": [
                [
                    "ServiceName",
                    "ClusterName"
                ]
            ]
        }
    ]
}
```

**Type: Cluster**

```
{
    "Version": "0",
    "Type": "Cluster",
    "ClusterName": "myCICluster",
    "Timestamp": 1561587300000,
    "TaskCount": 5,
    "ContainerInstanceCount": 5,
    "ServiceCount": 2,
    "CloudWatchMetrics": [
        {
            "Namespace": "ECS/ContainerInsights",
            "Metrics": [
                {
                    "Name": "TaskCount",
                    "Unit": "Count"
                },
                {
                    "Name": "ContainerInstanceCount",
                    "Unit": "Count"
                },
                {
                    "Name": "ServiceCount",
                    "Unit": "Count"
                }
            ],
            "Dimensions": [
                [
                    "ClusterName"
                ]
            ]
        }
    ]
}
```
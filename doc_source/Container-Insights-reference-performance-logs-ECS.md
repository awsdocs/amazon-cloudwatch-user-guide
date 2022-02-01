# Container Insights performance log events for Amazon ECS<a name="Container-Insights-reference-performance-logs-ECS"></a>

The following are examples of the performance log events that Container Insights collects from Amazon ECS\.

**Type: Container**

```
{
	"Version":"0",
	"Type":"Container",
	"ContainerName":"sleep",
	"TaskId":"7ac4dfba69214411b4783a3b8189c9ba",
	"TaskDefinitionFamily":"sleep360",
	"TaskDefinitionRevision":"1",
	"ContainerInstanceId":"0d7650e6dec34c1a9200f72098071e8f",
	"EC2InstanceId":"i-0c470579dbcdbd2f3",
	"ClusterName":"MyCluster",
	"Image":"busybox",
	"ContainerKnownStatus":"RUNNING",
	"Timestamp":1623963900000,
	"CpuUtilized":0.0,
	"CpuReserved":10.0,
	"MemoryUtilized":0,
	"MemoryReserved":10,
	"StorageReadBytes":0,
	"StorageWriteBytes":0,
	"NetworkRxBytes":0,
	"NetworkRxDropped":0,
	"NetworkRxErrors":0,
	"NetworkRxPackets":14,
	"NetworkTxBytes":0,
	"NetworkTxDropped":0,
	"NetworkTxErrors":0,
	"NetworkTxPackets":0
}
```

**Type: Task**

```
{
	"Version": "0",
	"Type": "Task",
	"TaskId": "7ac4dfba69214411b4783a3b8189c9ba",
	"TaskDefinitionFamily": "sleep360",
	"TaskDefinitionRevision": "1",
	"ContainerInstanceId": "0d7650e6dec34c1a9200f72098071e8f",
	"EC2InstanceId": "i-0c470579dbcdbd2f3",
	"ClusterName": "MyCluster",
	"AccountID": "637146863587",
	"Region": "us-west-2",
	"AvailabilityZone": "us-west-2b",
	"KnownStatus": "RUNNING",
	"LaunchType": "EC2",
	"PullStartedAt": 1623963608201,
	"PullStoppedAt": 1623963610065,
	"CreatedAt": 1623963607094,
	"StartedAt": 1623963610382,
	"Timestamp": 1623963900000,
	"CpuUtilized": 0.0,
	"CpuReserved": 10.0,
	"MemoryUtilized": 0,
	"MemoryReserved": 10,
	"StorageReadBytes": 0,
	"StorageWriteBytes": 0,
	"NetworkRxBytes": 0,
	"NetworkRxDropped": 0,
	"NetworkRxErrors": 0,
	"NetworkRxPackets": 14,
	"NetworkTxBytes": 0,
	"NetworkTxDropped": 0,
	"NetworkTxErrors": 0,
	"NetworkTxPackets": 0,
	"CloudWatchMetrics": [{
		"Namespace": "ECS/ContainerInsights",
		"Metrics": [{
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
				"ClusterName",
				"TaskDefinitionFamily"
			]
		]
	}]
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
# AWS OpsWorks Metrics and Dimensions<a name="ops-metricscollected"></a>

AWS OpsWorks sends metrics to CloudWatch for each active stack every minute\. Detailed monitoring is enabled by default\. For more information, see [Monitoring](http://docs.aws.amazon.com/opsworks/latest/userguide/monitoring.html) in the *AWS OpsWorks User Guide*\.

## AWS OpsWorks Stacks Metrics<a name="opsworks-metrics-dimensions"></a>

AWS OpsWorks Stacks sends the following metrics to CloudWatch every five minutes\.


**CPU Metrics**  

| Metric | Description | 
| --- | --- | 
|  `cpu_idle` |  The percentage of time that the CPU is idle\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_nice` |  The percentage of time that the CPU is handling processes with a positive `nice` value, which have a lower scheduling priority\. For more information about what this measures, see [nice \(Unix\)](http://en.wikipedia.org/wiki/Nice_(Unix))\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_steal` |  As AWS allocates hypervisor CPU resources among increasing numbers of instances, virtualization load rises, and can affect how often the hypervisor can perform requested work on an instance\. `cpu_steal` measures the percentage of time that an instance is waiting for the hypervisor to allocate physical CPU resources\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_system` |  The percentage of time that the CPU is handling system operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_user` |  The percentage of time that the CPU is handling user operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `cpu_waitio` |  The percentage of time that the CPU is waiting for input/output operations\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Memory Metrics**  

| Metric | Description | 
| --- | --- | 
|  `memory_buffers` |  The amount of buffered memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_cached` |  The amount of cached memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_free` |  The amount of free memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_swap` |  The amount of swap space\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_total` |  The total amount of memory\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `memory_used` |  The amount of memory in use\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Load Metrics**  

| Metric | Description | 
| --- | --- | 
|  `load_1` |  The load averaged over a one\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `load_5` |  The load averaged over a five\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 
|  `load_15` |  The load averaged over a 15\-minute window\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 


**Process Metrics**  

| Metric | Description | 
| --- | --- | 
|  `procs` |  The number of active processes\. Valid Dimensions: The IDs of the individual resources for which you are viewing metrics: StackId, LayerId, or InstanceId\. Valid Statistics: `Average`, `Minimum`, `Maximum`, `Sum`, or `Data Samples`\. Unit: None  | 

## Dimensions for AWS OpsWorks Metrics<a name="ops-metric-dimensions"></a>

AWS OpsWorks data can be filtered along any of the following dimensions in the table below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  StackId  |  Average values for a stack\.  | 
|  LayerId  |  Average values for a layer\.  | 
|  InstanceId  |  Average values for an instance\.  | 
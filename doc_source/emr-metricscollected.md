# Amazon EMR Metrics and Dimensions<a name="emr-metricscollected"></a>

Amazon EMR \(Amazon EMR\) sends metrics to CloudWatch\. All Amazon EMR job flows automatically send metrics in five\-minute intervals\. Metrics are archived for 15 months; after that period, the data is discarded\. For more information, see [Monitor Metrics with Amazon CloudWatch](http://docs.aws.amazon.com/emr/latest/DeveloperGuide/UsingEMR_ViewingMetrics.html) in the *Amazon EMR Developer Guide*\.

## Amazon EMR Metrics<a name="emr-metricsdata"></a>

Amazon EMR sends the following metrics to Amazon CloudWatch\.

The `AWS/ElasticMapReduce` namespace includes the following metrics\.

**Note**  
Amazon EMR pulls metrics from a cluster\. If a cluster becomes unreachable, no metrics are reported until the cluster becomes available again\.

The following are Hadoop 1 metrics:


| Metric | Description | 
| --- | --- | 
| Cluster Status | 
|  IsIdle  |  Indicates that a cluster is no longer performing work, but is still alive and accruing charges\. It is set to 1 if no tasks are running and no jobs are running, and set to 0 otherwise\. This value is checked at five\-minute intervals and a value of 1 indicates only that the cluster was idle when checked, not that it was idle for the entire five minutes\. To avoid false positives, you should raise an alarm when this value has been 1 for more than one consecutive 5\-minute check\. For example, you might raise an alarm on this value if it has been 1 for thirty minutes or longer\. Use case: Monitor cluster performance Units: *Boolean*  | 
|  JobsRunning  |  The number of jobs in the cluster that are currently running\. Use case: Monitor cluster health Units: *Count*  | 
|  JobsFailed  |  The number of jobs in the cluster that have failed\. Use case: Monitor cluster health Units: *Count*  | 
| Map/Reduce | 
|  MapTasksRunning  |  The number of running map tasks for each job\. If you have a scheduler installed and multiple jobs running, multiple graphs are generated\. Use case: Monitor cluster progress Units: *Count*  | 
|  MapTasksRemaining  |  The number of remaining map tasks for each job\. If you have a scheduler installed and multiple jobs running, multiple graphs are generated\. A remaining map task is one that is not in any of the following states: Running, Killed, or Completed\. Use case: Monitor cluster progress Units: *Count*  | 
|  MapSlotsOpen  |  The unused map task capacity\. This is calculated as the maximum number of map tasks for a given cluster, less the total number of map tasks currently running in that cluster\. Use case: Analyze cluster performance Units: *Count*  | 
|  RemainingMapTasksPerSlot  |  The ratio of the total map tasks remaining to the total map slots available in the cluster\. Use case: Analyze cluster performance Units: *Ratio*  | 
|  ReduceTasksRunning  |  The number of running reduce tasks for each job\. If you have a scheduler installed and multiple jobs running, multiple graphs are generated\. Use case: Monitor cluster progress Units: *Count*  | 
|  ReduceTasksRemaining  |  The number of remaining reduce tasks for each job\. If you have a scheduler installed and multiple jobs running, multiple graphs are generated\. Use case: Monitor cluster progress Units: *Count*  | 
|  ReduceSlotsOpen  |  Unused reduce task capacity\. This is calculated as the maximum reduce task capacity for a given cluster, less the number of reduce tasks currently running in that cluster\. Use case: Analyze cluster performance Units: *Count*  | 
| Node Status | 
|  CoreNodesRunning  |  The number of core nodes working\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  CoreNodesPending  |  The number of core nodes waiting to be assigned\. All of the core nodes requested may not be immediately available; this metric reports the pending requests\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  LiveDataNodes  |  The percentage of data nodes that are receiving work from Hadoop\. Use case: Monitor cluster health Units: *Percent*  | 
|  TaskNodesRunning  |  The number of task nodes working\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  TaskNodesPending  |  The number of core nodes waiting to be assigned\. All of the task nodes requested may not be immediately available; this metric reports the pending requests\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  LiveTaskTrackers  |  The percentage of task trackers that are functional\. Use case: Monitor cluster health Units: *Percent*  | 
| IO | 
|  S3BytesWritten  |  The number of bytes written to Amazon S3\. This metric aggregates MapReduce jobs only, and does not apply for other workloads on EMR\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  S3BytesRead  |  The number of bytes read from Amazon S3\. This metric aggregates MapReduce jobs only, and does not apply for other workloads on EMR\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  HDFSUtilization  |  The percentage of HDFS storage currently used\. Use case: Analyze cluster performance Units: *Percent*  | 
|  HDFSBytesRead  |  The number of bytes read from HDFS\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Bytes*  | 
|  HDFSBytesWritten  |  The number of bytes written to HDFS\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Bytes*  | 
|  MissingBlocks  |  The number of blocks in which HDFS has no replicas\. These might be corrupt blocks\. Use case: Monitor cluster health Units: *Count*  | 
|  TotalLoad  |  The current, total number of readers and writers reported by all DataNodes in a cluster\. Use case: Diagnose the degree to which high I/O might be contributing to poor job execution performance\. Worker nodes running the DataNode daemon must also perform map and reduce tasks\. Persistently high TotalLoad values over time can indicate that high I/O might be a contributing factor to poor performance\. Occasional spikes in this value are typical and do not usually indicate a problem\. Units: *Count*  | 
| HBase | 
|  BackupFailed  |  Whether the last backup failed\. This is set to 0 by default and updated to 1 if the previous backup attempt failed\. This metric is only reported for HBase clusters\. Use case: Monitor HBase backups Units: *Count*  | 
|  MostRecentBackupDuration  |  The amount of time it took the previous backup to complete\. This metric is set regardless of whether the last completed backup succeeded or failed\. While the backup is ongoing, this metric returns the number of minutes after the backup started\. This metric is only reported for HBase clusters\. Use case: Monitor HBase Backups Units: *Minutes*  | 
|  TimeSinceLastSuccessfulBackup  |  The number of elapsed minutes after the last successful HBase backup started on your cluster\. This metric is only reported for HBase clusters\. Use case: Monitor HBase backups Units: *Minutes*  | 

The following metrics are available for Hadoop 2 AMIs:


| Metric | Description | 
| --- | --- | 
| Cluster Status | 
|  IsIdle  |  Indicates that a cluster is no longer performing work, but is still alive and accruing charges\. It is set to 1 if no tasks are running and no jobs are running, and set to 0 otherwise\. This value is checked at five\-minute intervals and a value of 1 indicates only that the cluster was idle when checked, not that it was idle for the entire five minutes\. To avoid false positives, you should raise an alarm when this value has been 1 for more than one consecutive 5\-minute check\. For example, you might raise an alarm on this value if it has been 1 for thirty minutes or longer\. Use case: Monitor cluster performance Units: *Boolean*  | 
|  ContainerAllocated  |  The number of resource containers allocated by the ResourceManager\. Use case: Monitor cluster progress Units: *Count*  | 
|  ContainerReserved  |  The number of containers reserved\. Use case: Monitor cluster progress Units: *Count*  | 
|  ContainerPending  |  The number of containers in the queue that have not yet been allocated\. Use case: Monitor cluster progress Units: *Count*  | 
|  ContainerPendingRatio  |  The ratio of pending containers to containers allocated \(ContainerPendingRatio = ContainerPending / ContainerAllocated\)\. If ContainerAllocated = 0, then ContainerPendingRatio = ContainerPending\. The value of ContainerPendingRatio represents a number, not a percentage\. This value is useful for scaling cluster resources based on container allocation behavior\. Units: *Count*  | 
|  AppsCompleted  |  The number of applications submitted to YARN that have completed\. Use case: Monitor cluster progress Units: *Count*  | 
|  AppsFailed  |  The number of applications submitted to YARN that have failed to complete\. Use case: Monitor cluster progress, Monitor cluster health Units: *Count*  | 
|  AppsKilled  |  The number of applications submitted to YARN that have been killed\. Use case: Monitor cluster progress, Monitor cluster health Units: *Count*  | 
|  AppsPending  |  The number of applications submitted to YARN that are in a pending state\. Use case: Monitor cluster progress Units: *Count*  | 
|  AppsRunning  |  The number of applications submitted to YARN that are running\. Use case: Monitor cluster progress Units: *Count*  | 
|  AppsSubmitted  |  The number of applications submitted to YARN\. Use case: Monitor cluster progress Units: *Count*  | 
| Node Status | 
|  CoreNodesRunning  |  The number of core nodes working\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  CoreNodesPending  |  The number of core nodes waiting to be assigned\. All of the core nodes requested may not be immediately available; this metric reports the pending requests\. Data points for this metric are reported only when a corresponding instance group exists\. Use case: Monitor cluster health Units: *Count*  | 
|  LiveDataNodes  |  The percentage of data nodes that are receiving work from Hadoop\. Use case: Monitor cluster health Units: *Percent*  | 
|  MRTotalNodes  |  The number of nodes presently available to MapReduce jobs\. Equivalent to YARN metric `mapred.resourcemanager.TotalNodes`\. Use ase: Monitor cluster progress Units: *Count*  | 
|  MRActiveNodes  |  The number of nodes presently running MapReduce tasks or jobs\. Equivalent to YARN metric `mapred.resourcemanager.NoOfActiveNodes`\. Use case: Monitor cluster progress Units: *Count*  | 
|  MRLostNodes  |  The number of nodes allocated to MapReduce that have been marked in a LOST state\. Equivalent to YARN metric `mapred.resourcemanager.NoOfLostNodes`\. Use case: Monitor cluster health, Monitor cluster progress Units: *Count*  | 
|  MRUnhealthyNodes  |  The number of nodes available to MapReduce jobs marked in an UNHEALTHY state\. Equivalent to YARN metric `mapred.resourcemanager.NoOfUnhealthyNodes`\. Use case: Monitor cluster progress Units: *Count*  | 
|  MRDecommissionedNodes  |  The number of nodes allocated to MapReduce applications that have been marked in a DECOMMISSIONED state\. Equivalent to YARN metric `mapred.resourcemanager.NoOfDecommissionedNodes`\. Use ase: Monitor cluster health, Monitor cluster progress Units: *Count*  | 
|  MRRebootedNodes  |  The number of nodes available to MapReduce that have been rebooted and marked in a REBOOTED state\. Equivalent to YARN metric `mapred.resourcemanager.NoOfRebootedNodes`\. Use case: Monitor cluster health, Monitor cluster progress Units: *Count*  | 
| IO | 
|  S3BytesWritten  |  The number of bytes written to Amazon S3\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  S3BytesRead  |  The number of bytes read from Amazon S3\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  HDFSUtilization  |  The percentage of HDFS storage currently used\. Use case: Analyze cluster performance Units: *Percent*  | 
|  HDFSBytesRead  |  The number of bytes read from HDFS\. This metric aggregates MapReduce jobs only, and does not apply for other workloads on EMR\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  HDFSBytesWritten  |  The number of bytes written to HDFS\. This metric aggregates MapReduce jobs only, and does not apply for other workloads on EMR\. Use case: Analyze cluster performance, Monitor cluster progress Units: *Count*  | 
|  MissingBlocks  |  The number of blocks in which HDFS has no replicas\. These might be corrupt blocks\. Use case: Monitor cluster health Units: *Count*  | 
|  CorruptBlocks  |  The number of blocks that HDFS reports as corrupted\. Use case: Monitor cluster health Units: *Count*  | 
|  TotalLoad  |  The total number of concurrent data transfers\. Use case: Monitor cluster health Units: *Count*  | 
|  MemoryTotalMB  |  The total amount of memory in the cluster\. Use case: Monitor cluster progress Units: *Bytes*  | 
|  MemoryReservedMB  |  The amount of memory reserved\. Use case: Monitor cluster progress Units: *Bytes*  | 
|  MemoryAvailableMB  |  The amount of memory available to be allocated\. Use case: Monitor cluster progress Units: *Bytes*  | 
|  YARNMemoryAvailablePercentage  |  The percentage of remaining memory available to YARN \(YARNMemoryAvailablePercentage = MemoryAvailableMB / MemoryTotalMB\)\. This value is useful for scaling cluster resources based on YARN memory usage\.  | 
|  MemoryAllocatedMB  |  The amount of memory allocated to the cluster\. Use case: Monitor cluster progress Units: *Bytes*  | 
|  PendingDeletionBlocks  |  The number of blocks marked for deletion\. Use case: Monitor cluster progress, Monitor cluster health Units: *Count*  | 
|  UnderReplicatedBlocks  |  The number of blocks that need to be replicated one or more times\. Use case: Monitor cluster progress, Monitor cluster health Units: *Count*  | 
|  DfsPendingReplicationBlocks  |  The status of block replication: blocks being replicated, age of replication requests, and unsuccessful replication requests\. Use case: Monitor cluster progress, Monitor cluster health Units: *Count*  | 
|  CapacityRemainingGB  |  The amount of remaining HDFS disk capacity\.  Use case: Monitor cluster progress, Monitor cluster health Units: *Bytes*  | 
| HBase | 
|  HbaseBackupFailed  |  Whether the last backup failed\. This is set to 0 by default and updated to 1 if the previous backup attempt failed\. This metric is only reported for HBase clusters\. Use case: Monitor HBase backups Units: *Count*  | 
|  MostRecentBackupDuration  |  The amount of time it took the previous backup to complete\. This metric is set regardless of whether the last completed backup succeeded or failed\. While the backup is ongoing, this metric returns the number of minutes after the backup started\. This metric is only reported for HBase clusters\. Use case: Monitor HBase Backups Units: *Minutes*  | 
|  TimeSinceLastSuccessfulBackup  |  The number of elapsed minutes after the last successful HBase backup started on your cluster\. This metric is only reported for HBase clusters\. Use case: Monitor HBase backups Units: *Minutes*  | 

## Amazon EMR Dimensions<a name="emr-metricsdimensions"></a>

The following dimensions are available for Amazon EMR\.


| Dimension  | Description  | 
| --- | --- | 
| JobFlowId |  The same as cluster ID, which is the unique identifier of a cluster in the form j\-XXXXXXXXXXXXX\. Find this value by clicking on the cluster in the Amazon EMR console\.  | 
| JobId |  The identifier of a job within a cluster\. You can use this to filter the metrics returned from a cluster down to those that apply to a single job within the cluster\. JobId takes the form job\_XXXXXXXXXXXX\_XXXX\.  | 
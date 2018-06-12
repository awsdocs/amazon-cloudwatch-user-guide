# AWS Storage Gateway Metrics and Dimensions<a name="awssg-metricscollected"></a>

 AWS Storage Gateway sends data points to CloudWatch for several metrics\. All active queues automatically send five\-minute metrics to CloudWatch\. Detailed monitoring, or one\-minute metrics, is currently unavailable for AWS Storage Gateway\. For more information, see [Monitoring Your AWS Storage Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/GatewayMetrics.html) in the *AWS Storage Gateway User Guide*\.

## AWS Storage Gateway Metrics<a name="storagegateway-metrics"></a>

The `AWS/StorageGateway` namespace includes the following metrics\.

You can use these metrics to get information about your gateways\. Specify the `GatewayId` or `GatewayName` dimension for each metric to view the data for a gateway\. Note that these metrics are measured in 5\-minute intervals\.


| Metric | Description | Applies To\.\. | 
| --- | --- | --- | 
| CacheHitPercent |  Percent of application reads served from the cache\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CachePercentUsed |  Percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CachePercentDirty |  Percent of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Units: Percent  |  File, Cached volumes and Tape\.  | 
| CloudBytesDownloaded |  The total number of compressed bytes that the gateway downloaded from AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure input/output operations per second \(IOPS\)\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| CloudDownloadLatency |  The total number of milliseconds spent reading data from AWS during the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| CloudBytesUploaded |  The total number of compressed bytes that the gateway uploaded to AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\.  Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| UploadBufferFree |  The total amount of unused space in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Cached volumes and Tape\.  | 
| CacheFree |  The total amount of unused space in the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes, and Tape\.  | 
| UploadBufferPercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Percent  |   Cached volumes and Tape\.  | 
| UploadBufferUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Cached volumes and Tape\.  | 
| CacheUsed |  The total number of bytes being used in the gateway's cache storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes and Tape\.  | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period for all volumes in the gateway\. These bytes are kept in your gateway's working storage\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| TotalCacheSize |  The total size of the cache in bytes\. The sample is taken at the end of the reporting period\. Units: Bytes  |  File, Cached volumes, and Tape\.  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period for all volumes in the gateway\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  |  File, Cached volumes, Stored volumes and Tape\.  | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period for all volumes in the gateway\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  |  File, Cached volumes, Stored volumes and Tape\.  | 
| TimeSinceLastRecoveryPoint |  The time since the last available recovery point\. For more information, see [Using Volume Recovery Points for Your Cached Volumes Setup ](http://docs.aws.amazon.com/storagegateway/latest/userguide/troubleshoot-volume-issues.html#RecoverySnapshotTroubleshooting.html) Units: Seconds  |  Cached volumes and Stored volumes\.  | 
| WorkingStorageFree |  The total amount of unused space in the gateway's working storage\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Stored volumes only\.  | 
| WorkingStoragePercentUsed |  Percent use of the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Percent  |  Stored volumes only\.  | 
| WorkingStorageUsed |  The total number of bytes being used in the gateway's upload buffer\. The sample is taken at the end of the reporting period\. Units: Bytes  |  Stored volumes only\.  | 

The following table describes the AWS Storage Gateway metrics that you can use to get information about your storage volumes\. Specify the `VolumeId` dimension for each metric to view the data for a storage volume\.


| Metric | Description | Cached volumes | Stored volumes | 
| --- | --- | --- | --- | 
| CacheHitPercent |  Percent of application read operations from the volume that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the volume, this metric reports 100 percent\.  Units: Percent  | yes | no | 
| CachePercentDirty |  The volume's contribution to the overall percentage of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that has not been persisted to AWS\. For more information, see [Monitoring Your Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html)\. Units: Percent  | yes | no | 
| CachePercentUsed |  The volume's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. For more information, see [Monitoring Your Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html)\. Units: Percent  | yes | no | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | yes | yes | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | yes | yes | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | yes | yes | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | yes | yes | 
| QueuedWrites |  The number of bytes waiting to be written to AWS, sampled at the end of the reporting period\.  Units: Bytes  | yes | yes | 

The following table describes the Storage Gateway metrics that you can use to get information about your file shares\. 


| Metric | Description | 
| --- | --- | 
| CacheHitPercent |  Percent of application read operations from the file shares that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the file share, this metric reports 100 percent\.  Units: Percent  | 
| CachePercentDirty |  The file share's contribution to the overall percentage of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that has not been persisted to AWS\. For more information, see [Monitoring Your Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html)\. Units: Percent  | 
| CachePercentUsed |  The file share's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. For more information, see [Monitoring Your Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html)\. Units: Percent  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for a file share\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 
| ReadTime |  The total number of milliseconds spent to do read operations from your on\-premises applications in the reporting period\. Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 
| WriteTime |  The total number of milliseconds spent to do write operations from your on\-premises applications in the reporting period\.  Use this metric with the `Average` statistic to measure latency\. Units: Milliseconds  | 

## Dimensions for AWS Storage Gateway Metrics<a name="storagegateway-metric-dimensions"></a>

The Amazon CloudWatch namespace for the AWS Storage Gateway service is `AWS/StorageGateway`\. Data is available automatically in 5\-minute periods at no charge\. 


|  Dimension  |  Description  | 
| --- | --- | 
|  GatewayId, GatewayName |  These dimensions filter the data you request to gateway\-specific metrics\. You can identify a gateway to work by its `GatewayId` or its `GatewayName`\. However, note that if the name of your gateway was changed for the time range that you are interested in viewing metrics, then you should use the `GatewayId`\.   Throughput and latency data of a gateway is based on all the volumes for the gateway\. For information about working with gateway metrics, see [Measuring Performance Between Your Gateway and AWS](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html#PerfGatewayAWS-common)\.   | 
|  VolumeId  |  This dimension filters the data you request to volume\-specific metrics\. Identify a storage volume to work with by its `VolumeId`\. For information about working with volume metrics, see [Measuring Performance Between Your Application and Gateway](http://docs.aws.amazon.com/storagegateway/latest/userguide/Main_monitoring-gateways-common.html#PerfAppGateway-common)\.   | 
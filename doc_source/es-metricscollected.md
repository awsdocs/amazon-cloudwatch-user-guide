# Amazon Elasticsearch Service Metrics and Dimensions<a name="es-metricscollected"></a>

Amazon Elasticsearch Service sends data to CloudWatch every minute\. You can create alarms using [Amazon Elasticsearch Service Metrics and Dimensions](#es-metricscollected)\. For more information, see [Monitoring Cluster Metrics and Statistics with Amazon CloudWatch](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-cloudwatchmetrics) in the *Amazon Elasticsearch Service Developer Guide*\.

## Amazon Elasticsearch Service Metrics<a name="elasticsearch-metrics"></a>

The `AWS/ES` namespace includes the following metrics for clusters\.


| Metric | Description | 
| --- | --- | 
| ClusterStatus\.green | Indicates that all index shards are allocated to nodes in the cluster\. Relevant statistics: Minimum, Maximum | 
| ClusterStatus\.yellow | Indicates that the primary shards for all indices are allocated to nodes in a cluster, but the replica shards for at least one index are not\. Single node clusters always initialize with this cluster status because there is no second node to which a replica can be assigned\. You can either increase your node count to obtain a green cluster status, or you can use the Elasticsearch API to set the number\_of\_replicas setting for your index to 0\. To learn more, see [Configuring Amazon Elasticsearch Service Domains](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html#es-createdomains-configure-cluster)\.Relevant statistics: Minimum, Maximum | 
| ClusterStatus\.red | Indicates that the primary and replica shards of at least one index are not allocated to nodes in a cluster\. To recover, you must delete the indices or restore a snapshot and then add EBS\-based storage, use larger instance types, or add instances\. For more information, see [Red Cluster Status](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/aes-handling-errors.html#aes-handling-errors-red-cluster-status)\. Relevant statistics: Minimum, Maximum | 
| Nodes | The number of nodes in the Amazon ES cluster\. Relevant Statistics: Minimum, Maximum, Average | 
| SearchableDocuments | The total number of searchable documents across all indices in the cluster\. Relevant statistics: Minimum, Maximum, Average | 
| DeletedDocuments | The total number of documents marked for deletion across all indices in the cluster\. These documents no longer appear in search results, but Elasticsearch only removes deleted documents from disk during segment merges\. This metric increases after delete requests and decreases after segment merges\. Relevant statistics: Minimum, Maximum, Average | 
| CPUUtilization | The maximum percentage of CPU resources used for data nodes in the cluster\. Relevant statistics: Maximum, Average | 
| FreeStorageSpace | The free space, in megabytes, for nodes in the cluster\. `Sum` shows total free space for the cluster\. `Minimum`, `Maximum`, and `Average` show free space for individual nodes\. Amazon ES throws a `ClusterBlockException` when this metric reaches `0`\. To recover, you must either delete indices, add larger instances, or add EBS\-based storage to existing instances\. To learn more, see [Recovering from a Lack of Free Storage Space](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/aes-handling-errors.html#aes-handling-errors-watermark) `FreeStorageSpace` will always be lower than the value that the Elasticsearch `_cluster/stats` API provides\. Amazon ES reserves a percentage of the storage space on each instance for internal operations\. Relevant statistics: Minimum, Maximum, Average, Sum | 
| ClusterUsedSpace | The total used space, in megabytes, for a cluster\. You can view this metric in the Amazon CloudWatch console, but not in the Amazon ES console\. Relevant statistics: Minimum, Maximum | 
| ClusterIndexWritesBlocked | Indicates whether your cluster is accepting or blocking incoming write requests\. A value of 0 means that the cluster is accepting requests\. A value of 1 means that it is blocking requests\. Many factors can cause a cluster to begin blocking requests\. Some common factors include the following: `FreeStorageSpace` is too low, `JVMMemoryPressure` is too high, or `CPUUtilization` is too high\. To alleviate this issue, consider adding more disk space or scaling your cluster\. Relevant statistics: Maximum You can view this metric in the Amazon CloudWatch console, but not the Amazon ES console\. | 
| JVMMemoryPressure | The maximum percentage of the Java heap used for all data nodes in the cluster\. Relevant statistics: Maximum | 
| AutomatedSnapshotFailure | The number of failed automated snapshots for the cluster\. A value of `1` indicates that no automated snapshot was taken for the domain in the previous 36 hours\. Relevant statistics: Minimum, Maximum | 
| CPUCreditBalance | The remaining CPU credits available for data nodes in the cluster\. A CPU credit provides the performance of a full CPU core for one minute\. For more information, see [CPU Credits](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/t2-instances.html#t2-instances-cpu-credits) in the *Amazon EC2 Developer Guide*\. This metric is available only for the t2\.micro\.elasticsearch, t2\.small\.elasticsearch, and t2\.medium\.elasticsearch instance types\. Relevant statistics: Minimum | 
| KibanaHealthyNodes | A health check for Kibana\. A value of 1 indicates normal behavior\. A value of 0 indicates that Kibana is inaccessible\. In most cases, the health of Kibana mirrors the health of the cluster\. Relevant statistics: Minimum You can view this metric on the Amazon CloudWatch console, but not the Amazon ES console\. | 
| KMSKeyError | A value of 1 indicates that the KMS customer master key used to encrypt data at rest has been disabled\. To restore the domain to normal operations, re\-enable the key\. The console displays this metric only for domains that encrypt data at rest\.\. Relevant statistics: Minimum, Maximum | 
| KMSKeyInaccessible | A value of 1 indicates that the KMS customer master key used to encrypt data at rest has been deleted or revoked its grants to Amazon ES\. You can't recover domains that are in this state\. If you have a manual snapshot, though, you can use it to migrate the domain's data to a new domain\. The console displays this metric only for domains that encrypt data at rest\. Relevant statistics: Minimum, Maximum | 
| InvalidHostHeaderRequests | The number of HTTP requests made to the Elasticsearch cluster that included an invalid \(or missing\) host header\. Valid requests include the domain endpoint as the host header value\. If you see large values for this metric, check that your Elasticsearch clients include the proper host header value in their requests\. Otherwise, Amazon ES might reject the requests\. You can also update the domain’s access policy to require signed requests\. Relevant statistics: Sum | 
| ElasticsearchRequests | The number of requests made to the Elasticsearch cluster\. Relevant statistics: Sum | 

The `AWS/ES` namespace includes the following metrics for dedicated master nodes\.


| Metric | Description | 
| --- | --- | 
| MasterCPUUtilization | The maximum percentage of CPU resources used by the dedicated master nodes\. We recommend increasing the size of the instance type when this metric reaches 60 percent\. Relevant statistics: Average | 
| MasterFreeStorageSpace | This metric is not relevant and can be ignored\. The service does not use master nodes as data nodes\. | 
| MasterJVMMemoryPressure | The maximum percentage of the Java heap used for all dedicated master nodes in the cluster\. We recommend moving to a larger instance type when this metric reaches 85 percent\. Relevant statistics: Maximum | 
| MasterCPUCreditBalance | The remaining CPU credits available for dedicated master nodes in the cluster\. A CPU credit provides the performance of a full CPU core for one minute\. For more information, see [CPU Credits](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/t2-instances.html#t2-instances-cpu-credits) in the *Amazon EC2 User Guide for Linux Instances*\. This metric is available only for the t2\.micro\.elasticsearch, t2\.small\.elasticsearch, and t2\.medium\.elasticsearch instance types\. Relevant statistics: Minimum | 
| MasterReachableFromNode | A health check for `MasterNotDiscovered` exceptions\. A value of 1 indicates normal behavior\. A value of 0 indicates that `/_cluster/health/` is failing\. Failures mean that the master node stopped or is not reachable\. They are usually the result of a network connectivity issue or AWS dependency problem\. Relevant statistics: Minimum You can view this metric on the Amazon CloudWatch console, but not the Amazon ES console\. | 

The `AWS/ES` namespace includes the following metrics for EBS volumes\.


| Metric | Description | 
| --- | --- | 
| ReadLatency | The latency, in seconds, for read operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 
| WriteLatency | The latency, in seconds, for write operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 
| ReadThroughput | The throughput, in bytes per second, for read operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 
| WriteThroughput | The throughput, in bytes per second, for write operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 
| DiskQueueDepth | The number of pending input and output \(I/O\) requests for an EBS volume\. Relevant statistics: Minimum, Maximum, Average | 
| ReadIOPS | The number of input and output \(I/O\) operations per second for read operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 
| WriteIOPS | The number of input and output \(I/O\) operations per second for write operations on EBS volumes\. Relevant statistics: Minimum, Maximum, Average | 

## Dimensions for Amazon Elasticsearch Service Metrics<a name="es-metric-dimensions"></a>

To filter the metrics, use the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `ClientId` |  The AWS account ID\.  | 
| `DomainName` |  The name of the search domain\.  | 
# Amazon Redshift Metrics and Dimensions<a name="rs-metricscollected"></a>

Amazon Redshift sends metrics to CloudWatch for each active cluster every minute\. Detailed monitoring is enabled by default\. For more information, see [Monitoring Amazon Redshift Cluster Performance](http://docs.aws.amazon.com/redshift/latest/mgmt/metrics.html) in the *Amazon Redshift Cluster Management Guide*\.

## Amazon Redshift Metrics<a name="redshift-metrics"></a>

The `AWS/Redshift` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| CPUUtilization |  The percentage of CPU utilization\. For clusters, this metric represents an aggregation of all nodes \(leader and compute\) CPU utilization values\. Units: Percent Dimensions: `NodeID`, `ClusterIdentifier`  | 
| DatabaseConnections |  The number of database connections to a cluster\. Units: Count Dimensions: `ClusterIdentifier`  | 
| HealthStatus |  Indicates the health of the cluster\. Every minute the cluster connects to its database and performs a simple query\. If it is able to perform this operation successfully, the cluster is considered healthy\. Otherwise, the cluster is unhealthy\. An unhealthy status can occur when the cluster database is under extremely heavy load or if there is a configuration problem with a database on the cluster\. The exception to this is when the cluster is undergoing maintenance\. Even though your cluster might be unavailable due to maintenance tasks, the cluster remains in HEALTHY state\. For more information, see [ Maintenance Windows](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-maintenance-windows) in the *Amazon Redshift Cluster Management Guide*\.   In Amazon CloudWatch this metric is reported as 1 or 0 whereas in the Amazon CloudWatch console, this metric is displayed with the words `HEALTHY` or `UNHEALTHY` for convenience\. When this metric is displayed in the Amazon CloudWatch console, sampling averages are ignored and only `HEALTHY` or `UNHEALTHY` are displayed\. In Amazon CloudWatch, values different than 1 and 0 may occur because of sampling issue\. Any value below 1 for `HealthStatus` is reported as 0 \(`UNHEALTHY`\)\.  Units: 1/0 \(`HEALTHY`/`UNHEALTHY` in the Amazon CloudWatch console\) Dimensions: `ClusterIdentifier`  | 
| MaintenanceMode |  Indicates whether the cluster is in maintenance mode\.  In Amazon CloudWatch this metric is reported as 1 or 0 whereas in the Amazon CloudWatch console, this metric is displayed with the words `ON` or `OFF` for convenience\. When this metric is displayed in the Amazon CloudWatch console, sampling averages are ignored and only `ON` or `OFF` are displayed\. In Amazon CloudWatch, values different than 1 and 0 may occur because of sampling issues\. Any value greater than 0 for `MaintenanceMode` is reported as 1 \(`ON`\)\.  Units: 1/0 \(`ON`/`OFF` in the Amazon CloudWatch console\)\. Dimensions: `ClusterIdentifier`  | 
| NetworkReceiveThroughput |  The rate at which the node or cluster receives data\. Units: Bytes/seconds \(MB/s in the Amazon CloudWatch console\) Dimensions: `NodeID`, `ClusterIdentifier`  | 
| NetworkTransmitThroughput |  The rate at which the node or cluster writes data\. Units: Bytes/second \(MB/s in the Amazon CloudWatch console\) Dimensions: `NodeID`, `ClusterIdentifier`  | 
| PercentageDiskSpaceUsed |  The percent of disk space used\. Units: Percent Dimensions: `NodeID`, `ClusterIdentifier`  | 
| ReadIOPS |  The average number of disk read operations per second\. Units: Count/second Dimensions: `NodeID`  | 
| ReadLatency |  The average amount of time taken for disk read I/O operations\. Units: Seconds Dimensions: `NodeID`  | 
| ReadThroughput |  The average number of bytes read from disk per second\. Units: Bytes \(GB/s in the Amazon CloudWatch console\) Dimensions: `NodeID`  | 
| WriteIOPS |  The average number of write operations per second during the polling period\. Units: Count/seconds Dimensions: `NodeID`  | 
| WriteLatency |  The average amount of time taken for disk write I/O operations\. Units: Seconds Dimensions: `NodeID`  | 
| WriteThroughput |  The average number of bytes written to disk per second\. Units: Bytes \(GB/s in the Amazon CloudWatch console\) Dimensions: `NodeID`  | 

## Dimensions for Amazon Redshift Metrics<a name="rs-metric-dimensions"></a>

Amazon Redshift data can be filtered along any of the following dimensions in the table below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  NodeID  |  Filters requested data that is specific to the nodes of a cluster\. `NodeID` will be either "Leader", "Shared", or "Compute\-N" where N is 0, 1, \.\.\. for the number of nodes in the cluster\. "Shared" means that the cluster has only one node, i\.e\. the leader node and compute node are combined\. Metrics are reported for the leader node and compute nodes only for `CPUUtilization`, `NetworkTransmitThroughput`, and `ReadIOPS`\. Other metrics that use the `NodeId` dimension are reported only for compute nodes\.  | 
|  ClusterIdentifier  |  Filters requested data that is specific to the cluster\. Metrics that are specific to clusters include `HealthStatus`, `MaintenanceMode`, and `DatabaseConnections`\. In general metrics in for this dimension \(e\.g\. `ReadIOPS`\) that are also metrics of nodes represent an aggregate of the node metric data\. You should take care in interpreting these metrics because they aggregate behavior of leader and compute nodes\.  | 
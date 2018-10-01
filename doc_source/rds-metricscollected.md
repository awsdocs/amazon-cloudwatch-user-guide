# Amazon RDS Metrics and Dimensions<a name="rds-metricscollected"></a>

Amazon Relational Database Service sends metrics to CloudWatch for each active database instance every minute\. Detailed monitoring is enabled by default\. For more information, see [Monitoring a DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Monitoring.html) in the *Amazon RDS User Guide*\.

## Amazon RDS Metrics<a name="rds-metrics"></a>

The `AWS/RDS` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| BinLogDiskUsage |  The amount of disk space occupied by binary logs on the master\. Applies to MySQL read replicas\. Units: Bytes  | 
| BurstBalance |  The percent of General Purpose SSD \(gp2\) burst\-bucket I/O credits available\.  Units: Percent  | 
| CPUUtilization |  The percentage of CPU utilization\. Units: Percent  | 
| CPUCreditUsage |  \[T2 instances\] The number of CPU credits spent by the instance for CPU utilization\. One CPU credit equals one vCPU running at 100% utilization for one minute or an equivalent combination of vCPUs, utilization, and time \(for example, one vCPU running at 50% utilization for two minutes or two vCPUs running at 25% utilization for two minutes\)\. CPU credit metrics are available at a five\-minute frequency only\. If you specify a period greater than five minutes, use the `Sum` statistic instead of the `Average` statistic\. Units: Credits \(vCPU\-minutes\)  | 
| CPUCreditBalance |  \[T2 instances\] The number of earned CPU credits that an instance has accrued since it was launched or started\. For T2 Standard, the `CPUCreditBalance` also includes the number of launch credits that have been accrued\. Credits are accrued in the credit balance after they are earned, and removed from the credit balance when they are spent\. The credit balance has a maximum limit, determined by the instance size\. Once the limit is reached, any new credits that are earned are discarded\. For T2 Standard, launch credits do not count towards the limit\. The credits in the `CPUCreditBalance` are available for the instance to spend to burst beyond its baseline CPU utilization\. When an instance is running, credits in the `CPUCreditBalance` do not expire\. When the instance stops, the `CPUCreditBalance` does not persist, and all accrued credits are lost\. CPU credit metrics are available at a five\-minute frequency only\. Units: Credits \(vCPU\-minutes\)  | 
| DatabaseConnections |  The number of database connections in use\. Units: Count  | 
| DiskQueueDepth |  The number of outstanding IOs \(read/write requests\) waiting to access the disk\. Units: Count  | 
| FreeableMemory |  The amount of available random access memory\. Units: Bytes  | 
| FreeStorageSpace |  The amount of available storage space\. Units: Bytes  | 
| MaximumUsedTransactionIDs |  The maximum transaction ID that has been used\. Applies to PostgreSQL\. Units: Count  | 
| NetworkReceiveThroughput |  The incoming \(Receive\) network traffic on the DB instance, including both customer database traffic and Amazon RDS traffic used for monitoring and replication\. Units: Bytes/second  | 
| NetworkTransmitThroughput |  The outgoing \(Transmit\) network traffic on the DB instance, including both customer database traffic and Amazon RDS traffic used for monitoring and replication\. Units: Bytes/second  | 
| OldestReplicationSlotLag |  The lagging size of the replica lagging the most in terms of WAL data received\. Applies to PostgreSQL\. Units: Megabytes  | 
| ReadIOPS |  The average number of disk read I/O operations per second\. Units: Count/Second  | 
| ReadLatency |  The average amount of time taken per disk I/O operation\. Units: Seconds  | 
| ReadThroughput |  The average number of bytes read from disk per second\. Units: Bytes/Second  | 
| ReplicaLag |  The amount of time a Read Replica DB instance lags behind the source DB instance\. Applies to MySQL, MariaDB, and PostgreSQL Read Replicas\. Units: Seconds  | 
| ReplicationSlotDiskUsage |  The disk space used by replication slot files\. Applies to PostgreSQL\. Units: Megabytes  | 
| SwapUsage |  The amount of swap space used on the DB instance\. This metric is not available for SQL Server\. Units: Bytes  | 
| TransactionLogsDiskUsage |  The disk space used by transaction logs\. Applies to PostgreSQL\. Units: Megabytes  | 
| TransactionLogsGeneration |  The size of transaction logs generated per second\. Applies to PostgreSQL\. Units: Megabytes/second  | 
| WriteIOPS |  The average number of disk write I/O operations per second\. Units: Count/Second  | 
| WriteLatency |  The average amount of time taken per disk I/O operation\. Units: Seconds  | 
| WriteThroughput |  The average number of bytes written to disk per second\. Units: Bytes/Second  | 

## Amazon Aurora Metrics<a name="aurora-metrics"></a>

The `AWS/RDS` namespace includes the following metrics that apply to database entities running on Amazon Aurora\.


| Metric | Description | Applies to | 
| --- | --- | --- | 
|  `ActiveTransactions`  |  The average number of current transactions executing on an Aurora database instance per second\.  | Aurora MySQL   | 
|  `AuroraBinlogReplicaLag`  |  The amount of time a replica DB cluster running on Aurora with MySQL compatibility lags behind the source DB cluster\. This metric reports the value of the `Seconds_Behind_Master` field of the MySQL `SHOW SLAVE STATUS` command\. This metric is useful for monitoring replica lag between Aurora DB clusters that are replicating across different AWS Regions\. For more information, see [Aurora MySQL Replication](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/AuroraMySQL.Replication.CrossRegion.html)\.  | Aurora MySQL   | 
|  `AuroraReplicaLag`  |  For an Aurora Replica, the amount of lag when replicating updates from the primary instance, in milliseconds\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `AuroraReplicaLagMaximum`  |  The maximum amount of lag between the primary instance and each Aurora DB instance in the DB cluster, in milliseconds\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `AuroraReplicaLagMinimum`  |  The minimum amount of lag between the primary instance and each Aurora DB instance in the DB cluster, in milliseconds\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `BinLogDiskUsage`  |  The amount of disk space occupied by binary logs on the master, in bytes\.  |  Aurora MySQL  | 
|  `BacktrackChangeRecordsCreationRate`  |  The number of backtrack change records created over five minutes for your DB cluster\.  |  Aurora MySQL  | 
|  `BacktrackChangeRecordsStored`  |  The actual number of backtrack change records used by your DB cluster\.  |  Aurora MySQL  | 
|  `BacktrackWindowActual`  |  The difference between the target backtrack window and the actual backtrack window\.  |  Aurora MySQL  | 
|  `BacktrackWindowAlert`  |  The number of times that the actual backtrack window is smaller than the target backtrack window for a given period of time\.  |  Aurora MySQL  | 
|  `BlockedTransactions`  | The average number of transactions in the database that are blocked per second\.  | Aurora MySQL | 
|  `BufferCacheHitRatio`  |  The percentage of requests that are served by the buffer cache\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `CommitLatency`  | The amount of latency for commit operations, in milliseconds\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `CommitThroughput`  | The average number of commit operations per second\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `CPUCreditBalance`  |  The number of CPU credits that an instance has accumulated\. This metric applies only to `db.t2.small ` and `db.t2.medium ` instances\. It is used to determine how long an Aurora MySQL DB instance can burst beyond its baseline performance level at a given rate\.  CPU credit metrics are reported at 5\-minute intervals\.   | Aurora MySQL | 
|  `CPUCreditUsage`  |  The number of CPU credits consumed during the specified period\. This metric applies only to `db.t2.small` and `db.t2.medium` instances\. It identifies the amount of time during which physical CPUs have been used for processing instructions by virtual CPUs allocated to the Aurora MySQL DB instance\.   CPU credit metrics are reported at 5\-minute intervals\.   | Aurora MySQL | 
|  `CPUUtilization`  |  The percentage of CPU used by an Aurora DB instance\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `DatabaseConnections`  |  The number of connections to an Aurora DB instance\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `DDLLatency`  |  The amount of latency for data definition language \(DDL\) requests, in milliseconds—for example, create, alter, and drop requests\.  | Aurora MySQL | 
|  `DDLThroughput`  |  The average number of DDL requests per second\.  | Aurora MySQL | 
|  `Deadlocks`  | The average number of deadlocks in the database per second\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `DeleteLatency`  |  The amount of latency for delete queries, in milliseconds\.  | Aurora MySQL | 
|  `DeleteThroughput`  |  The average number of delete queries per second\.  | Aurora MySQL | 
|  `DiskQueueDepth`  |  The number of outstanding read/write requests waiting to access the disk\.  | Aurora PostgreSQL | 
|  `DMLLatency`  |  The amount of latency for inserts, updates, and deletes, in milliseconds\.  | Aurora MySQL | 
|  `DMLThroughput`  | The average number of inserts, updates, and deletes per second\.  | Aurora MySQL | 
|  `EngineUptime`  |  The amount of time that the instance has been running, in seconds\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `FreeableMemory`  |  The amount of available random access memory, in bytes\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `FreeLocalStorage`  |  The amount of storage available for temporary tables and logs, in bytes\. Unlike for other DB engines, for Aurora DB instances this metric reports the amount of storage available to each DB instance for temporary tables and logs\. This value depends on the DB instance class \(for pricing information, see the [Amazon RDS product page](http://aws.amazon.com/rds/#pricing)\)\. You can increase the amount of free storage space for an instance by choosing a larger DB instance class for your instance\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `InsertLatency`  |  The amount of latency for insert queries, in milliseconds\.  | Aurora MySQL | 
|  `InsertThroughput`  |  The average number of insert queries per second\.  | Aurora MySQL | 
|  `LoginFailures`  | The average number of failed login attempts per second\.  | Aurora MySQL | 
|  `MaximumUsedTransactionIDs`  | The age of the oldest unvacuumed transaction ID, in transactions\. If this value reaches 2,146,483,648 \(2^31 \- 1,000,000\), the database is forced into read\-only mode, to avoid transaction ID wraparound\. For more information, see [Preventing Transaction ID Wraparound Failures](https://www.postgresql.org/docs/current/static/routine-vacuuming.html#VACUUM-FOR-WRAPAROUND) in the PostgreSQL documentation\.  | Aurora PostgreSQL | 
|  `NetworkReceiveThroughput`  |  The amount of network throughput received from clients by each instance in the Aurora MySQL DB cluster, in bytes per second\. This throughput doesn't include network traffic between instances in the Aurora DB cluster and the cluster volume\.  | Aurora MySQL and Aurora PostgreSQL  | 
|  `NetworkThroughput`  |  The amount of network throughput both received from and transmitted to clients by each instance in the Aurora MySQL DB cluster, in bytes per second\. This throughput doesn't include network traffic between instances in the DB cluster and the cluster volume\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `NetworkTransmitThroughput`  | The amount of network throughput sent to clients by each instance in the Aurora DB cluster, in bytes per second\. This throughput doesn't include network traffic between instances in the DB cluster and the cluster volume\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `Queries`  | The average number of queries executed per second\. | Aurora MySQL | 
|  `RDSToAuroraPostgreSQLReplicaLag`  | The amount of lag in seconds when replicating updates from the primary RDS PostgreSQL instance to other nodes in the cluster\.  | Aurora PostgreSQL | 
|  `ReadIOPS`  |  The average number of disk I/O operations per second\. Aurora with PostgreSQL compatibility reports read and write IOPS separately, in 1\-minute intervals\.  | Aurora PostgreSQL | 
|  `ReadLatency`  |  The average amount of time taken per disk I/O operation\.  | Aurora PostgreSQL | 
|  `ReadThroughput`  |  The average number of bytes read from disk per second\.  | Aurora PostgreSQL | 
|  `ResultSetCacheHitRatio`  |  The percentage of requests that are served by the Resultset cache\.  | Aurora MySQL | 
|  `SelectLatency`  |  The amount of latency for select queries, in milliseconds\.  | Aurora MySQL | 
|  `SelectThroughput`  |  The average number of select queries per second\.  | Aurora MySQL | 
|  `SwapUsage`  |  The amount of swap space used on the Aurora PostgreSQL DB instance\.  | Aurora PostgreSQL | 
|  `TransactionLogsDiskUsage`  |  The amount of disk space occupied by transaction logs on the Aurora PostgreSQL DB instance\.  | Aurora PostgreSQL | 
|  `UpdateLatency`  |  The amount of latency for update queries, in milliseconds\.  | Aurora MySQL | 
|  `UpdateThroughput`  |  The average number of update queries per second\.  | Aurora MySQL | 
| `VolumeBytesUsed` |  The amount of storage used by your Aurora DB instance, in bytes\. This value affects the cost of the Aurora DB cluster \(for pricing information, see the [Amazon RDS product page](http://aws.amazon.com/rds/#pricing)\)\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `VolumeReadIOPs`  |  The number of billed read I/O operations from a cluster volume, reported at 5\-minute intervals\. Billed read operations are calculated at the cluster volume level, aggregated from all instances in the Aurora DB cluster, and then reported at 5\-minute intervals\. The value is calculated by taking the value of the **Read operations** metric over a 5\-minute period\. You can determine the amount of billed read operations per second by taking the value of the **Billed read operations** metric and dividing by 300 seconds\. For example, if the **Billed read operations** returns 13,686, then the billed read operations per second is 45 \(13,686 / 300 = 45\.62\)\.  You accrue billed read operations for queries that request database pages that aren't in the buffer cache and therefore must be loaded from storage\. You might see spikes in billed read operations as query results are read from storage and then loaded into the buffer cache\.   | Aurora MySQL and Aurora PostgreSQL | 
|  `VolumeWriteIOPs`  |  The number of write disk I/O operations to the cluster volume, reported at 5\-minute intervals\. See the description of VolumeReadIOPS above for a detailed description of how billed write operations are calculated\.  | Aurora MySQL and Aurora PostgreSQL | 
|  `WriteIOPS`  |  The average number of disk I/O operations per second\.  Aurora PostgreSQL reports read and write IOPS separately, on 1\-minute intervals\.  | Aurora PostgreSQL | 
|  `WriteLatency`  |  The average amount of time taken per disk I/O operation\.  | Aurora PostgreSQL | 
|  `WriteThroughput`  |  The average number of bytes written to disk per second\.  | Aurora PostgreSQL | 

## Amazon RDS Performance Insights Metrics<a name="performance-insights-metrics"></a>

The `AWS/RDS` namespace includes the following metrics that apply to database entities running Amazon RDS Performance Insights\.


| Metric | Description | 
| --- | --- | 
|  `DBLoad`  |  The average number of active sessions for the DB engine\.  | 
|  `DBLoadCPU`  |  The number of active sessions where the wait event type is CPU\.  | 
|  `DBLoadNonCPU`  |  The number of active sessions where the wait event type is not CPU\.  | 

## Dimensions for RDS Metrics<a name="rds-metric-dimensions"></a>

Amazon RDS data can be filtered along any of the following dimensions in the table below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  DBInstanceIdentifier  |  This dimension filters the data you request for a specific database instance\.  | 
|  DBClusterIdentifier  |  This dimension filters the data you request for a specific Amazon Aurora DB cluster\.  | 
|  DBClusterIdentifier, Role  |  This dimension filters the data you request for a specific Amazon Aurora DB cluster, aggregating the metric by instance role \(WRITER/READER\)\. For example, you can aggregate metrics for all READER instances that belong to a cluster\.  | 
|  DatabaseClass  |  This dimension filters the data you request for all instances in a database class\. For example, you can aggregate metrics for all instances that belong to the database class `db.m1.small`  | 
|  EngineName  |  This dimension filters the data you request for the identified engine name only\. For example, you can aggregate metrics for all instances that have the engine name `mysql`\.  | 
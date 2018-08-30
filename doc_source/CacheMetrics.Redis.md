# Metrics for Redis<a name="CacheMetrics.Redis"></a>

The `AWS/ElastiCache` namespace includes the following Redis metrics\.

With the exception of `ReplicationLag`, these metrics are derived from the Redis info command\. Each metric is calculated at the cache node level\.

For complete documentation of the Redis info command, go to [ http://redis\.io/commands/info](http://redis.io/commands/info)\. 

**See Also**
+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| ActiveDefragHits | The number of value reallocations per minute performed by the active defragmentation process\. | Number | 
| BytesUsedForCache | The total number of bytes allocated by Redis\. | Bytes | 
| CacheHits | The number of successful key lookups\. | Count | 
| CacheMisses | The number of unsuccessful key lookups\. | Count | 
| CurrConnections | The number of client connections, excluding connections from read replicas\. ElastiCache uses two to three of the connections to monitor the cluster in each case\. | Count | 
| EngineCPUUtilization | `EngineCPUUtilization` provides access to the Redis process CPU utilization to gain better insights into your Redis workloads\. As Redis is single threaded and uses just one CPU core at any given point in time, `EngineCPUUtilization` provides more precise visibility into the load of the Redis process itself\. `EngineCPUUtilization` adds to the pre\-existing `CPUUtilization` metric which exposes CPU utilization for the server instance as a whole including other operating system and management processes\. We recommend that you use both `EngineCPUUtilization` and `CPUUtilization` metrics together to get a detailed understanding of CPU utilization for your Redis environment\.  | Percent | 
| Evictions | The number of keys that have been evicted due to the maxmemory limit\. | Count | 
| HyperLogLogBasedCmds | The total number of HyperLogLog based commands\. This is derived from the Redis commandstats statistic by summing all of the pf type of commands \(pfadd, pfcount, pfmerge\)\. | Count | 
| NewConnections | The total number of connections that have been accepted by the server during this period\. | Count | 
| Reclaimed | The total number of key expiration events\. | Count | 
| ReplicationBytes | For primaries with attached replicas, ReplicationBytes reports the number of bytes that the primary is sending to all of its replicas\. This metric is representative of the write load on the replication group\. For replicas and standalone primaries, ReplicationBytes is always 0\.  | Bytes | 
| ReplicationLag | This metric is only applicable for a node running as a read replica\. It represents how far behind, in seconds, the replica is in applying changes from the primary node\. | Seconds | 
| SaveInProgress | This binary metric returns 1 whenever a background save \(forked or forkless\) is in progress, and 0 otherwise\. A background save process is typically used during snapshots and syncs\. These operations can cause degraded performance\. Using the  SaveInProgress metric, you can diagnose whether or not degraded performance was caused by a background save process\. | Count | 

**EngineCPUUtilization availability**  
Nodes in a region created or replaced after the date and time specified in the following table will include the `EngineCPUUtilization` metric\.


| Region | Region name |  EngineCPUUtilization availability  | 
| --- | --- | --- | 
| us\-east\-2 | US East \(Ohio\) | February 16, 2017 | 17:21 \(UTC\) | 
| us\-east\-1 | US East \(N\. Virginia\) | February 8, 2017 | 21:20 \(UTC\) | 
| us\-west\-1 | US West \(N\. California\) | February 14, 2017 | 22:23 \(UTC\) | 
| us\-west\-2 | US West \(Oregon\) | Febrary 20, 2017 | 19:20 \(UTC\) | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) | February 14, 2017 | 19:58 \(UTC\) | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) | Available on all nodes\. | 
| ap\-northeast\-3 | Asia Pacific \(Osaka\-Local\) | Available on all nodes\. | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) | February 7, 2017 | 02:51 \(UTC\) | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) | February 13, 2017 | 23:40 \(UTC\) | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) | February 14, 2017 | 03:33 \(UTC\) | 
| ca\-central\-1 | Canada \(Central\) | Available on all nodes\. | 
| cn\-north\-1 | China \(Beijing\) | February 16, 2017 | 22:39 \(UTC\) | 
| cn\-northwest\-2 | China \(Ningxia\) | Available on all nodes\. | 
| eu\-central\-1 | EU \(Frankfurt\) | February 15, 2017 | 00:46 \(UTC\) | 
| eu\-west\-1 | EU \(Ireland\) | February 7, 2017 | 21:30 \(UTC\) | 
| eu\-west\-2 | EU \(London\) | February 16, 2017 | 18:58 \(UTC\) | 
| eu\-west\-3 | EU \(Paris\) | Available on all nodes\. | 
| sa\-east\-1 | South America \(São Paulo\) | February 7, 2017 | 04:35 \(UTC\) | 
| us\-gov\-west\-1 | AWS GovCloud \(US\) | February 16, 2017 | 20:11 \(UTC\) | 

These are aggregations of certain kinds of commands, derived from info commandstats:


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| CurrItems | The number of items in the cache\. This is derived from the Redis keyspace statistic, summing all of the keys in the entire keyspace\. | Count | 
| GetTypeCmds | The total number of get types of commands\. This is derived from the Redis commandstats statistic by summing all of the get types of commands \(get, mget, hget, etc\.\) | Count | 
| HashBasedCmds | The total number of commands that are hash\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more hashes\. | Count | 
| KeyBasedCmds | The total number of commands that are key\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more keys\. | Count | 
| ListBasedCmds | The total number of commands that are list\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more lists\. | Count | 
| SetBasedCmds | The total number of commands that are set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sets\. | Count | 
| SetTypeCmds | The total number of set types of commands\. This is derived from the Redis commandstats statistic by summing all of the set types of commands \(set, hset, etc\.\) | Count | 
| SortedSetBasedCmds | The total number of commands that are sorted set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sorted sets\. | Count | 
| StringBasedCmds | The total number of commands that are string\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more strings\. | Count | 
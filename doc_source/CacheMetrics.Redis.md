# Metrics for Redis<a name="CacheMetrics.Redis"></a>

The `AWS/ElastiCache` namespace includes the following Redis metrics\.

With the exception of `ReplicationLag`, these metrics are derived from the Redis info command\. Each metric is calculated at the cache node level\.

For complete documentation of the Redis info command, go to [ http://redis\.io/commands/info](http://redis.io/commands/info)\. 

**See Also**

+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| BytesUsedForCache | The total number of bytes allocated by Redis\. | Bytes | 
| CacheHits | The number of successful key lookups\. | Count | 
| CacheMisses | The number of unsuccessful key lookups\. | Count | 
| CurrConnections | The number of client connections, excluding connections from read replicas\. ElastiCache uses two to three of the connections to monitor the cluster in each case\. | Count | 
| EngineCPUUtilization | CPU utilization of the single core that Redis is running on\. Since Redis is single threaded, this is the percent of your thread's capacity that is being used\. This metric is available on clusters created or replaced after November 1, 2017\. | Percent | 
| Evictions | The number of keys that have been evicted due to the maxmemory limit\. | Count | 
| HyperLogLogBasedCmds | The total number of HyperLogLog based commands\. This is derived from the Redis commandstats statistic by summing all of the pf type of commands \(pfadd, pfcount, pfmerge\)\. | Count | 
| NewConnections | The total number of connections that have been accepted by the server during this period\. | Count | 
| Reclaimed | The total number of key expiration events\. | Count | 
| ReplicationBytes | For primaries with attached replicas, ReplicationBytes reports the number of bytes that the primary is sending to all of its replicas\. This metric is representative of the write load on the replication group\. For replicas and standalone primaries, ReplicationBytes is always 0\.  | Bytes | 
| ReplicationLag | This metric is only applicable for a node running as a read replica\. It represents how far behind, in seconds, the replica is in applying changes from the primary node\. | Seconds | 
| SaveInProgress | This binary metric returns 1 whenever a background save \(forked or forkless\) is in progress, and 0 otherwise\. A background save process is typically used during snapshots and syncs\. These operations can cause degraded performance\. Using the  SaveInProgress metric, you can diagnose whether or not degraded performance was caused by a background save process\. | Count | 

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
# Metrics for Memcached<a name="CacheMetrics.Memcached"></a>

The `AWS/ElastiCache` namespace includes the following metrics that are derived from the Memcached stats command\. Each metric is calculated at the cache node level\. 

For complete documentation of the Memcached stats command, go to [ https://github\.com/memcached/memcached/blob/master/doc/protocol\.txt](https://github.com/memcached/memcached/blob/master/doc/protocol.txt)\. 

**See Also**

+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| BytesReadIntoMemcached |  The number of bytes that have been read from the network by the cache node\.  |  Bytes  | 
| BytesUsedForCacheItems  |  The number of bytes used to store cache items\.  |  Bytes  | 
| BytesWrittenOutFromMemcached  |  The number of bytes that have been written to the network by the cache node\.  |  Bytes  | 
| CasBadval |  The number of CAS \(check and set\) requests the cache has received where the Cas value did not match the Cas value stored\.  |  Count  | 
| CasHits |  The number of Cas requests the cache has received where the requested key was found and the Cas value matched\.  |  Count  | 
| CasMisses |  The number of Cas requests the cache has received where the key requested was not found\.  |  Count  | 
| CmdFlush |  The number of flush commands the cache has received\.  |  Count  | 
| CmdGet |  The number of get commands the cache has received\.  |  Count  | 
| CmdSet |  The number of set commands the cache has received\.  |  Count  | 
| CurrConnections |  A count of the number of connections connected to the cache at an instant in time\. ElastiCache uses two to three of the connections to monitor the cluster in each case\. |  Count  | 
| CurrItems |  A count of the number of items currently stored in the cache\.  |  Count  | 
| DecrHits |  The number of decrement requests the cache has received where the requested key was found\.  |  Count  | 
| DecrMisses |  The number of decrement requests the cache has received where the requested key was not found\.  |  Count  | 
| DeleteHits |  The number of delete requests the cache has received where the requested key was found\.  |  Count  | 
| DeleteMisses |  The number of delete requests the cache has received where the requested key was not found\.  |  Count  | 
| Evictions |  The number of non\-expired items the cache evicted to allow space for new writes\.  |  Count  | 
| GetHits |  The number of get requests the cache has received where the key requested was found\.  |  Count  | 
| GetMisses |  The number of get requests the cache has received where the key requested was not found\.  |  Count  | 
| IncrHits |  The number of increment requests the cache has received where the key requested was found\.  |  Count  | 
| IncrMisses |  The number of increment requests the cache has received where the key requested was not found\.  |  Count  | 
| Reclaimed |  The number of expired items the cache evicted to allow space for new writes\.  |  Count  | 

For Memcached 1\.4\.14, the following additional metrics are provided\.


| Metric | Description | Unit | 
| --- | --- | --- | 
| BytesUsedForHash |  The number of bytes currently used by hash tables\.  |  Bytes  | 
| CmdConfigGet |  The cumulative number of config get requests\.  |  Count  | 
| CmdConfigSet |  The cumulative number of config set requests\.  |  Count  | 
| CmdTouch |  The cumulative number of touch requests\.  |  Count  | 
| CurrConfig |  The current number of configurations stored\.  |  Count  | 
| EvictedUnfetched |  The number of valid items evicted from the least recently used cache \(LRU\) which were never touched after being set\.  |  Count  | 
| ExpiredUnfetched |  The number of expired items reclaimed from the LRU which were never touched after being set\.  |  Count  | 
| SlabsMoved |  The total number of slab pages that have been moved\.  |  Count  | 
| TouchHits |  The number of keys that have been touched and were given a new expiration time\.  |  Count  | 
| TouchMisses |  The number of items that have been touched, but were not found\.  |  Count  | 

The `AWS/ElastiCache` namespace includes the following calculated cache\-level metrics\.


| Metric | Description | Unit | 
| --- | --- | --- | 
| NewConnections |  The number of new connections the cache has received\. This is derived from the memcached total\_connections statistic by recording the change in total\_connections across a period of time\. This will always be at least 1, due to a connection reserved for a ElastiCache\.  |  Count  | 
| NewItems |  The number of new items the cache has stored\. This is derived from the memcached total\_items statistic by recording the change in total\_items across a period of time\.  |  Count  | 
| UnusedMemory | The amount of memory not used by data\. This is derived from the Memcached statistics `limit_maxbytes` and `bytes` by subtracting `bytes` from `limit_maxbytes`\. Because Memcached overhead uses memory in addition to that used by data, `UnusedMemory` should not be considered to be the amount of memory available for additional data\. You may experience evictions even though you still have some unused memory\. For more detailed information, see [Memcached item memory usage](http://www.deplication.net/2016/02/memcached-item-memory-usage.html)\.  |  Bytes  | 
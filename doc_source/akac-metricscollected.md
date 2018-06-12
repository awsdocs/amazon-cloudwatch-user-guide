# Amazon Kinesis Video Streams Metrics and Dimensions<a name="akac-metricscollected"></a>

## Metrics<a name="kinesis-video-metrics"></a>

The `AWS/KinesisVideo` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `PutMedia.Requests` |  Number of PutMedia API requests for a given stream\. Units: Count  | 
|  `PutMedia.IncomingBytes` |  Number of bytes received as part of PutMedia for the stream\. Units: Bytes  | 
|  `PutMedia.IncomingFragments` |  Number of complete fragments received as part of PutMedia for the stream\. Units: Count  | 
|  `PutMedia.IncomingFrames` |  Number of complete frames received as part of PutMedia for the stream\. Units: Count  | 
|  `PutMedia.ActiveConnections` |  The total number of connections to the service host\. Units: Count  | 
|  `PutMedia.ConnectionErrors` |  Errors while establishing PutMedia connection for the stream\. Units: Count  | 
|  `PutMedia.FragmentIngestionLatency` |  Time difference between when the first and last bytes of a fragment are received by Kinesis Video Streams\. Units: Milliseconds  | 
|  `PutMedia.FragmentPersistLatency` |  Time taken from when the complete fragment data is received and archived\. Units: Count  | 
|  `PutMedia.Latency` |  Time difference between the request and the HTTP response from InletService while establishing the connection Units: Count  | 
|  `PutMedia.BufferingAckLatency` |  Time difference between when the first byte of a new fragment is received by Kinesis Video Streams and when the Buffering Ack is sent for the fragment Units: Milliseconds  | 
|  `PutMedia.ReceivedAckLatency` |  Time difference between when the last byte of a new fragment is received by Kinesis Video Streams and when the Received ACK is sent for the fragment\. Units: Milliseconds  | 
|  `PutMedia.PersistedAckLatency` |  Time difference between when the last byte of a new fragment is received by Kinesis Video Streams and when the Persisted ACK is sent for the fragment\. Units: Milliseconds  | 
|  `PutMedia.ErrorAckCount` |  Number of Error ACKs sent while doing PutMedia for the stream\. Units: Count  | 
|  `PutMedia.Success` |  1 for each fragment successfully written; 0 for every failed fragment\. The average value of this metric indicates how many complete, valid fragments are sent\. Units: Count  | 
|  `GetMedia.Requests` |  Number of GetMedia API requests for a given stream\. Units: Count  | 
|  `GetMedia.OutgoingBytes` |  Total number of bytes sent out from the service as part of the GetMedia API for a given stream\. Units: Bytes  | 
|  `GetMedia.OutgoingFragments` |  Number of fragments sent while doing GetMedia for the stream\. Units: Count  | 
|  `GetMedia.OutgoingFrames` |  Number of frames sent during GetMedia on the given stream\. Units: Count  | 
|  `GetMedia.MillisBehindNow` |  Time difference between the current server time stamp and the server time stamp of the last fragment sent\. Units: Milliseconds  | 
|  `GetMedia.ConnectionErrors` |  The number of connections that were not successfully established\. Units: Count  | 
|  `GetMedia.Success` |  1 for every fragment successfully sent; 0 for every failure\. The average value indicates the rate of success\. Units: Count  | 
|  `GetMediaForFragmentList.OutgoingBytes` |  Total number of bytes sent out from the service as part of the GetMediaForFragmentList API for a given stream\. Units: Bytes  | 
|  `GetMediaForFragmentList.OutgoingFragments` |  Total number of fragments sent out from the service as part of the GetMediaForFragmentList API for a given stream\. Units: Count  | 
|  `GetMediaForFragmentList.OutgoingFrames` |  Total number of frames sent out from the service as part of the GetMediaForFragmentList API for a given stream\. Units: Count  | 
|  `GetMediaForFragmentList.Requests` |  Number of GetMediaForFragmentList API requests for a given stream\. Units: Count  | 
|  `GetMediaForFragmentList.Success` |  1 for every fragment successfully sent; 0 for every failure\. The average value indicates the rate of success\. Units: Count  | 
|  `ListFragments.Latency` |  Latency of the ListFragments API calls for the given stream name\. Units: Milliseconds  | 

## Dimensions for Amazon Kinesis Video Streams Metrics<a name="akac-metric-dimensions"></a>

You can use the following dimensions to filter the metrics for Amazon Kinesis Video Streams\.


| Dimension | Description | 
| --- | --- | 
|  `StreamName`  |  The name of the Kinesis video stream\.  | 
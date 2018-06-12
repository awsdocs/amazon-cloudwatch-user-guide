# Amazon WorkSpaces Metrics and Dimensions<a name="wsp-metricscollected"></a>

Amazon WorkSpaces sends data points to CloudWatch for several metrics every five minutes \(five\-minute metrics\)\. Detailed monitoring, or one\-minute metrics, is currently unavailable for Amazon WorkSpaces\. For more information, see [Monitoring Amazon WorkSpaces](http://docs.aws.amazon.com/workspaces/latest/adminguide/monitoring.html) in the *Amazon WorkSpaces Administration Guide*\.

## Amazon WorkSpaces Metrics<a name="wsp-metrics"></a>

The `AWS/WorkSpaces` namespace includes the following metrics\.


| Metric | Description | Dimensions | Statistics Available | Units | 
| --- | --- | --- | --- | --- | 
| Available1 |  The number of WorkSpaces that returned a healthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Unhealthy1 |  The number of WorkSpaces that returned an unhealthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionAttempt2 |  The number of connection attempts\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionSuccess2 |  The number of successful connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionFailure2 |  The number of failed connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| SessionLaunchTime2 | The amount of time it takes to initiate a WorkSpaces session\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Second \(time\) | 
| InSessionLatency2 | The round trip time between the WorkSpaces client and the WorkSpace\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Millisecond \(time\) | 
| SessionDisconnect2 | The number of connections that were closed, including user\-initiated and failed connections\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| UserConnected3 | The number of WorkSpaces that have a user connected\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Stopped | The number of WorkSpaces that are stopped\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Maintenance4 | The number of WorkSpaces that are under maintenance\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 

1 Amazon WorkSpaces periodically sends status requests to a WorkSpace\. A WorkSpace is marked `Available` when it responds to these requests, and `Unhealthy` when it fails to respond to these requests\. These metrics are available at a per\-WorkSpace granularity, and also aggregated for all WorkSpaces in an organization\. 

2 Amazon WorkSpaces records metrics on connections made to each WorkSpace\. These metrics are emitted after a user has successfully authenticated via the WorkSpaces client and the client then initiates a session\. The metrics are available at a per\-WorkSpace granularity, and also aggregated for all WorkSpaces in a directory\.

3 Amazon WorkSpaces periodically sends connection status requests to a WorkSpace\. Users are reported as connected when they are actively using their sessions\. This metric is available at a per\-WorkSpace granularity, and is also aggregated for all WorkSpaces in an organization\.

4 This metric applies to WorkSpaces that are configured with an AutoStop running mode\. If you have maintenance enabled for your WorkSpaces, this metric captures the number of WorkSpaces that are currently under maintenance\. This metric is available at a per\-WorkSpace granularity, which describes when a WorkSpace went into maintenance and when it was removed\.

## Dimensions for Amazon WorkSpaces Metrics<a name="wsp-metric-dimensions"></a>

Amazon WorkSpaces metrics are available for the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| DirectoryId | Limits the data you receive to the WorkSpaces in the specified directory\. The DirectoryId value is in the form of d\-XXXXXXXXXX\. | 
| WorkspaceId | Limits the data you receive to the specified WorkSpace\. The WorkspaceId value is in the formws\-XXXXXXXXXX\. | 
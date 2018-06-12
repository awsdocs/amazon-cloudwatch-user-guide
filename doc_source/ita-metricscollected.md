# AWS IoT Analytics Metrics and Dimensions<a name="ita-metricscollected"></a>

AWS IoT Analytics sends the following metrics to CloudWatch\.

## AWS IoT Analytics Metrics<a name="aws-iot-analytics-metrics"></a>

AWS IoT Analytics sends the following metrics to CloudWatch once per received request\.


**AWS IoT Analytics Metrics**  

| Metric | Description | 
| --- | --- | 
|  ActionExecution  |  The number of actions executed\.  | 
|  ActivityExecutionError  |  The number of errors generated while executing the pipeline activity\.  | 
|  IncomingMessages  |  The number of messages coming into the channel\.  | 

## Dimensions for Metrics<a name="aws-iot-analytics-metricdimensions"></a>

Metrics use the namespace and provide metrics for the following dimension\(s\):


| Dimension | Description | 
| --- | --- | 
| ActionType |  The type of action that is being monitored\.  | 
| ChannelName |  The name of the channel that is being monitored\.  | 
| DatasetName |  The name of the data set that is being monitored\.  | 
| DatastoreName |  The name of the data store that is being monitored\.  | 
| PipelineActivityName |  The name of the pipeline activity that is being monitored\.  | 
| PipelineActivityType |  The type of the pipeline activity that is being monitored\.  | 
| PipelineName |  The name of the pipeline that is being monitored\.  | 
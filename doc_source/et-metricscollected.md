# Amazon Elastic Transcoder Metrics and Dimensions<a name="et-metricscollected"></a>

When you interact with Amazon Elastic Transcoder, it sends the following metrics to CloudWatch every minute\.

## Elastic Transcoder Metrics<a name="ET-metrics"></a>

The `AWS/ElasticTranscoder` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `Billed HD Output` |  The number of billable seconds of HD output for a pipeline\. Valid Dimensions: PipelineId Unit: Seconds  | 
|  `Billed SD Output` |  The number of billable seconds of SD output for a pipeline\. Valid Dimensions: PipelineId Unit: Seconds  | 
|  `Billed Audio Output` |  The number of billable seconds of audio output for a pipeline\. Valid Dimensions: PipelineId Unit: Seconds  | 
|  `Jobs Completed` |  The number of jobs completed by this pipeline\. Valid Dimensions: PipelineId Unit: Count  | 
|  `Jobs Errored` |  The number of jobs that failed because of invalid inputs, such as a request to transcode a file that is not in the given input bucket\. Valid Dimensions: PipelineId Unit: Count  | 
|  `Outputs per Job` |  The number of outputs Elastic Transcoder created for a job\. Valid Dimensions: PipelineId Unit: Count  | 
|  `Standby Time` |  The number of seconds before Elastic Transcoder started transcoding a job\. Valid Dimensions: PipelineId Unit: Seconds  | 
|  `Errors` |  The number of errors caused by invalid operation parameters, such as a request for a job status that does not include the job ID\. Valid Dimensions: Operation Unit: Count  | 
|  `Throttles` |  The number of times that Elastic Transcoder automatically throttled an operation\. Valid Dimensions: Operation Unit: Count  | 

## Dimensions for Elastic Transcoder Metrics<a name="ET-metricdimensions"></a>

Elastic Transcoder metrics use the Elastic Transcoder namespace and provide metrics for the following dimension\(s\):


| Dimension | Description | 
| --- | --- | 
|  `PipelineId`  |  The ID of a pipeline\. This dimension filters the data you request for an Elastic Transcoder pipeline\.  | 
|  `Operation`  |  This dimension filters the data you request for the APIs that Elastic Transcoder provides\.  | 
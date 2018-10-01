# Amazon Machine Learning Metrics and Dimensions<a name="ml-metricscollected"></a>

Amazon Machine Learning sends metrics to CloudWatch every five minutes\. For more information, see [Monitoring Amazon ML with Amazon CloudWatch Metrics](https://docs.aws.amazon.com/machine-learning/latest/dg/cw-doc.html) in the *Amazon Machine Learning Developer Guide*\.

## Amazon ML Metrics<a name="machinelearning-metrics"></a>

The `AWS/ML` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| PredictCount |  The number of observations received by Amazon ML, measured over the specified time period\.  Units: Count  | 
| PredictFailureCount |  The number of invalid or malformed observations received by Amazon ML, measured over the specified time period\. Units: Count  | 

## Dimensions for Amazon Machine Learning Metrics<a name="ml-metric-dimensions"></a>

Amazon ML data can be filtered along any of the following dimensions in the table below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  MLModelId  |  The identifier of an Amazon ML model\. All available statistics are filtered by `MLModelId`\.   | 
|  RequestMode  |  An indicator specifying whether observations were received as part of a batch prediction request or as real\-time predict requests\. All available statistics are filtered by `RequestMode`\.   | 
# Amazon SageMaker Metrics and Dimensions<a name="sagemaker-metricscollected"></a>

Amazon SageMaker sends metrics to CloudWatch\. For more information, see [Monitoring Amazon SageMaker with Amazon CloudWatch](http://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html) in the *Amazon SageMaker Developer Guide*\.

**Invocation Metrics**

The `AWS/SageMaker` namespace includes the following request metrics\.


| Metric | Description | 
| --- | --- | 
| ModelLatency |  The latency of the model's response, as viewed from Amazon SageMaker\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| Invocation4XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 4xx HTTP response code\. For each 4xx response, 1 is sent; otherwise, 0 is sent\. Units: Count Valid statistics: Average, Sum  | 
| Invocation5XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 5xx HTTP response code\. For each 5xx response, 1 is sent; otherwise, 0 is sent\. Units: Count Valid statistics: Average, Sum  | 
| Invocations |  The `number of InvokeEndpoint` requests sent to a model\.  To get the total number of requests to the endpoint variant, use the Sum statistic\. Units: Count Valid statistics: Sum, Sample Count  | 
| InvocationsPerInstance | The number of invocations sent to a model, normalized by `InstanceCount` in each ProductionVariant\. 1/`numberOfInstances` is sent as the value on each request, where `numberOfInstances` is the number of active instances for the ProductionVariant behind the endpoint at the time of the request\.Units: CountValid statistics: Sum | 

**Dimensions for Invocation Metrics**


| Dimension | Description | 
| --- | --- | 
| EndpointName, VariantName |  Filters invocation metrics for a ProductionVariant of the specified endpoint and variant\.  | 

**Instance Metrics**

The `/aws/sagemaker/Endpoints and /aws/sagemaker/TrainingJobs` namespaces include the following metrics\.


| Metric | Description | 
| --- | --- | 
| CPUUtilization |  The percentage of CPU units that are used by the containers on an instance\. The value can range between 0 and 100, and is multiplied by the number of CPUs\. For example, if there are four CPUs, `CPUUtilization` can range from 0% to 400%\. For endpoint variants, the value is the sum of the CPU utilization of the primary and supplementary containers on the instance\. For training jobs, the value is the CPU utilization of the Algorithm container on the instance\. Units: Percent Valid statistics: Max  | 
| MemoryUtilizaton |  The percentage of memory that is used by the containers on an instance\. This value can range between 0% and 100%\. For endpoint variants, the value is the sum of the memory utilization of the primary and supplementary containers on the instance\. For training jobs, the value is the memory utilization of the Algorithm container on the instance\. Units: Percent Valid statistics: Max  | 
| GPUUtilization |  The percentage of GPU units that are used by the containers on an instance\. The value can range between 0 and 100 and is multiplied by the number of GPUs\. For example, if there are four GPUs, `GPUUtilization` can range from 0% to 400%\. For endpoint variants, the value is the sum of the GPU utilization of the primary and supplementary containers on the instance\. For training jobs, the value is the GPU utilization of the Algorithm container on the instance\. Units: Percent Valid statistics: Max  | 

**Dimensions for Host Metrics**


| Dimension | Description | 
| --- | --- | 
| Host |  For endpoints, the value for this dimension has the format `[endpoint-name]/[ production-variant-name ]/[instance-id]`\. Use this dimension to filter instance metrics for the specified endpoint, variant, and instance\. This dimension format is present only in the `/aws/sagemaker/Endpoints` namespace\. For training jobs, the value for this dimension has the format `[training-job-name]/algo-[instance-number-in-cluster]`\. Use this dimension to filter instance metrics for the specified training job and instance\. This dimension format is present only in the `/aws/sagemaker/TrainingJobs` namespace\.  | 
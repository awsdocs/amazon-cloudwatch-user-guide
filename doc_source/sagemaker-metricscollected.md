# Amazon SageMaker Metrics and Dimensions<a name="sagemaker-metricscollected"></a>

Amazon SageMaker sends metrics to CloudWatch\. For more information, see [Monitoring Amazon SageMaker with Amazon CloudWatch](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html) in the *Amazon SageMaker Developer Guide*\.

**Invocation Metrics**

The `AWS/SageMaker` namespace includes the following request metrics\.


| Metric | Description | 
| --- | --- | 
| Invocation4XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 4xx HTTP response code\. For each 4xx response, 1 is sent; otherwise, 0 is sent\. Units: Count Valid statistics: Average, Sum  | 
| Invocation5XXErrors |  The number of `InvokeEndpoint` requests where the model returned a 5xx HTTP response code\. For each 5xx response, 1 is sent; otherwise, 0 is sent\. Units: Count Valid statistics: Average, Sum  | 
| Invocations |  The `number of InvokeEndpoint` requests sent to a model\.  To get the total number of requests to the endpoint variant, use the Sum statistic\. Units: Count Valid statistics: Sum, Sample Count  | 
| InvocationsPerInstance | The number of invocations sent to a model, normalized by `InstanceCount` in each ProductionVariant\. 1/`numberOfInstances` is sent as the value on each request, where `numberOfInstances` is the number of active instances for the ProductionVariant behind the endpoint at the time of the request\.Units: CountValid statistics: Sum | 
| ModelLatency |  The interval of time taken by a model to respond as viewed from \. This interval includes the local communication times taken to send the request and to fetch the response from container of a model and the time taken to complete the inference in the container\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| OverheadLatency |  The interval of time added to the time taken to respond to a client request by overheads\. This interval is measured from the time receives the request until it returns a response to the client, minus the `ModelLatency`\. Overhead latency can vary depending on multiple factors, including request and response payload sizes, request frequency, and authentication/authorization of the request\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 

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
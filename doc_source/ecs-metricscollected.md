# Amazon ECS Metrics and Dimensions<a name="ecs-metricscollected"></a>

Amazon Elastic Container Service \(Amazon ECS\) sends metrics to Amazon CloudWatch\. For more information, see [Amazon ECS CloudWatch Metrics](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch-metrics.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Amazon ECS Metrics<a name="ecs-metrics"></a>

Amazon ECS provides metrics for you to monitor the CPU and memory reservation and utilization across your cluster as a whole, and the CPU and memory utilization on the services in your clusters\.

The metrics made available will depend on the launch type of the tasks and services in your clusters\. If you are using the Fargate launch type for your services then CPU and memory utilization metrics are provided to assist in the monitoring of your services\. For the Amazon EC2 launch type you will own and need to monitor the EC2 instances that make your underlying infrastructure so additional CPU and memory reservation and utilization metrics are made available at the cluster, service, and task level\.

Amazon ECS sends the following metrics to CloudWatch every minute\. When Amazon ECS collects metrics, it collects multiple data points every minute\. It then aggregates them to one data point before sending the data to CloudWatch\. So in CloudWatch, one sample count is actually the aggregate of multiple data points during one minute\.

The `AWS/ECS` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `CPUReservation`  |  The percentage of CPU units that are reserved by running tasks in the cluster\. Cluster CPU reservation \(this metric can only be filtered by `ClusterName`\) is measured as the total CPU units that are reserved by Amazon ECS tasks on the cluster, divided by the total CPU units that were registered for all of the container instances in the cluster\. This metric is only used for tasks using the EC2 launch type\. Valid Dimensions: `ClusterName` Valid Statistics: Average, Minimum, Maximum, Sum, Sample Count\. The most useful statistic is Average\. Unit: Percent  | 
|  `CPUUtilization`  |  The percentage of CPU units that are used in the cluster or service\. Cluster CPU utilization \(metrics that are filtered by `ClusterName` without `ServiceName`\) is measured as the total CPU units in use by Amazon ECS tasks on the cluster, divided by the total CPU units that were registered for all of the container instances in the cluster\. Cluster CPU utilization metrics are only used for tasks using the EC2 launch type\. Service CPU utilization \(metrics that are filtered by `ClusterName` and `ServiceName`\) is measured as the total CPU units in use by the tasks that belong to the service, divided by the total number of CPU units that are reserved for the tasks that belong to the service\. Service CPU utilization metrics are used for tasks using both the Fargate and the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Sample Count\. The most useful statistic is Average\. Unit: Percent  | 
|  `MemoryReservation`  |  The percentage of memory that is reserved by running tasks in the cluster\. Cluster memory reservation \(this metric can only be filtered by `ClusterName`\) is measured as the total memory that is reserved by Amazon ECS tasks on the cluster, divided by the total amount of memory that was registered for all of the container instances in the cluster\. This metric is only used for tasks using the EC2 launch type\. Valid Dimensions: `ClusterName` Valid Statistics: Average, Minimum, Maximum, Sum, Sample Count\. The most useful statistic is Average\. Unit: Percent  | 
|  `MemoryUtilization`  |  The percentage of memory that is used in the cluster or service\.  Cluster memory utilization \(metrics that are filtered by `ClusterName` without `ServiceName`\) is measured as the total memory in use by Amazon ECS tasks on the cluster, divided by the total amount of memory that was registered for all of the container instances in the cluster\. Cluster memory utilization metrics are only used for tasks using the EC2 launch type\. Service memory utilization \(metrics that are filtered by `ClusterName` and `ServiceName`\) is measured as the total memory in use by the tasks that belong to the service, divided by the total memory that is reserved for the tasks that belong to the service\. Service memory utilization metrics are used for tasks using both the Fargate and the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Sample Count\. The most useful statistic is Average\. Unit: Percent  | 

**Note**  
If you are using tasks with the EC2 launch type and have Linux container instances, the Amazon ECS container agent relies on Docker `stats` metrics to gather CPU and memory data for each container running on the instance\. If you are using an Amazon ECS agent prior to version 1\.14\.0, ECS includes filesystem cache usage when reporting memory utilization to CloudWatch so your CloudWatch graphs show a higher than actual memory utilization for tasks\. To remediate this, starting with Amazon ECS agent version 1\.14\.0, the Amazon ECS container agent excludes the filesystem cache usage from the memory utilization metric\. This change does not impact the out\-of\-memory behavior of containers\.

## Dimensions for Amazon ECS Metrics<a name="ecs-metrics-dimensions"></a>

Amazon ECS metrics use the `AWS/ECS` namespace and provide metrics for the following dimensions:


| Dimension | Description | 
| --- | --- | 
|  `ClusterName`  |  This dimension filters the data you request for all resources in a specified cluster\. All Amazon ECS metrics are filtered by `ClusterName`\.  | 
|  `ServiceName`  |  This dimension filters the data you request for all resources in a specified service within a specified cluster\.  | 
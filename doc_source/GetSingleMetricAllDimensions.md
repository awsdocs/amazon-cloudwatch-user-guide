# Aggregating Statistics Across Resources<a name="GetSingleMetricAllDimensions"></a>

You can aggregate the metrics for AWS resources across multiple resources\. Metrics are completely separate between Regions but you can use CloudWatch metric math to aggregate and transform metrics from multiple accounts and Regions\.

For example, you can aggregate statistics for your EC2 instances that have detailed monitoring enabled\. Instances that use basic monitoring aren't included\. Therefore, you must enable detailed monitoring \(at an additional charge\), which provides data in 1\-minute periods\. For more information, see [Enable or Disable Detailed Monitoring for Your Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html) in the *Amazon EC2 User Guide for Linux Instances*\.

This example shows you how to get the average CPU usage for your EC2 instances\. Because no dimension is specified, CloudWatch returns statistics for all dimensions in the `AWS/EC2` namespace\. To get statistics for other metrics, see [AWS Services That Publish CloudWatch Metrics](aws-services-cloudwatch-metrics.md)\.

**Important**  
This technique for retrieving all dimensions across an AWS namespace doesn't work for custom namespaces that you publish to CloudWatch\. With custom namespaces, you must specify the complete set of dimensions that are associated with any given data point to retrieve statistics that include the data point\. 

**To display average CPU utilization for your EC2 instances**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose the **EC2** namespace and choose **Across All Instances**\.

1. Select the row that contains `CPUUtilization`, which displays a graph for the metric for all your EC2 instances\. To change the name of the graph, choose the pencil icon\. To change the time range, select one of the predefined values or choose **custom**\.  
![\[Metrics aggregated across your EC2 instances\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_aggregated_instances.png)

1. To change the statistic, choose the **Graphed metrics** tab\. Choose the column heading or an individual value and then choose one of the statistics or predefined percentiles, or specify a custom percentile \(for example, **p95\.45**\)\.

1. To change the period, choose the **Graphed metrics** tab\. Choose the column heading or an individual value and then choose a different value\.

**To get average CPU utilization across your EC2 instances using the AWS CLI**  
Use the [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-staticstics.html) command as follows:

```
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --statistics "Average" "SampleCount" \
--start-time 2016-10-11T23:18:00 --end-time 2016-10-12T23:18:00 --period 3600
```

The following is example output:

```
{
    "Datapoints": [
        {
            "SampleCount": 238.0, 
            "Timestamp": "2016-10-12T07:18:00Z", 
            "Average": 0.038235294117647062, 
            "Unit": "Percent"
        }, 
        {
            "SampleCount": 240.0, 
            "Timestamp": "2016-10-12T09:18:00Z", 
            "Average": 0.16670833333333332, 
            "Unit": "Percent"
        }, 
        {
            "SampleCount": 238.0, 
            "Timestamp": "2016-10-11T23:18:00Z", 
            "Average": 0.041596638655462197, 
            "Unit": "Percent"
        }, 
        ...
    ], 
    "Label": "CPUUtilization"
}
```

# View Available Metrics<a name="viewing_metrics_with_cloudwatch"></a>

Metrics are grouped first by namespace, and then by the various dimension combinations within each namespace\. For example, you can view all EC2 metrics, EC2 metrics grouped by instance, or EC2 metrics grouped by Auto Scaling group\.

Only the AWS services that you're using send metrics to Amazon CloudWatch\.

**To view available metrics by namespace and dimension using the console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select a metric namespace \(for example, EC2\)\.  
![\[View the metrics namespaces\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_view_categories.png)

1. Select a metric dimension \(for example, Per\-Instance Metrics\)\.  
![\[View the metric dimensions for Amazon EC2\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_view_metric_category.png)

1. The **All metrics** tab displays all metrics for that dimension in the namespace\. You can do the following:

   1. To sort the table, use the column heading\.

   1. To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To filter by resource, choose the resource ID and then choose **Add to search**\.

   1. To filter by metric, choose the metric name and then choose **Add to search**\.  
![\[View the metrics for Amazon EC2\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_view_metrics.png)

**To view available metrics by namespace, dimension, or metric using the AWS CLI**  
Use the [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command to list CloudWatch metrics\. For a list of all service namespaces, see [AWS Namespaces](aws-namespaces.md)\. For lists of the metrics and dimensions for each service, see [Amazon CloudWatch Metrics and Dimensions Reference](CW_Support_For_AWS.md)\.

The following example specifies the `AWS/EC2` namespace to view all the metrics for Amazon EC2:

```
aws cloudwatch list-metrics --namespace AWS/EC2
```

The following is example output:

```
{
  "Metrics" : [
    ...
    {
        "Namespace": "AWS/EC2",
        "Dimensions": [
            {
                "Name": "InstanceId",
                "Value": "i-1234567890abcdef0"
            }
        ],
        "MetricName": "NetworkOut"
    },
    {
        "Namespace": "AWS/EC2",
        "Dimensions": [
            {
                "Name": "InstanceId",
                "Value": "i-1234567890abcdef0"
            }
        ],
        "MetricName": "CPUUtilization"
    },
    {
        "Namespace": "AWS/EC2",
        "Dimensions": [
            {
                "Name": "InstanceId",
                "Value": "i-1234567890abcdef0"
            }
        ],
        "MetricName": "NetworkIn"
    },
    ...
  ]
}
```

**To list all the available metrics for a specified resource**  
The following example specifies the `AWS/EC2` namespace and the `InstanceId` dimension to view the results for the specified instance only\.

```
aws cloudwatch list-metrics --namespace AWS/EC2 --dimensions Name=InstanceId,Value=i-1234567890abcdef0
```

**To list a metric for all resources**  
The following example specifies the `AWS/EC2` namespace and a metric name to view the results for the specified metric only\.

```
aws cloudwatch list-metrics --namespace AWS/EC2 --metric-name CPUUtilization
```
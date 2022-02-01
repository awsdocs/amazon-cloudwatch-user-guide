# Modifying the y\-axis for a graph<a name="switch_graph_axes"></a>

You can set custom bounds for the y\-axis on a graph to help you see the data better\. For example, you can change the bounds on a `CPUUtilization` graph to 100 percent so that it's easy to see whether the CPU is low \(the plotted line is near the bottom of the graph\) or high \(the plotted line is near the top of the graph\)\.

You can switch between two different y\-axes for your graph\. This is useful if the graph contains metrics that have different units or that differ greatly in their range of values\.

**To modify the y\-axis on a graph**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select a metric namespace \(for example, **EC2**\) and then a metric dimension \(for example, **Per\-Instance Metrics**\)\.

1. The **All metrics** tab displays all metrics for that dimension in that namespace\. To graph a metric, select the check box next to the metric\.

1. On the **Graph options** tab, specify the **Min** and **Max** values for **Left Y Axis**\. The value of **Min** can't be greater than the value of **Max**\.  
![\[Set custom bounds for the y-axis\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_graph_custom_bounds.png)

1. To create a second y\-axis, specify the **Min** and **Max** values for **Right Y Axis**\.

1. To switch between the two y\-axes, choose the **Graphed metrics** tab\. For **Y Axis**, choose **Left Y Axis** or **Right Y Axis**\.  
![\[Switch between the y-axes for a graph\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_graph_switch_axis.png)
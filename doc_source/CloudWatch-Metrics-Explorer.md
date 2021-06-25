# Use Metrics Explorer to Monitor Resources by Their Tags and Properties<a name="CloudWatch-Metrics-Explorer"></a>

Metrics explorer is a tag\-based tool that enables you to filter, aggregate, and visualize your metrics by tags and resource properties, to enhance observability for your services\. This gives you a flexible and dynamic troubleshooting experience, so that you to create multiple graphs at a time and use these graphs to build your application health dashboards\.

Metrics explorer visualizations are dynamic, so if a matching resource is created after you create a metrics explorer widget and add it to a CloudWatch dashboard, the new resource automatically appears in the explorer widget\.

For example, if all of your EC2 production instances have the **production** tag, you can use metrics explorer to filter and aggregate metrics from all of these instances to understand their health and performance\. If a new instance with a matching tag is later created, it's automatically added to the metrics explorer widget\. 

With metrics explorer, you can choose how to aggregate metrics from the resources that match the criteria, and whether to show them all in a single graph or on different graphs within one metrics explorer widget\.

Metrics explorer includes templates that you can use to see useful visualization graphs with one click, and you can also extend these templates to create completely customized metrics explorer widgets\.

Metrics explorer supports metrics emitted by AWS and EC2 metrics that are published by the CloudWatch agent, including memory, disk, and CPU metrics\. To use metrics explorer to see the metrics that are published by the CloudWatch agent, you might have to update your CloudWatch agent configuration file\. For more information, see [CloudWatch agent configuration for metrics explorer](#CloudWatch-Metrics-Explorer-agent)

To create a visualization with metrics explorer and optionally add it to a dashboard, follow these steps\.

**To create a visualization with metrics explorer**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Explorer**\.

1. Do one of the following:
   + To use a template, select it in the box that currently shows **Empty Explorer**\.

     Depending on the template, the explorer might immediately display graphs of metrics\. If it doesn't, choose one or more tags or properties in the **From** box and then data should appear\. If it doesn't, use the options at the top of the page to display a longer time range in the graphs\.
   + To create a custom visualization, under **Metrics**, choose a single metric or all the available metrics from a service\.

     After you choose a metric, you can optionally repeat this step to add more metrics\.

1. For each metric selected, CloudWatch displays the statistic that it will use immediately after the metric name\. To change this, choose the statistic name, and then choose the statistic that you want\.

1. Under **From**, choose a tag or a resource property to filter your results\.

   After you do this, you can optionally repeat this step to choose more tags or resource properties\.

   If you choose multiple values of the same property, such as two EC2 instance types, the explorer displays all the resources that match either chosen property\. It's treated as an OR operation\.

   If you choose different properties or tags, such as the **Production** tag and the M5 instance type, only the resources that match all of these selections are displayed\. It's treated as an AND operation\.

1. \(Optional\) For **Aggregate by**, choose a statistic to use to aggregate the metrics\. Then, next to **for**, choose how to aggregate the metric from the list\. You can aggregate together all the resources that are currently displayed, or aggregate by a single tag or resource property\.

   Depending on how you choose to aggregate, the result may be a single time series or multiple time series\. 

1. Under **Split by**, you can choose to split a single graph with multiple time series into multiple graphs\. The split can be made by a variety of criteria, which you choose under **Split by**\.

1. Under **Graph options**, you can refine the graph by changing the period, the type of graph, the legend placement, and the layout\.

1. To add this visualization as a widget to a CloudWatch dashboard, choose **Add to dashboard**\.

## CloudWatch agent configuration for metrics explorer<a name="CloudWatch-Metrics-Explorer-agent"></a>

To enable metrics explorer to discover EC2 metrics published by the CloudWatch agent, make sure that the CloudWatch agent configuration file contains the following values:
+ In the `metrics` section, make sure that the `aggregation_dimensions` parameter includes `[InstanceId"]`\. It can also contain other dimensions\.
+ In the `metrics` section, make sure that the `append_dimensions` parameter includes a `{InstanceId":"${aws:InstanceId}"}` line\. It can also contain other lines\.
+ In the `metrics` section,inside the `metrics_collected` section, check the sections for each resource type that you want metrics explorer to discover, such as the `cpu`, `disk`, and `memory` sections\. Make sure that each of these sections has a `"resources": [ "*"] line.`\. `aggregation_dimensions` parameter includes `[InstanceId"]`\. It can also contain other dimensions\.
+ In the `cpu` section of the `metrics_collected>` section, make sure there is a `"totalcpu": true` line\.

The settings in the previous list cause the CloudWatch agent to publish aggregate metrics for disks, CPUs, and other resources that can be plotted in metrics explorer for all the instances that use it\.

These settings will republish the metrics that you had previously set up to be published with multiple dimensions, adding to your metric costs\.

For more information about editing the CloudWatch agent configuration file, see [ Manually Create or Edit the CloudWatch Agent Configuration File](CloudWatch-Agent-Configuration-File-Details.md)\. 

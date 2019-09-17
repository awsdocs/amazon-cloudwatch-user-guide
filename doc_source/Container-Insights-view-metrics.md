# Viewing Container Insights Metrics<a name="Container-Insights-view-metrics"></a>

After you have Container Insights set up and it's collecting metrics, you can view those metrics on the CloudWatch automatic dashboards\.

For Container Insights metrics to appear on your dashboard, you must complete the Container Insights setup\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.

**To view Container Insights metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the upper left of the screen, select the down arrow next to **Overview** and choose **Container Insights**\.

   Several graphs appear, displaying metrics about your clusters\. Below the graphs is a list of clusters, with health and basic metrics shown for each one\.

   To filter the view to a certain cluster, use the box at the top in the middle\. 

1. At the top left, where it shows **Clusters**, switch to see metrics at the node, pod, service, task, and namespace levels\. When you do so, you can also filter those views to look only at individual pods and nodes\.

   You can also switch between Amazon EKS and Amazon ECS metrics\.

1. In any graph, pause on a legend line to see more information about that resource\.

1. At the cluster level, to view logs about one of the clusters in the dashboard, select the button next to its name in the list at the bottom of the screen\. Then choose **Actions** and select one of the options in the list\.

   A new browser appears, showing a CloudWatch Logs Insights query for that resource\.

   Choose **Run query**\. The query results are diplayed\.

   For more information about CloudWatch Logs Insights, see [Analyze Log Data With CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\.

1. At the node, pod, or task level, to view logs about one or more resources currently displayed in the list at the bottom of the page, select the check boxes next to the names of those resources\. Then choose **Actions** and select one of the options in the list\. You can view logs or AWS X\-Ray traces of the resource\.

   A new browser appears, showing the information you requested\.

You can set a CloudWatch alarm on any metric that Container Insights collects\. For more information, see [Using Amazon CloudWatch Alarms](AlarmThatSendsEmail.md)

## Using CloudWatch Logs Insights to View Container Insights Data<a name="Container-Insights-CloudWatch-Logs-Insights"></a>

Container Insights collects metrics by using performance log events, which are stored in CloudWatch Logs\. You can use CloudWatch Logs Insights queries for additional views of your container data\.

For more information about CloudWatch Logs Insights, see [Analyze Log Data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\. For more information about the log fields you can use in queries, see [](Container-Insights-reference-performance-logs-EKS.md)\.

**To use CloudWatch Logs Insights to query your container metric data**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Insights**\.

   Near the top of the screen is the query editor\. When you first open CloudWatch Logs Insights, this box contains a default query that returns the 20 most recent log events\.

1. In the box above the query editor, select one of the Container Insights log groups to query\. For the following example queries to work, the log group name must end with **performance**\.

   When you select a log group, CloudWatch Logs Insights automatically detects fields in the data in the log group and displays them in **Discovered fields** in the right pane\. It also displays a bar graph of log events in this log group over time\. This bar graph shows the distribution of events in the log group that matches your query and time range, not only the events displayed in the table\.

1. In the query editor, replace the default query with the following query and choose **Run query**\.

   ```
   STATS avg(node_cpu_utilization) as avg_node_cpu_utilization by NodeName
   | SORT avg_node_cpu_utilization DESC
   ```

   This query shows a list of nodes, sorted by average node CPU utilization\.

1. To try another example, replace that query with another query and choose **Run query**\. More sample queries are listed later on this page\.

   ```
   STATS avg(number_of_container_restarts) as avg_number_of_container_restarts by PodName
   | SORT avg_number_of_container_restarts DESC
   ```

   This query displays a list of your pods, sorted by average number of container restarts\.

1. If you want to try another query, you can use include fields in the list at the right of the screen\. For more information about query syntax, see [CloudWatch Logs Insights Query Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)\.

### Other Sample Queries for Container Insights<a name="Container-Insights-sample-queries"></a>

**List of your pods, sorted by average number of container restarts**

```
STATS avg(number_of_container_restarts) as avg_number_of_container_restarts by PodName
| SORT avg_number_of_container_restarts DESC
```

**Pods requested vs\. pods running**

```
fields @timestamp, @message 
| sort @timestamp desc 
| filter Type="Pod" 
| stats min(pod_number_of_containers) as requested, min(pod_number_of_running_containers) as running, ceil(avg(pod_number_of_containers-pod_number_of_running_containers)) as pods_missing by kubernetes.pod_name 
| sort pods_missing desc
```

**Count of cluster node failures**

```
stats avg(cluster_failed_node_count) as CountOfNodeFailures 
| filter Type="Cluster" 
| sort @timestamp desc
```

**Application log errors**

```
Count by container name: stats count() as countoferrors by kubernetes.container_name 
| filter stream="stderr" 
| sort countoferrors desc
```

**Disk usage by container name**

```
stats floor(avg(container_filesystem_usage/1024)) as container_filesystem_usage_avg_kb by InstanceId, kubernetes.container_name, device 
| filter Type="ContainerFS" 
| sort container_filesystem_usage_avg_kb desc
```

**CPU usage by container name**

```
stats pct(container_cpu_usage_total, 50) as CPUPercMedian by kubernetes.container_name 
| filter Type="Container"
```
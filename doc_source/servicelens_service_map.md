# Using the service map in ServiceLens<a name="servicelens_service_map"></a>

This section introduces the service map and helps you learn to navigate it\.

To see a service map, you must have installed AWS X\-Ray and completed the other ServiceLens deployment steps\. For more information, see [Deploying ServiceLens](deploy_servicelens.md)\.

You must also be signed in to an account that has the `AWSXrayReadOnlyAccess` managed policy, as well as permissions that enable you to view the CloudWatch console\. For more information, see [How AWS X\-Ray Works with IAM](https://docs.aws.amazon.com/xray/latest/devguide/security_iam_service-with-iam.html) and [Using Amazon CloudWatch dashboards](CloudWatch_Dashboards.md)\.

**To begin using the service map**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **ServiceLens**, **Service Map**\.

   A service map appears\. It has the following parts:
   + The AWS services and your custom applications that you have enabled tracing for are shown as circles or “nodes\.” The size of each node indicates the relative number of traced requests that are going to that service\.
   + Edges, or connections between nodes, are shown as lines connecting the nodes\. By default, the thickness of a line indicates the relative number of traced requests between those nodes\.

     You can use the dropdown menu in the top right to choose whether the number of traced requests or the average latency is used for node and edge sizing\. You can also select to use constant size for all nodes and edges\.
   + The entry point to your nodes is shown on the left as a "Client\." A "Client" represents both web server traffic and traced API operation requests\.
   + A node outlined partially in red, orange, or purple has issues\. Some traced requests to these nodes have faults, errors, or throttling\. The percentage of the color outline indicates the percentage of traced requests that are having issues\.
   + If a node has a triangle with an exclamation point next to it, at least one CloudWatch alarm related to that node is in alarm state\.

1. By default, the data in the map is for the most recent 6\-hour time window\. To change the timeframe of the window, use the controls at the upper right of the screen\. The time range to be shown can be up to 6 hours, and can be as much as 30 days in the past\.

1. If you have enabled X\-Ray groups, you can filter the map by selecting an X\-ray group in the filter\.

1. To view metrics for a node, choose the node\. To then see more information about that node, choose **View logs**, **View traces**, or **View dashboard**\. 

1. To focus on the incoming and outgoing connections for a node, select the node and choose **View connections** near the top of the service map\.

1. To see a pop\-up displaying latency, errors, requests, and alarm summary statistics for a node, pause on that node\.

1. To see latency statistics for an edge connection, pause on the line representing that edge\.

1. To display alarm status for a service, along with line charts for latency, errors, and trace counts, choose that service node on the map\.

   For more information about this view, see the following procedures\. 

1. To view the service map as a table, choose **List view** near the top of the screen\. In this view, you can filter and sort the nodes and alarms that are displayed on the map\.

1. To see a dashboard with metrics for a specific node, select the node and then choose **View dashboard** near the bottom of the screen\.

**To view traces for a service or application on the service map**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **ServiceLens**, **Service Map**\.

1. Choose the node that represents the service or application that you want to investigate\.

   CloudWatch displays line charts of latency, errors, and trace counts for that service, along with a summary of alarm status\.

   Above those charts are options to dive down to logs and traces for the service\.

1. To view traces related to the service, choose **View traces**\.

   The console switches to the **Traces** view, focused on the service that you are investigating\. For more information, see [Using the traces view in ServiceLens](servicelens_service_map_traces.md)\.
# See Amazon ECS lifecycle events within Container Insights<a name="container-insights-ECS-lifecycle-events"></a>

You can view Amazon ECS lifecycle events within the Container Insights console\. This helps you correlate your container metrics, logs, and events in a single view to give you a more complete operational visibility\.

The events include container instance state change events, task state change events, and service action events\. They are automatically sent by Amazon ECS to Amazon EventBridge and are also collected in CloudWatch in event log format\. For more information about these events, see [Amazon ECS events](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_cwe_events.html)\.

Lifecycle events don't incur extra costs\. Standard Container Insights pricing applies\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

To configure the table of lifecycle events and create rules for a cluster, you must have the `events:PutRule`, `events:PutTargets`, and `logs:CreateLogGroup` permissions\.

To view the table of lifecycle events, you must have the `events:DescribeRule`, `events:ListTargetsByRule`, and `logs:DescribeLogGroups` permissions\.

**To view Amazon ECS lifecycle events in the CloudWatch Container Insights console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Insights**, **Container Insights**\.

1. In the drop\-down box near the top of the page, choose **Performance monitoring**\. 

1. In the next drop\-down, choose either **ECS Clusters**, **ECS Services**, or **ECS Tasks**\.

1. If you chose **ECS Services** or **ECS Tasks** in the previous step, choose the **Lifecycle events** tab\.

1. At the bottom of the page, if you see **Configure lifecycle events**, choose it to create EventBridge rules for your cluster\.

   The events are displayed below the container insights panes and above the Application Insights section\. To run extra analytics and create additional visualizations on these events, choose **View in Logs Insights** in the Lifecycle Events table\.
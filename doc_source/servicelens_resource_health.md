# Using the resource health view in ServiceLens<a name="servicelens_resource_health"></a>

You can use the resource health view to automatically discover, manage, and visualize the health and performance of hosts across their applications in a single view\. You can visualize the health of their hosts by a performance dimension such as CPU or memory, and slice and dice hundreds of hosts in a single view using filters\. You can filter by tags or by use cases, such as hosts in the same Auto Scaling group or hosts that use the same load balancer, 

## Prerequisites<a name="servicelens_resource_health-prerequisites"></a>

To make sure that you get the full benefit of the resource health view, check that you have the following prerequisites\.
+ To see the memory utilization of your hosts and use it as a filter, you must install the CloudWatch agent on your hosts and set it up to send a memory metric to CloudWatch in the default `CWAgent` namespace\. On Linux and macOS instances, the CloudWatch agent must send the `mem_used_percent` metric\. On Windows instances, the agent must send the `Memory % Committed Bytes in Use` metric\. These metrics are included if you use the wizard to create the CloudWatch agent configuration file and select any of the pre\-defined sets of metrics\. Metrics collected by the CloudWatch agent are billed as custom metrics\. For more information, see [Installing the CloudWatch agent](install-CloudWatch-Agent-on-EC2-Instance.md)\. 

  When you use the CloudWatch agent to collect these memory metrics to use with the resource health view, you must include the following section in the CloudWatch agent configuration file\. This section contains the default dimension settings and is created by default, so do not change any part of this section to anything different than what is shown in the following example\.

  ```
  "append_dimensions": {
    "ImageId": "${aws:ImageId}",
    "InstanceId": "${aws:InstanceId}",
    "InstanceType": "${aws:InstanceType}",
    "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
  },
  ```
+  To view all the information available in the resource health view, you must be signed in to an account that has the following permissions\. If you are signed on with fewer permissions, you can still use the resource health view but some performance data will not be visible\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Action": [
                  "autoscaling:Describe*",
                  "cloudwatch:Describe*",
                  "cloudwatch:Describe*",
                  "cloudwatch:Get*",
                  "cloudwatch:List*",
                  "logs:Get*",
                  "logs:Describe*",
                  "sns:Get*",
                  "sns:List*",
                  "ec2:DescribeInstances",
                  "ec2:DescribeInstanceStatus",
                  "ec2:DescribeRegions"
              ],
              "Effect": "Allow",
              "Resource": "*"
          }
      ]
  }
  ```

**To view resource health in your account**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Resource Health**\.

   The resource health page appears, showing a square for each host in your account\. Each square is colored based on the current status of that host, based on the setting for **Color by**\. Host squares with an alarm symbol have one or more alarms currently in ALARM state\.

   You can see up to 500 hosts in a single view\. If you have more hosts in your account, use the filter settings in step 6 of this procedure\.

1. To change what criteria is used to show each host's health, choose a setting for **Color by**\. You can choose **CPU Utilization**, **Memory Utilization**, or **Status check**\. Memory utilization metrics are available only for hosts that are running the CloudWatch agent and have it configured to collect memory metrics and send them to the default `CWAgent` namespace\. For more information, see [Collect metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](Install-CloudWatch-Agent.md)\.

1. To change the thresholds and the colors that are used for the health indicators in the grid, choose the gear icon above the grid\.

1. To toggle whether to show alarms in the host grid, choose or clear **Show alarms across all metrics**\.

1. To split the hosts in the map into groups, choose a grouping criteria for **Group by**\.

1. To narrow the view to fewer hosts, choose a filter criteria for **Filter by**\. You can filter by tags and by resource groupings such as Auto Scaling group, instance type, security group, and more\.

1. To sort hosts, choose a sorting criteria for **Sort by**\. You can sort by status check results, instance state, CPU or memory utilization, and the number of alarms that are in ALARM state\.

1. To see more information about a host, choose the square that represents that host\. A popup pane appears\. To then dive deeper into information about that host, choose **View dashboard** or **View on list**\.
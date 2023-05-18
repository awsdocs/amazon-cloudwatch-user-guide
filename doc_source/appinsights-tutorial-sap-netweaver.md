# Tutorial: Set up monitoring for SAP NetWeaver<a name="appinsights-tutorial-sap-netweaver"></a>

This tutorial demonstrates how to configure Amazon CloudWatch Application Insights to set up monitoring for SAP NetWeaver\. You can use CloudWatch Application Insights automatic dashboards to visualize problem details, accelerate troubleshooting, and reduce mean time to resolution \(MTTR\) for your SAP NetWeaver application servers\.

**Topics**
+ [Supported environments](#appinsights-tutorial-sap-netweaver-supported-environments)
+ [Supported operating systems](#appinsights-tutorial-sap-netweaver-supported-os)
+ [Features](#appinsights-tutorial-sap-netweaver-features)
+ [Prerequisites](#appinsights-tutorial-sap-netweaver-prerequisites)
+ [Set up monitoring](#appinsights-tutorial-sap-netweaver-set-up)
+ [Manage monitoring](#appinsights-tutorial-sap-netweaver-manage)
+ [Troubleshooting problems](#appinsights-tutorial-sap-netweaver-troubleshooting)
+ [Troubleshooting Application Insights](#appinsights-tutorial-sap-netweaver-troubleshooting-health-dashboard)

## Supported environments<a name="appinsights-tutorial-sap-netweaver-supported-environments"></a>

CloudWatch Application Insights supports the deployment of AWS resources for the following systems and patterns\. 
+ **SAP NetWeaver Distributed deployments on multiple Amazon EC2 instances\.**
+ **Cross\-AZ SAP NetWeaver high availability setup** – SAP NetWeaver with high availability configured across two Availability Zones using SUSE/RHEL clustering\.

## Supported operating systems<a name="appinsights-tutorial-sap-netweaver-supported-os"></a>

CloudWatch Application Insights for SAP NetWeaver is supported on the following operating systems:
+ Oracle Linux 8
+ Red Hat Enterprise Linux 7\.6
+ Red Hat Enterprise Linux 7\.7
+ Red Hat Enterprise Linux 7\.9
+ Red Hat Enterprise Linux 8\.1
+ Red Hat Enterprise Linux 8\.2
+ Red Hat Enterprise Linux 8\.4
+ SUSE Linux Enterprise Server 15 for SAP
+ SUSE Linux Enterprise Server 15 SP1 for SAP
+ SUSE Linux Enterprise Server 15 SP2 for SAP
+ SUSE Linux Enterprise Server 15 SP3 for SAP
+ SUSE Linux Enterprise Server 15 SP4 for SAP
+ SUSE Linux Enterprise Server 12 SP4 for SAP
+ SUSE Linux Enterprise Server 12 SP5 for SAP
+ SUSE Linux Enterprise Server 15 except High Availability patterns
+ SUSE Linux Enterprise Server 15 SP1 except High Availability patterns
+ SUSE Linux Enterprise Server 15 SP2 except High Availability patterns
+ SUSE Linux Enterprise Server 15 SP3 except High Availability patterns
+ SUSE Linux Enterprise Server 15 SP4 except High Availability patterns
+ SUSE Linux Enterprise Server 12 SP4 except High Availability patterns
+ SUSE Linux Enterprise Server 12 SP5 except High Availability patterns

## Features<a name="appinsights-tutorial-sap-netweaver-features"></a>

CloudWatch Application Insights for SAP NetWeaver 7\.0x–7\.5x \(including ABAP Platform\) provides the following features:
+ Automatic SAP NetWeaver workload detection 
+ Automatic SAP NetWeaver alarm creation based on static thresholds
+ Automatic SAP NetWeaver log pattern recognition 
+ Health dashboard for SAP NetWeaver
+ Problem dashboard for SAP NetWeaver

## Prerequisites<a name="appinsights-tutorial-sap-netweaver-prerequisites"></a>

You must perform the following prerequisites to configure SAP NetWeaver with CloudWatch Application Insights:
+ **AWS Systems Manager enablement** – Install SSM Agent on your Amazon EC2 instances, and enable the instances for SSM\. For information about how to install the SSM Agent, see [Setting up AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) in the *AWS Systems Manager User Guide*\.
+ **Amazon EC2 instance roles** – You must attach the following Amazon EC2 instance roles to configure your SAP NetWeaver monitoring\.
  + You must attach the `AmazonSSMManagedInstanceCore` role to enable Systems Manager\. For more information, see [AWS Systems Manager identity\-based policy examples](https://docs.aws.amazon.com/systems-manager/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html)\.
  + You must attach the `CloudWatchAgentServerPolicy` policy to enable instance metrics and logs to be emitted through CloudWatch\. For more information, see [Create IAM roles and users for use with CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)\.
+ **AWS resource groups** – You must create a resource group that includes all of the associated AWS resources used by your application stack to onboard your applications to CloudWatch Application Insights\. This includes Amazon EC2 instances, Amazon EFS, and Amazon EBS volumes running your SAP NetWeaver application servers\. If there are multiple SAP NetWeaver systems per account, we recommend that you create one resource group that includes the AWS resources for each SAP NetWeaver system\. For more information about creating resource groups, see the *[AWS Resource Groups and Tags User Guide](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html)*\.
+ **IAM permissions** – For users who don't have administrative access, you must create an AWS Identity and Access Management \(IAM\) policy that allows Application Insights to create a service\-linked role and attach it to the user's identity\. For more information about how to create the IAM policy, see [IAM policy](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-iam.html)\.
+ **Service\-linked role** – Application Insights uses AWS Identity and Access Management \(IAM\) service\-linked roles\. A service\-linked role is created for you when you create your first Application Insights application in the Application Insights console\. For more information, see [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)\.
+ **Amazon CloudWatch agent** – Application Insights installs and configures the CloudWatch agent\. If you have CloudWatch agent installed, Application Insights retains your configuration\. To avoid a merge conflict, remove the configuration of resources that you want to use in Application Insights from the existing CloudWatch agent configuration file\. For more information, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

## Set up your SAP NetWeaver application servers for monitoring<a name="appinsights-tutorial-sap-netweaver-set-up"></a>

Use the following steps to set up monitoring for your SAP NetWeaver application servers\.

**To set up monitoring**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. From the left navigation pane, under **Insights**, select **Application Insights**\.

1. The **Application Insights** page displays the list of applications that are monitored with Application Insights, and the monitoring status for each application\. In the upper right\-hand corner, select **Add an application**\.

1. On the **Specify application details** page, from the dropdown list under **Resource group**, select the AWS resource group you created that contains your SAP NetWeaver resources\. If you haven't created a resource group for your application, you can create one by choosing **Create new resource group** under the **Resource group** dropdown list\. 

1. Under **Automatic monitoring of new resources**, select the check box to allow Application Insights to automatically monitor the resources that are added to the application's resource group after onboarding\. 

1. Under **Monitor EventBridge events**, select the check box to integrate Application Insights monitoring with CloudWatch Events to get insights from Amazon EBS, Amazon EC2, AWS CodeDeploy, Amazon ECS, AWS Health APIs and notifications, Amazon RDS, Amazon S3, and AWS Step Functions\.

1. Under **Integrate with AWS Systems Manager OpsCenter**, select the check box next to **Generate AWS Systems Manager OpsCenter OpsItems for remedial actions** to view and get notifications when problems are detected for the selected applications\. To track the operations that are performed to resolve operational work items, called [OpsItems](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter-getting-started-sns.html), that are related to your AWS resources, provide an SNS topic ARN\. 

1. You can optionally enter tags to help you identify and organize your resources\. CloudWatch Application Insights supports both tag\-based and AWS CloudFormation stack\-based resource groups, with the exception of Application Auto Scaling groups\. For more information, see [Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) in the *AWS Resource Groups and Tags User Guide*\. 

1. Choose **Next** to continue to set up monitoring\.

1. On the **Set up monitoring** page, the monitored components automatically detected by CloudWatch Application Insights are listed\. Choose **Next**\.

1. On the **Specify component details** page, choose **Next**\.

1. Review your application monitoring configuration, and choose **Submit**\.

1. The application details page opens, where you can view the **Application summary**, **Dashboard**, and **Components**\. You can also view the **Configuration history**, **Log patterns**, and any **Tags** that you have created\. After you submit your application, CloudWatch Application Insights deploys all of the metrics and alarms for your SAP NetWeaver system, which can take up to an hour\.

## Manage monitoring of your SAP NetWeaver application servers<a name="appinsights-tutorial-sap-netweaver-manage"></a>

Use the following steps to manage monitoring of your SAP NetWeaver application servers\.

**To manage monitoring**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. From the left navigation pane, under **Insights**, select **Application Insights**\.

1. Choose the **List view** tab\.

1. The **Application Insights** page displays the list of applications that are monitored with Application Insights, and the monitoring status for each application\.

1. Select your application\.

1. Choose the **Components** tab\.

1. Under **Monitored components**, select the radio button next to the component name\. Then, select **Manage monitoring**\.

1. Under **Instance logs**, you can update the existing log path, log pattern set, and log group name\. In addition, you can add up to three additional **Application logs**\.

1. Under **Metrics**, you can select the SAP NetWeaver metrics according to your requirements\. SAP NetWeaver metric names are prefixed with `sap`\. You can add up to 40 metrics per component\.

1. Under **Custom alarms**, you can add additional alarms to be monitored by CloudWatch Application Insights\.

1. Review your application monitoring configuration and choose **Save**\. When you submit your configuration, your account updates all of the metrics and alarms for your SAP NetWeaver systems\.

## View and troubleshoot SAP NetWeaver problems detected by CloudWatch Application Insights<a name="appinsights-tutorial-sap-netweaver-troubleshooting"></a>

The following sections provide steps to help you resolve common troubleshooting scenarios that occur when you configure monitoring for SAP NetWeaver on Application Insights\.

**Topics**
+ [SAP NetWeaver database connectivity issues](#appinsights-tutorial-sap-netweaver-troubleshooting-database)
+ [SAP NetWeaver application availability issues](#appinsights-tutorial-sap-netweaver-troubleshooting-availability)

### SAP NetWeaver database connectivity issues<a name="appinsights-tutorial-sap-netweaver-troubleshooting-database"></a>

**Description**  
Your SAP NetWeaver application experiences database connectivity issues\.

**Cause**  
You can identify the connectivity issue by going to the CloudWatch Application Insights console and checking the SAP NetWeaver Application Insights problem dashboard\. Select the link under **Problem summary** to see the specific issue\.

![\[Detected problems dashboard for CloudWatch Application Insights with further information under the Detected problems section in the Problem summary column.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-1.png)

In the following example, under **Problem summary**, SAP: Availability is the issue\.

![\[Problem summary page for CloudWatch Application Insights under the Problem summary section.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-2.png)

Immediately following the **Problem summary**, the **Insight** section provides more context about the error and where you can get more information about the causes of the issue\.

![\[Problem insight for CloudWatch Application Insights with additional information about the cause of the error.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-3.png)

On the same problem dashboard, you can view related logs and metrics that problem detection has grouped together to help you isolate the cause of the error\. The `sap_alerts_Availability` metric tracks the availability of the SAP NetWeaver system over time\. You can use historical tracking to correlate when the metric initiated an error state or breached the alarm threshold\. In the following example, there is an availability issue with the SAP NetWeaver system\. The example shows two alarms because there are two SAP application server instances and an alarm was created for each instance\.

![\[SAP Availability metric for CloudWatch Application Insights with additional information about the history of when the error occurred.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-4.png)

For more information about each alarm, hover over the `sap_alerts_Availability` metric name\.

![\[SAP Availability metric for CloudWatch Application Insights with additional details about the error.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-5.png)

In the following example, the `sap_alerts_Database` metric shows that the database layer has an issue or a failure\. This alarm indicates that SAP NetWeaver had issues connecting to or communicating with its database\. 

![\[SAP Database metric for CloudWatch Application Insights with additional history about when the error occurred.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-6.png)

Since the database is a key resource for SAP NetWeaver, you may get many related alarms when the database has an issue or failure\. In the following example, the `sap_alerts_FrontendResponseTime` and `sap_alerts_LongRunners` metrics are initiated because the database is not available\.

![\[Additional SAP Database metrics for CloudWatch Application Insights created because of a database failure.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-7.png)

**Resolution**  
Application Insights monitors the detected problem hourly\. If there are no new related log entries in your SAP NetWeaver log files, the older log entries will be treated as resolved\. You must fix any error conditions related to the CloudWatch alarms\. After the error conditions are fixed, the alarm is resolved when the alarms and logs are recovered\. When all of the CloudWatch log errors and alarms are resolved, Application Insights stops detecting errors and the problem is automatically resolved within an hour\. We recommend that you resolve all log error conditions and alarms so that you have the latest problems on the problem dashboard\.

In the following example, the SAP Availability issue is resolved\.

![\[CloudWatch Application Insights problem dashboard with SAP Availability issue resolved.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-database-connectivity-resolved.png)

### SAP NetWeaver application availability issues<a name="appinsights-tutorial-sap-netweaver-troubleshooting-availability"></a>

**Description**  
Your SAP NetWeaver High Availability Enqueue replication stopped working\.

**Cause**  
You can identify the connectivity issue by going to the CloudWatch Application Insights console and checking the SAP NetWeaver Application Insights problem dashboard\. Select the link under **Problem summary** to see the specific issue\.

![\[Problem dashboard in CloudWatch Application Insights with more information under Problem summary.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-problem-dashboard.png)

In the following example, under **Problem summary**, High Availability Enqueue Replication is the issue\.

![\[Problem summary in CloudWatch Application Insights with SAP Availability: Enqueue replication error listed.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-1.png)

Immediately following the **Problem summary**, the **Insight** section provides more context about the error and where you can get more information about the causes of the issue\.

![\[Problem insight for CloudWatch Application Insights with additional information about the cause of the error.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-2.png)

The following example shows the problem dashboard where you view logs and metrics which are grouped to help you isolate the causes of the error\. The `sap_enqueue_server_replication_state` metric tracks the value over time\. You can use historical tracking to correlate when the metric initiated an error state or breached the alarm threshold\.

![\[Enqueue server replication state metric on the problem dashboard with additional information about when the error occurred.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-3.png)

In the following example, the `ha_cluster_pacemaker_fail_count` metric shows that the high availability pacemaker cluster experienced a resource failure\. The specific pacemaker resources that had a fail count greater than or equal to one are identified in the component dashboard\.

![\[Application availability metric for CloudWatch Application Insights for pacemaker resource with fail count greater than or equal to one.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-4.png)

The following example shows the `sap_alerts_Shortdumps` metric, which indicates that the SAP application performance was reduced when the problem was detected\.

![\[Application availability alert Shortdumps metric for CloudWatch Application Insights.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-5.png)

#### Logs<a name="ai-sap-netweaver-logs"></a>

The log entries are helpful to get a better understanding of issues that occurred at the SAP NetWeaver layer when the problem was detected\. The log group widget in the problem dashboard shows the specific time of the issue\.

![\[Log entries for CloudWatch Application Insights showing the exact time issues occurred.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-7.png)

To see detailed information about the logs, select the three vertical dots in the upper\-right corner, and select **View in CloudWatch Logs Insights**\.

![\[CloudWatch Application Insights details with View in CloudWatch Logs Insights.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-8.png)

Use the following steps to get more information about the metrics and alarms displayed in the problem dashboard\.

**To get more information about metrics and alarms**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. In the left navigation pane, under **Insights**, select **Application Insights**\. Then, choose the **List view** tab, and select your application\.

1. Select the **Components** tab\. Then, select the SAP NetWeaver component about which you want to get more information\.

   The following example shows the **HA Metrics** section with the `ha_cluster_pacemaker_fail_count` metric that was displayed in the problem dashboard\.  
![\[HA Metrics for CloudWatch Application Insights showing the pacemaker resources fail count.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-9.png)

**Resolution**  
Application Insights monitors the detected problem hourly\. If there are no new related log entries in your SAP NetWeaver log files, the older log entries will be treated as resolved\. You must fix any error conditions related to this problem\.

For the `sap_alerts_Shortdumps` alarm, you must resolve the alert in the SAP NetWeaver system by using transaction code `RZ20 → R3Abap → Shortdumps` to navigate to the CCMS alert\. For more information about CCMS alerts, see the [SAP website\.](https://help.sap.com/docs/SAP_NETWEAVER_701/6f45651d6c4b1014a50f9ef0fc8df39d/408dc4a7c415437a9b91d2ef6caa9d7d.html) Resolve all of the CCMS alerts in the Shortdumps tree\. After all of the alerts are resolved in the SAP NetWeaver system, CloudWatch no longer reports the metric in an alarm state\.

When all of the CloudWatch log errors and alarms are resolved, Application Insights stops detecting errors and the problem is automatically resolved within an hour\. We recommend that you resolve all log error conditions and alarms so that you have the latest problems on the problem dashboard\. In the following example, the SAP Netweaver High Availability Enqueue Replication problem is resolved\.

![\[Problem dashboard for CloudWatch Application Insights showing the SAP Availability: Enqueue Replication problem with a Status of Resolved.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-nw-app-availability-problem-resolved.png)

## Troubleshooting Application Insights for SAP NetWeaver<a name="appinsights-tutorial-sap-netweaver-troubleshooting-health-dashboard"></a>

This section provides steps to help you resolve common errors returned by the Application Insights dashboard\.

### <a name="ai-unable-add-monitor-metrics"></a>

**Unable to add more than 60 monitor metrics**  
**Error returned**: `Component cannot have more than 60 monitored metrics.`

**Root cause**: `The current metric limit is 60 monitor metrics per component.`

**Resolution**: Remove metrics that are not necessary to adhere to the limit\.

### <a name="sap-metrics-not-on-dashboard"></a>

**SAP metrics do not appear on the dashboard after the onboarding process**  
**Root cause**: Component Dashboard uses a five minute metric period to aggregate the data points\.

**Resolution**: All metrics should show up on the dashboard after five minutes\.

### <a name="sap-metrics-and-alarms-not-on-dashboard"></a>

**SAP metrics and alarms don't appear on the dashboard**  
Use the following steps to identify why SAP metrics and alarms don't appear on the dashboard after the onboarding process\. 

**To identify the issue with metrics and alarms**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. In the left navigation pane, under **Insights**, select **Application Insights**\. Then, choose the **List view** tab, and select your application\.

1. Choose the **Configuration history** tab\.

1. If you see missing metrics datapoints, check for errors related to `prometheus-sap_host_exporter`\.

1. If you don't find an error in the previous step, [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html)\. For High Availability deployments, connect to the primary cluster Amazon EC2 instance\.

1. In your instance, verify that the exporter is running by using the following command\. The default port is `9680`\. If you are using a different port, replace `9680` with the port you are using\.

   ```
   curl localhost:9680/metrics
   ```

   If no data is returned, then the exporter failed to start\.

1. Run the following command to check for errors in the exporter service logs:

   ```
   sudo journalctl -e --unit=prometheus-sap_host_exporter.service
   ```

   If this command does not return an error, continue to the next step\.

1. Run the following command to check the exporter manager service logs for errors:

   ```
   sudo journalctl -e --unit=prometheus-sap_host_exporter_manager.service
   ```
**Note**  
This service should be up and running at all times\.

   If this command does not return an error, continue to the next step\.

1. Run the following command to manually start the exporter\. Then, check the exporter output\.

   ```
   sudo /opt/aws/sap_host_exporter/sap_host_exporter
   ```

   You can exit the exporter process after you check for errors\.

**Root cause**: There are several possible causes for this issue\. A common cause is that the exporter is not able to connect to one of the application server instances\.

**Resolution**

Use the following steps to connect the exporter to the application server instances\. You will verify that the SAP application instance is running and use SAPControl to connect to the instance\.

**To connect the exporter to the application server instances**

1. In your Amazon EC2 instance, run the following command to verify that the SAP application is running\. 

   ```
   sapcontrol -nr <App_InstNo> -function GetProcessList
   ```

1. You must establish a working SAPControl connection\. If the SAPControl connection doesn't work, find the root cause of the issue on the relevant SAP application instance\.

1. To manually start the exporter after you fix the SAP Control connection issue, run the following command:

   ```
   sudo systemctl start prometheus-sap_host_exporter.service
   ```

1. If you can't resolve the SAPControl connection issue, use the following procedure as a temporary fix\. 

   1. Open the [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager)\.

   1. From the left navigation pane, choose **State Manager**\.

   1. Under **Associations** search for the SAP NetWeaver system's association\.

      ```
      Association Name: Equal: AWS-ApplicationInsights-SSMSAPHostExporterAssociationForCUSTOMSAPNW<SID>-1
      ```

   1. Select the **Association id**\.

   1. Choose the **Parameters** tab and remove the application server number from **additionalArguments**\.

   1. Choose **Apply Association Now**\.
**Note**  
This is a temporary fix\. If updates are made to the component's monitoring configurations, the instance will be added back\.
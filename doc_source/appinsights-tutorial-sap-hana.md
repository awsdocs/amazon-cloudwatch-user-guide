# Tutorial: Set up monitoring for SAP HANA<a name="appinsights-tutorial-sap-hana"></a>

This tutorial demonstrates how to configure CloudWatch Application Insights to set up monitoring for your SAP HANA databases\. You can use CloudWatch Application Insights automatic dashboards to visualize problem details, accelerate troubleshooting, and facilitate mean time to resolution \(MTTR\) for your SAP HANA databases\.

**Topics**
+ [Supported environments](#appinsights-tutorial-sap-hana-supported-environments)
+ [Supported operating systems](#appinsights-tutorial-sap-hana-supported-os)
+ [Features](#appinsights-tutorial-sap-hana-features)
+ [Prerequisites](#appinsights-tutorial-sap-hana-prerequisites)
+ [Set up monitoring](#appinsights-tutorial-sap-hana-set-up)
+ [Manage monitoring](#appinsights-tutorial-sap-hana-manage)
+ [Configure the alarm threshold](#appinsights-tutorial-sap-hana-configure-alarm-threshold)
+ [Troubleshooting problems detected](#appinsights-tutorial-sap-hana-troubleshooting)
+ [Anomaly detection](#appinsights-tutorial-sap-hana-troubleshooting-anomaly-detection)
+ [Troubleshooting Application Insights](#appinsights-tutorial-sap-hana-troubleshooting-health-dashboard)

## Supported environments<a name="appinsights-tutorial-sap-hana-supported-environments"></a>

CloudWatch Application Insights supports the deployment of AWS resources for the following systems and patterns\. You provide and install SAP HANA database software and supported SAP application software\.
+ **SAP HANA database on a single Amazon EC2 instance** — SAP HANA in a single\-node, scale\-up architecture, with up to 24TB of memory\.
+ **SAP HANA database on multiple Amazon EC2 instances** — SAP HANA in a multi\-node, scale\-out architecture\.
+ **Cross\-AZ SAP HANA database high availability setup** — SAP HANA with high availability configured across two Availability Zones using SUSE/RHEL clustering\.

## Supported operating systems<a name="appinsights-tutorial-sap-hana-supported-os"></a>

CloudWatch Application Insights for SAP HANA supports x86\-64 architecture on the following operating systems:
+ SUSE Linux 15
+ SuSE Linux 15 SP1
+ SuSE Linux 15 SP2
+ SuSE Linux 15 For SAP
+ SuSE Linux 15 SP1 For SAP
+ SuSE Linux 15 SP2 For SAP
+ RedHat Linux 8\.2 For SAP With High Availability and Update Services
+ RedHat Linux 8\.1 For SAP With High Availability and Update Services
+ RedHat Linux 7\.9 For SAP With High Availability and Update Services

## Features<a name="appinsights-tutorial-sap-hana-features"></a>

CloudWatch Application Insights for SAP HANA provides the following features:
+ Automatic SAP HANA workload detection 
+ Automatic SAP HANA alarm creation based on static threshold
+ Automatic SAP HANA alarm creation based on anomaly detection 
+ Automatic SAP HANA log pattern recognition 
+ Health dashboard for SAP HANA
+ Problem dashboard for SAP HANA

## Prerequisites<a name="appinsights-tutorial-sap-hana-prerequisites"></a>

You must perform the following prerequisites to configure an SAP HANA database with CloudWatch Application Insights:
+ **SAP HANA** — Install a running and reachable SAP HANA database 2\.0 SPS05 on an Amazon EC2 instance\.
+ **SAP HANA database user** — Run the following SQL commands to create a user with monitoring roles\.

  ```
  su - <sid>adm
  hdbsql -u SYSTEM -p <SYSTEMDB password> -d SYSTEMDB
  CREATE USER CW_HANADB_EXPORTER_USER PASSWORD <Monitoring user password> NO FORCE_FIRST_PASSWORD_CHANGE;
  CREATE ROLE CW_HANADB_EXPORTER_ROLE;
  GRANT MONITORING TO CW_HANADB_EXPORTER_ROLE;
  GRANT CW_HANADB_EXPORTER_ROLE TO CW_HANADB_EXPORTER_USER;
  ```
+ **Python 3\.6** — Install Python 3\.6 or later versions on your operating system\. If Python is not detected on your operating system, Python 3\.8 will be installed\. 
+ **Pip3** — Install the installer program, pip3, on your operating system\. If pip3 is not detected on your operating system, it will be installed\.
+ **Amazon CloudWatch agent** — Make sure that you are not running a preexisting Amazon CloudWatch agent on your Amazon EC2 instance\.
+ **AWS Systems Manager enablement** — Install SSM Agent on your instances, and the instances must be enabled for SSM\. For information about how to install the SSM agent, see [Working with SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) in the *AWS Systems Manager User Guide*\.
+ **Amazon EC2 instance roles** — You must attach the following Amazon EC2 instance roles to configure your database\.
  + You must attach the `AmazonSSMManagedInstanceCore` role to enable Systems Manager\. For more information, see [AWS Systems Manager identity\-based policy examples](https://docs.aws.amazon.com/systems-manager/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html)\.
  + You must attach the `CloudWatchAgentServerPolicy` to enable instance metrics and logs to be emitted through CloudWatch\. For more information, see [Create IAM Roles and Users for Use With CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)\.
  + You must attach the following IAM inline policy to the Amazon EC2 instance role to read the password stored in AWS Secrets Manager\. For more information about inline policies, see [Inline policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *AWS Identity and Access Management User Guide*\.

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "VisualEditor0",
                "Effect": "Allow",
                "Action": [
                    "secretsmanager:GetSecretValue"
                ],
                "Resource": "arn:aws:secretsmanager:*:*:secret:ApplicationInsights-*"
            }
        ]
    }
    ```
+ **AWS resource groups** — You must create a resource group that includes all of the associated AWS resources used by your application stack to onboard your applications to CloudWatch Application Insights\. This includes Amazon EC2 instances and Amazon EBS volumes running your SAP HANA database\.
+ **IAM permissions** — For non\-admin users, you must create an AWS Identity and Access Management \(IAM\) policy that allows Application Insights to create a service\-linked role, and attach it to your user identity\. For steps to attach the policy, see [IAM policy](appinsights-iam.md)\.
+ **Service\-linked role** — Application Insights uses AWS Identity and Access Management \(IAM\) service\-linked roles\. A service\-linked role is created for you when you create your first Application Insights application in the Application Insights console\. For more information, see [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)\.

## Set up your SAP HANA database for monitoring<a name="appinsights-tutorial-sap-hana-set-up"></a>

 To set up monitoring for your SAP HANA database, perform the following steps\. 

1. Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. From the left navigation pane, under **Insights**, choose **Application Insights**\.

1. The **Application Insights** page displays the list of applications that are monitored with Application Insights, and the monitoring status for each application\. In the upper right\-hand corner, choose **Add an application**\.

1. On the **Specify application details** page, from the dropdown list under **Resource group**, select the AWS resource group that contains your SAP HANA database resources\. If you haven't created a resource group for your application, you can create one by choosing **Create new resource group** under the **Resource group** dropdown\. For more information about creating resource groups, see the [https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html)

1. Under **Monitor CloudWatch Events**, select the check box to integrate Application Insights monitoring with CloudWatch Events to get insights from Amazon EBS, Amazon EC2, AWS CodeDeploy, Amazon ECS, AWS Health APIs and notifications, Amazon RDS, Amazon S3, and AWS Step Functions\.

1. Under **Integrate with AWS Systems Manager OpsCenter**, select the check box next to **Generate AWS Systems Manager OpsCenter OpsItems for remedial actions** to view and get notifications when problems are detected for the selected applications\. To track the operations that are performed to resolve operational work items, called OpsItems, that are related to your AWS resources, provide an SNS topic ARN\. 

1. You can optionally enter tags to help you identify and organize your resources\. CloudWatch Application Insights supports both tag\-based and AWS CloudFormation\-based resource groups, with the exception of Application Auto Scaling groups\. For more information, see [Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) in the *AWS Resource Groups and Tags User Guide*\. 

1. Choose **Next** to continue to set up monitoring\.

1. On the **Set up monitoring** page, the monitored components automatically detected by CloudWatch Application Insights are listed\. Choose **Next**\.

1. On the **Specify component details** page, enter the user name and password for your user\.

1. Choose **Next**\.

1. Review your application monitoring configuration, and choose **Submit**\.

1. The application details page opens, where you can view the **Application summary**, and the list of **Monitored components** and **Unmonitored components**\. If you select the radio button next to a component, you can also view the **Configuration history**, **Log patterns**, and any **Tags** that you have created\. When you submit your configuration, your account deploys all of the metrics and alarms for your SAP HANA system, which can take up to 2 hours\. 

## Manage monitoring of your SAP HANA database<a name="appinsights-tutorial-sap-hana-manage"></a>

You can manage user credentials, metrics, and log paths for your SAP HANA database by performing the following steps:

1. Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. From the left navigation pane, under **Insights**, choose **Application Insights**\.

1. The **Application Insights** page displays the list of applications that are monitored with Application Insights, and the monitoring status for each application\. 

1. Under **Monitored components**, select the radio button next to the component name\. Then, choose **Manage monitoring**\.

1. Under **EC2 instance group logs**, you can update the existing log path, log pattern set, and log group name\. In addition, you can add up to three additional **Application logs**\.

1. Under **Metrics**, you can choose the SAP HANA metrics according to your requirements\. SAP HANA metric names are prefixed with `hanadb`\. You can add up to 40 metrics per component\.

1. Under **HANA configuration**, enter the password and user name for the SAP HANA database\. This is the user name and password that Amazon CloudWatch agent uses to connect to the SAP HANA database\.

1. Under **Custom alarms**, you can add additional alarms to be monitored by CloudWatch Application Insights\.

1. Review your application monitoring configuration and choose **Submit**\. When you submit your configuration, your account updates all of the metrics and alarms for your SAP HANA system, which can take up to 2 hours\.

## Configure the alarm threshold<a name="appinsights-tutorial-sap-hana-configure-alarm-threshold"></a>

CloudWatch Application Insights automatically creates a Amazon CloudWatch metric for the alarm to watch, along with the threshold for that metric\. The alarm changes to the `ALARM ` state when the metric surpasses the threshold for a specified number of evaluation periods\. Note that these settings are not retained by Application Insights\.

To edit an alarm for a single metric, perform the following steps:

1. Navigate to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. In the left navigation pane, choose **Alarms**>**All alarms**\.

1. Select the radio button next to the alarm that was automatically created by CloudWatch Application Insights\. Then choose **Actions**, and select **Edit** from the dropdown menu\.

1. Edit the following parameters under **Metric**\.

   1. Under **Statistic**, choose one of the statistics or predefined percentiles, or specify a custom percentile\. For example, `p95.45`\.

   1. Under **Period**, choose the evaluations period for the alarm\. When you evaluate the alarm, each period is aggregated into one data point\.

1. Edit the following parameters under **Conditions**\.

   1. Choose whether the metric must be greater than, less than, or equal to the threshold\. 

   1. Specify the threshold value\.

1. Under **Additional configuration** edit the following parameters\.

   1. Under **Datapoints to alarm**, specify the number of data points, or evaluation periods, that must be in the `ALARM` state to initiate the alarm\. When the two values match, an alarm is created that enters `ALARM` state if the designated number of consecutive periods are exceeded\. To create an `m` out of `n` alarm, specify a lower value for the first data point than for the second\. For more information about evaluating alarms, see [Evaluating an alarm\.](https://docs.aws.amazon.com/AmazonCloudWatch/monitoring/AlarmThatSendsEmail.html#alarm-evaluation)\.

   1. Under **Missing data treatment**, choose the behavior of the alarm when some data points are missing\. For more information about missing data treatment, see [Configuring how CloudWatch alarms treat missing data](https://docs.aws.amazon.com/AmazonCloudWatch/monitoring/AlarmThatSendsEmail.html#alarms-and-missing-data)\.

   1. If the alarm uses a percentile as the monitored statistic, a **Percentiles with low samples** box appears\. Choose whether to evaluate or ignore cases with low sample rates\. If you choose **ignore \(maintain alarm state\)**, the current alarm state is always maintained when the sample size is too low\. For more information about percentiles with low samples, see [Percentile\-based CloudWatch alarms and low data samples](AlarmThatSendsEmail.md#percentiles-with-low-samples) \.

1. Choose **Next**\.

1. Under **Notification**, select an SNS topic to notify when the alarm is in `ALARM` state, `OK` state, or `INSUFFICIENT_DATA` state\.

1. Choose **Update alarm**\.

## View and troubleshoot SAP HANA problems detected by CloudWatch Application Insights<a name="appinsights-tutorial-sap-hana-troubleshooting"></a>

The following sections provide steps to help you resolve common troubleshooting scenarios that occur when you configure monitoring for SAP HANA on Application Insights\.

**Topics**
+ [SAP HANA database reaches memory allocation limit](#appinsights-tutorial-sap-hana-troubleshooting-memory)
+ [Disk full event](#appinsights-tutorial-sap-hana-troubleshooting-disk-full)
+ [SAP HANA backup stopped running](#appinsights-tutorial-sap-hana-troubleshooting-backup-stopped)

### SAP HANA database reaches memory allocation limit<a name="appinsights-tutorial-sap-hana-troubleshooting-memory"></a>

**Description**  
Your SAP application that is backed by an SAP HANA database malfunctions because of high memory pressure, leading to application performance degradation\.

**Resolution**  
You can identify the application layer that is causing the problem by checking the dynamically created dashboard, which shows the related metrics and log file snippets\. In the following example, the problem may be because of a large data load in the SAP HANA system\.

![\[Memory allocation exceeded.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-memory-allocation-1.png)

The used memory allocation exceeds the threshold of 80 percent of the total memory allocation limit\.

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-memory-allocation-2.png)

The log group shows the scheme `BNR-DATA` and table `IMDBMASTER_30003` ran out of memory\. In addition, the log group shows the exact time of the issue, current global location limit, shared memory, code size, and OOM reservation allocation size\.

![\[Log group text.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-memory-allocation-3.png)

### Disk full event<a name="appinsights-tutorial-sap-hana-troubleshooting-disk-full"></a>

**Description**  
Your SAP application that is backed by an SAP HANA database stops responding, which leads to an inability to access the database\.

**Resolution**  
You can identify the database layer that is causing the problem by checking the dynamically created dashboard, which shows the related metrics and log file snippets\. In the following example, the problem may be that the administrator failed to enable automatic log backup, which caused the sap/hana/log directory to fill up\.

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-disk-full-1.png)

The log group widget in the problem dashboard shows the `DISKFULL` event\.

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-disk-full-2.png)

### SAP HANA backup stopped running<a name="appinsights-tutorial-sap-hana-troubleshooting-backup-stopped"></a>

**Description**  
Your SAP application that is backed by an SAP HANA database has stopped working\.

**Resolution**  
You can identify the database layer that is causing the problem by checking the dynamically created dashboard, which shows the related metrics and log file snippets\. 

The log group widget in the problem dashboard shows the `ACCESS DENIED` event\. This includes additional information, such as the S3 bucket, the S3 bucket folder, and the S3 bucket Region\.

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-backup-stopped-2.png)

## Anomaly detection for SAP HANA<a name="appinsights-tutorial-sap-hana-troubleshooting-anomaly-detection"></a>

For specific SAP HANA metrics, such as the number of thread count, CloudWatch applies statistical and machine learning algorithms to define the threshold\. These algorithms continuously analyze the metrics of the SAP HANA database, determine normal baselines, and surface anomalies with minimal user intervention\. The algorithms generate an anomaly detection model, which generates a range of expected values that represent normal metric behavior\.

Anomaly detection algorithms account for the seasonality and trend changes of metrics\. The seasonality changes can be hourly, daily, or weekly, as shown in the following examples of the SAP HANA CPU usage\.

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/appinsights-anomaly-detection.png)

After you create a model, CloudWatch anomaly detection continuously evaluates the model and makes adjustments to it to ensure that is it as accurate as possible\. This includes retraining the model to adjust if the metric values evolve over time or experience sudden changes\. It also includes predictors to improve the models for metrics that are seasonal, spiky, or sparse\.

## Troubleshooting Application Insights for SAP HANA<a name="appinsights-tutorial-sap-hana-troubleshooting-health-dashboard"></a>

This section provides steps to help you resolve common errors returned by the Application Insights dashboard\. 

**Unable to add more than 60 monitor metrics**  
**Error returned**: `Component cannot have more than 60 monitored metrics.`

**Root cause**: `The current metric limit is 60 monitor metrics per component.`

**Resolution**: Remove metrics that are not necessary to adhere to the limit\.

**No SAP metrics or alarms appear after the onboarding process\.**  
**Error returned**: In AWS Systems Manager Run command, the `AWS-ConfigureAWSPackage` failed\. 

The output shows the following error:

```
Unable to find a host with system database, for more info rerun using -v
```

![\[Log group showing out of memory.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/app-insights-no-sap-metrics-2.png)

**Resolution**: The user name and password may be incorrect\. Verify that the user name and password are valid, and rerun the onboarding process\.
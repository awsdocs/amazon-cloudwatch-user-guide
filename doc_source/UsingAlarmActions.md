# Create alarms to stop, terminate, reboot, or recover an EC2 instance<a name="UsingAlarmActions"></a>

Using Amazon CloudWatch alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your EC2 instances\. You can use the stop or terminate actions to help you save money when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot those instances or recover them onto new hardware if a system impairment occurs\.

There are a number of scenarios in which you might want to automatically stop or terminate your instance\. For example, you might have instances dedicated to batch payroll processing jobs or scientific computing tasks that run for a period of time and then complete their work\. Rather than letting those instances sit idle \(and accrue charges\), you can stop or terminate them, which helps you to save money\. The main difference between using the stop and the terminate alarm actions is that you can easily restart a stopped instance if you need to run it again later\. You can also keep the same instance ID and root volume\. However, you cannot restart a terminated instance\. Instead, you must launch a new instance\.

You can add the stop, terminate, or reboot, actions to any alarm that is set on an Amazon EC2 per\-instance metric, including basic and detailed monitoring metrics provided by Amazon CloudWatch \(in the AWS/EC2 namespace\), in addition to any custom metrics that include the "InstanceId=" dimension, as long as the InstanceId value refers to a valid running Amazon EC2 instance\. You can also add the recover action to alarms that is set on any Amazon EC2 per\-instance metric except for `StatusCheckFailed_Instance`\.

To set up a CloudWatch alarm action that can reboot, stop, or terminate an instance, you must use a service\-linked IAM role, *AWSServiceRoleForCloudWatchEvents*\. The AWSServiceRoleForCloudWatchEvents IAM role enables AWS to perform alarm actions on your behalf\.

To create the service\-linked role for CloudWatch Events, use the following command:

```
aws iam create-service-linked-role --aws-service-name events.amazonaws.com
```

**Console support**  
You can create alarms using the CloudWatch console or the Amazon EC2 console\. The procedures in this documentation use the CloudWatch console\. For procedures that use the Amazon EC2 console, see [Create Alarms That Stop, Terminate, Reboot, or Recover an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingAlarmActions.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Permissions**  
If you are using an AWS Identity and Access Management \(IAM\) account to create or modify an alarm that performs EC2 actions or Systems Manager OpsItem actions, you must have the `iam:CreateServiceLinkedRole` permission\.

**Topics**
+ [Adding stop actions to Amazon CloudWatch alarms](#AddingStopActions)
+ [Adding terminate actions to Amazon CloudWatch alarms](#AddingTerminateActions)
+ [Adding reboot actions to Amazon CloudWatch alarms](#AddingRebootActions)
+ [Adding recover actions to Amazon CloudWatch alarms](#AddingRecoverActions)
+ [Viewing the history of triggered alarms and actions](#ViewAlarmHistory)

## Adding stop actions to Amazon CloudWatch alarms<a name="AddingStopActions"></a>

You can create an alarm that stops an Amazon EC2 instance when a certain threshold has been met\. For example, you may run development or test instances and occasionally forget to shut them off\. You can create an alarm that is triggered when the average CPU utilization percentage has been lower than 10 percent for 24 hours, signaling that it is idle and no longer in use\. You can adjust the threshold, duration, and period to suit your needs, plus you can add an SNS notification, so that you will receive an email when the alarm is triggered\.

Amazon EC2 instances that use an Amazon Elastic Block Store volume as the root device can be stopped or terminated, whereas instances that use the instance store as the root device can only be terminated\.

**To create an alarm to stop an idle instance using the Amazon CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **All alarms**\.

1. Choose **Create alarm**\.

1. Choose **Select Metric**\.

1. For **AWS namespaces**, choose **EC2**\.

1. Do the following:

   1. Choose **Per\-Instance Metrics**\.

   1. Select the check box in the row with the correct instance and the **CPUUtilization** metric\.

   1. Choose **Graphed metrics**\.

   1. For the statistic, choose **Average**\.

   1. Choose a period \(for example, **1 Hour**\)\.

   1. Choose **Select metric**\.

1. For the **Define Alarm** step, do the following:

   1. Under **Conditions**, choose **Static**\. 

   1. Under **Whenever CPUUtilization is**, choose **Lower**\.

   1. For **than**, type **10**\.

   1. Choose **Next**\.

   1. Under **Notification**, for **Send notification to**, choose an existing SNS topic or create a new one\.

      To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, Stop\_EC2\_Instance\)\. For **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

   1. Choose **Add EC2 Action**\.

   1. For **Alarm state trigger**, choose **In alarm**\. For **Take the following action**, choose **Stop this instance**\.

   1. Choose **Next**\.

   1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then choose **Next**\.

   1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Create alarm**\.

## Adding terminate actions to Amazon CloudWatch alarms<a name="AddingTerminateActions"></a>

You can create an alarm that terminates an EC2 instance automatically when a certain threshold has been met \(as long as termination protection is not enabled for the instance\)\. For example, you might want to terminate an instance when it has completed its work, and you don't need the instance again\. If you might want to use the instance later, you should stop the instance instead of terminating it\. For information about enabling and disabling termination protection for an instance, see [Enabling Termination Protection for an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_ChangingDisableAPITermination.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To create an alarm to terminate an idle instance using the Amazon CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. For the **Select Metric** step, do the following:

   1. Under **EC2 Metrics**, choose **Per\-Instance Metrics**\.

   1. Select the row with the instance and the **CPUUtilization** metric\.

   1. For the statistic, choose **Average**\.

   1. Choose a period \(for example, **1 Hour**\)\.

   1. Choose **Next**\.

1. For the **Define Alarm** step, do the following:

   1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, Terminate EC2 instance\) and a description of the alarm \(for example, Terminate EC2 instance when CPU is idle for too long\)\. Alarm names must contain only ASCII characters\.

   1. Under **Whenever**, for **is**, choose **<** and type **10**\. For **for**, type **24** consecutive periods\.

      A graphical representation of the threshold is shown under **Alarm Preview**\.

   1. Under **Notification**, for **Send notification to**, choose an existing SNS topic or create a new one\.

      To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, Terminate\_EC2\_Instance\)\. For **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

   1. Choose **EC2 Action**\.

   1. For **Whenever this alarm**, choose **State is ALARM**\. For **Take this action**, choose **Terminate this instance**\.

   1. Choose **Create Alarm**\.

## Adding reboot actions to Amazon CloudWatch alarms<a name="AddingRebootActions"></a>

You can create an Amazon CloudWatch alarm that monitors an Amazon EC2 instance and automatically reboots the instance\. The reboot alarm action is recommended for Instance Health Check failures \(as opposed to the recover alarm action, which is suited for System Health Check failures\)\. An instance reboot is equivalent to an operating system reboot\. In most cases, it takes only a few minutes to reboot your instance\. When you reboot an instance, it remains on the same physical host, so your instance keeps its public DNS name, private IP address, and any data on its instance store volumes\.

Rebooting an instance doesn't start a new instance billing hour, unlike stopping and restarting your instance\. For more information about rebooting an instance, see [Reboot Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-reboot.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Important**  
To avoid a race condition between the reboot and recover actions, avoid setting the same evaluation period for both a reboot alarm and a recover alarm\. We recommend that you set reboot alarms to three evaluation periods of one minute each\. 

**To create an alarm to reboot an instance using the Amazon CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. For the **Select Metric** step, do the following:

   1. Under **EC2 Metrics**, choose **Per\-Instance Metrics**\.

   1. Select the row with the instance and the **StatusCheckFailed\_Instance** metric\.

   1. For the statistic, choose **Minimum**\.

   1. Choose a period \(for example, **1 Minute**\) and choose **Next**\.

1. For the **Define Alarm** step, do the following:

   1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, Reboot EC2 instance\) and a description of the alarm \(for example, Reboot EC2 instance when health checks fail\)\. Alarm names must contain only ASCII characters\.

   1. Under **Whenever**, for **is**, choose **>** and type **0**\. For **for**, type **3** consecutive periods\.

      A graphical representation of the threshold is shown under **Alarm Preview**\.

   1. Under **Notification**, for **Send notification to**, choose an existing SNS topic or create a new one\.

      To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, Reboot\_EC2\_Instance\)\. For **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

   1. Choose **EC2 Action**\.

   1. For **Whenever this alarm**, choose **State is ALARM**\. For **Take this action**, choose **Reboot this instance**\.

   1. Choose **Create Alarm**\.

## Adding recover actions to Amazon CloudWatch alarms<a name="AddingRecoverActions"></a>

You can create an Amazon CloudWatch alarm that monitors an Amazon EC2 instance and automatically recovers the instance if it becomes impaired due to an underlying hardware failure or a problem that requires AWS involvement to repair\. Terminated instances cannot be recovered\. A recovered instance is identical to the original instance, including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata\.

When the `StatusCheckFailed_System` alarm is triggered, and the recover action is initiated, you will be notified by the Amazon SNS topic that you chose when you created the alarm and associated the recover action\. During instance recovery, the instance is migrated during an instance reboot, and any data that is in\-memory is lost\. When the process is complete, information is published to the SNS topic you've configured for the alarm\. Anyone who is subscribed to this SNS topic will receive an email notification that includes the status of the recovery attempt and any further instructions\. You will notice an instance reboot on the recovered instance\.

The recover action can be used only with `StatusCheckFailed_System`, not with `StatusCheckFailed_Instance`\.

Examples of problems that cause system status checks to fail include:
+ Loss of network connectivity
+ Loss of system power
+ Software issues on the physical host
+ Hardware issues on the physical host that impact network reachability

The recover action is supported only on some instance types\. For more information about supported instance types and other requirements, see [ Recover your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recover.html) and [ Requirements](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recover.html#requirements-for-recovery)\.

**Important**  
To avoid a race condition between the reboot and recover actions, avoid setting the same evaluation period for both a reboot alarm and a recover alarm\. We recommend that you set recover alarms to two evaluation periods of one minute each and reboot alarms to three evaluation periods of one minute each\.

**To create an alarm to recover an instance using the Amazon CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. For the **Select Metric** step, do the following:

   1. Under **EC2 Metrics**, choose **Per\-Instance Metrics**\.

   1. Select the row with the instance and the **StatusCheckFailed\_System** metric\.

   1. For the statistic, choose **Minimum**\.

   1. Choose a period \(for example, **1 Minute**\)\.
**Important**  
To avoid a race condition between the reboot and recover actions, avoid setting the same evaluation period for both a reboot alarm and a recover alarm\. We recommend that you set recover alarms to two evaluation periods of one minute each\.

   1. Choose **Next**\.

1. For the **Define Alarm** step, do the following:

   1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, Recover EC2 instance\) and a description of the alarm \(for example, Recover EC2 instance when health checks fail\)\. Alarm names must contain only ASCII characters\.

   1. Under **Whenever**, for **is**, choose **>** and type **0**\. For **for**, type **2** consecutive periods\.

   1. Under **Notification**, for **Send notification to**, choose an existing SNS topic or create a new one\.

      To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, Recover\_EC2\_Instance\)\. For **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

   1. Choose **EC2 Action**\.

   1. For **Whenever this alarm**, choose **State is ALARM**\. For **Take this action**, choose **Recover this instance**\.

   1. Choose **Create Alarm**\.

## Viewing the history of triggered alarms and actions<a name="ViewAlarmHistory"></a>

You can view alarm and action history in the Amazon CloudWatch console\. Amazon CloudWatch keeps the last two weeks' worth of alarm and action history\.

**To view the history of triggered alarms and actions**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms** and select an alarm\.

1. To view the most recent state transition along with the time and metric values, choose **Details**\.

1. To view the most recent history entries, choose **History**\.
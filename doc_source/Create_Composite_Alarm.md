# Create a composite alarm<a name="Create_Composite_Alarm"></a>

Composite alarms determine their states by monitoring the states of other alarms\. You can use composite alarms to reduce alarm noise\. For example, you can create a composite alarm where the underlying metric alarms go into ALARM when they meet specific conditions\. You then can set up your composite alarm to go into ALARM and send you notifications when the underlying metric alarms go into ALARM by configuring the underlying metric alarms never to take actions\. Currently, composite alarms can take the following actions:
+ Notify SNS topics
+ Create OpsItems in AWS Systems Manager Ops Center
+ Create incidents in AWS Systems Manager Incident Manager

If you are not using CloudWatch cross\-account observability, the composite alarm can monitor alarms in the current account\.

**Note**  
All of the underlying alarms in your composite alarm must be in the same account and the same Region as your composite alarm\. However, if you set up a composite alarm in a CloudWatch cross\-account observability monitoring account, the underlying alarms can watch metrics in different source accounts and in the monitoring account itself\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\.  
A single composite alarm can monitor 100 underlying alarms, and 150 composite alarms can monitor a single underlying alarm\.

**Rule expressions**

All composite alarms contain rule expressions\. Rule expressions tell composite alarms which other alarms to monitor and determine their states from\. Rule expressions can refer to metric alarms and composite alarms\. When you reference an alarm in a rule expression, you designate a function to the alarm that determines which of the following three states the alarm will be in:
+ ALARM

  ALARM \("alarm\-name or alarm\-ARN"\) is TRUE if the alarm is in ALARM state\.
+ OK

  OK \("alarm\-name or alarm\-ARN"\) is TRUE if the alarm is in OK state\.
+ INSUFFICIENT\_DATA

  INSUFFICIENT\_DATA \(“alarm\-name or alarm\-ARN"\) is TRUE if the named alarm is in INSUFFICIENT\_DATA state\.

**Note**  
TRUE always evaluates to TRUE, and FALSE always evaluates to FALSE\.

**Example expressions**

The request parameter `AlarmRule` supports the use of the logical operators `AND`, `OR`, and `NOT`, so you can combine multiple functions into a single expressions\. The following example expressions show how you can configure the underlying alarms in your composite alarm: 
+ `ALARM(CPUUtilizationTooHigh) AND ALARM(DiskReadOpsTooHigh)`

  The expression specifies that the composite alarm goes into `ALARM` only if `CPUUtilizationTooHigh` and `DiskReadOpsTooHigh` are in `ALARM`\.
+ `ALARM(CPUUtilizationTooHigh) AND NOT ALARM(DeploymentInProgress)`

  The expression specifies that the composite alarm goes into `ALARM` if `CPUUtilizationTooHigh` is in `ALARM` and `DeploymentInProgress` is not in `ALARM`\. This is an example of a composite alarm that reduces alarm noise during a deployment window\.
+ `(ALARM(CPUUtilizationTooHigh) OR ALARM(DiskReadOpsTooHigh)) AND OK(NetworkOutTooHigh)`

  The expression specifies that the composite alarm goes into `ALARM` if `(ALARM(CPUUtilizationTooHigh)` or `(DiskReadOpsTooHigh)` is in `ALARM` and `(NetworkOutTooHigh)` is in `OK`\. This is an example of a composite alarm that reduces alarm noise by not sending you notifications when either of the underlying alarms aren’t in `ALARM` while a network issue is occurring\.

## Create a composite alarm<a name="Create_Composite_Alarm_How_To"></a>

**To create a composite alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, and then choose **In alarm**\.

1. From the list of alarms, select the check box next to each of the existing alarms that you want to reference in your rule expression, and then choose **Create composite alarm**\.

1. Under **Specify composite alarm conditions**, specify the rule expression for your new composite alarm\.
**Note**  
Automatically, the alarms that you selected from the list of alarms are listed in the **Conditions** box\. By default, the `ALARM` function has been designated to each of your alarms, and each of your alarms is joined by the logical operator `OR`\.

   You can use the following substeps to modify your rule expression:

   1. You can change the required state for each of your alarms from `ALARM` to `OK` or `INSUFFICENT_DATA`\.

   1. You can change the logical operator in your rule expression from `OR` to `AND` or `NOT`, and you can add parentheses to group your functions\.

   1. You can include other alarms in your rule expression or delete alarms from your rule expression\.

   **Example: Rule expression with conditions**

   ```
   (ALARM("CPUUtilizationTooHigh") OR 
   ALARM("DiskReadOpsTooHigh")) AND 
   OK("NetworkOutTooHigh")
   ```

   In the example rule expression where the composite alarm goes into `ALARM` when ALARM \("CPUUtilizationTooHigh" or ALARM\("DiskReadOpsTooHigh"\) is in `ALARM` at the same time as OK\("NetworkOutTooHigh"\) is in `OK`\.

1. When finished, choose **Next**\.

1. Under **Configure actions**, you can choose from the following:

   For ***Notification***
   + **Select an exisiting SNS topic**, **Create a new SNS topic**, or **Use a topic ARN** to define the SNS topic that will receive the notification\.
   + **Add notification**, so your alarm can send multiple notifications for the same alarm state or different alarm states\.
   + **Remove** to stop your alarm from sending notifications or taking actions\.

   For ***Systems Manager action***
   + **Add Systems Manager action**, so your alarm can perform an SSM action when it goes into ALARM\.

   To learn more about Systems Manager actions, see [Configuring CloudWatch to create OpsItems from alarms](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter-create-OpsItems-from-CloudWatch-Alarms.html) in the *AWS Systems Manager User Guide* and [Incident creation](https://docs.aws.amazon.com/incident-manager/latest/userguide/incident-creation.html) in the *Incident Manager User Guide*\. To create an alarm that performs an SSM Incident Manager action, you must have the correct permissions\. For more information, see [Identity\-based policy examples for AWS Systems Manager Incident Manager](https://docs.aws.amazon.com/incident-manager/latest/userguide/security_iam_id-based-policy-examples.html) in the *Incident Manager User Guide*\.

1. When finished, choose **Next**\.

1. Under **Add name and description**, enter an alarm name and *optional* description for your new composite alarm\. The alarm name must contain only UTF\-8 characters, and can't contain ASCII control characters

1. When finished, choose **Next**\.

1. Under **Preview and create**, confirm your information, and then choose **Create composite alarm**\.
**Note**  
You can create a cycle of composite alarms, where one composite alarm and another composite alarm depend on each other\. If you find yourself in this scenario, your composite alarms stop being evaluated, and you can't delete your composite alarms because they're dependent on each other\. The easiest way to break the cycle of dependecy between your composite alarms is to change the function `AlarmRule` in one of your composite alarms to `False`\.

## Composite alarm action suppression<a name="Create_Composite_Alarm_Suppression"></a>

 With composite alarm action suppression, you define alarms as suppressor alarms\. Suppressor alarms prevent composite alarms from taking actions\. For example, you can specify a suppressor alarm that represents the status of a supporting resource\. If the supporting resource is down, the suppressor alarm prevents the composite alarm from sending notifications\. Composite alarm action suppression helps you reduce alarm noise, so you spend less time managing your alarms and more time focusing on your operations\. 

 You specify suppressor alarms when you configure composite alarms\. Any alarm can function as a suppressor alarm\. When a suppressor alarm changes states from `OK` to `ALARM`, its composite alarm stops taking actions\. When a suppressor alarm changes states from `ALARM` to `OK`, its composite alarm resumes taking actions\. 

### `WaitPeriod` and `ExtensionPeriod`<a name="Create_Composite_Alarm_Suppression_Wait_Extension"></a>

 When you specify a suppressor alarm, you set the parameters `WaitPeriod` and `ExtensionPeriod`\. These parameters prevent composite alarms from taking actions unexpectedly while suppressor alarms change states\. Use `WaitPeriod` to compensate for any delays that can occur when a suppressor alarm changes from `OK` to `ALARM`\. For example, if a suppressor alarm changes from `OK` to `ALARM` within 60 seconds, set `WaitPeriod` to 60 seconds\. 

![\[Actions suppression within WaitPeriod\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example1border.png)

 In the image, the composite alarm changes from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t8\. This gives the suppressor alarm time to change states from `OK` to `ALARM` at t4 before it suppresses the composite alarm's actions when the `WaitPeriod` expires at t8\. 

 Use `ExtensionPeriod` to compensate for any delays that can occur when a composite alarm changes to `OK` following a suppressor alarm changing to `OK`\. For example, if a composite alarm changes to `OK` within 60 seconds of a suppressor alarm changing to `OK`, set `ExtensionPeriod` to 60 seconds\. 

![\[Actions suppression within ExtensionPeriod\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example2border.png)

 In the image, the suppressor alarm changes from `ALARM` to `OK` at t2\. An `ExtensionPeriod` starts at t2 and ends at t8\. This gives the composite alarm time to change from `ALARM` to `OK` before the `ExtensionPeriod` expires at t8\. 

 Composite alarms don't take actions when `WaitPeriod` and `ExtensionPeriod` become active\. Composite alarms take actions that are based on their currents states when `ExtensionPeriod` and `WaitPeriod` become inactive\.  We recommend that you set the value for each parameter to 60 seconds, as CloudWatch evaluates metric alarms every minute\. You can set the parameters to any integer in seconds\. 

 The following examples describe in more detail how `WaitPeriod` and `ExtensionPeriod` prevent composite alarms from taking actions unexpectedly\. 

**Note**  
 In the following examples, `WaitPeriod` is configured as 2 time units, and `ExtensionPeriod` is configured as 3 time units\. 

#### Examples<a name="example_scenarios"></a>

 ** Example 1: Actions are not suppressed after `WaitPeriod` ** 

![\[first example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example3border.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t4, so it can prevent the composite alarm from taking actions\. After the `WaitPeriod` expires at t4, the composite alarm takes its actions because the suppressor alarm is still in `OK`\. 

 ** Example 2: Actions are suppressed by alarm before `WaitPeriod` expires ** 

![\[second example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example4border.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t4\. This gives the suppressor alarm time to change states from `OK` to `ALARM` at t3\. Because the suppressor alarm changes states from `OK` to `ALARM` at t3, the `WaitPeriod` that started at t2 is discarded, and the suppressor alarm now stops the composite alarm from taking actions\. 

 ** Example 3: State transition when actions are suppressed by `WaitPeriod` ** 

![\[third example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example5border.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t4\. This gives the suppressor alarm time to change states\. The composite alarm changes back to `OK` at t3, so the `WaitPeriod` that started at t2 is discarded\. A new `WaitPeriod` starts at t3 and ends at t5\. After the new `WaitPeriod` expires at t5, the composite alarm takes its actions\. 

 ** Example 4: State transition when actions are suppressed by alarm ** 

![\[fourth example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/cwasexamplefourborder.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. The suppressor alarm is already in `ALARM`\. The suppressor alarm stops the composite alarm from taking actions\. 

 ** Example 5: Actions are not suppressed after `ExtensionPeriod` ** 

![\[fifth example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/example7border.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t4\. This gives the suppressor alarm time to change states from `OK` to `ALARM` at t3 before it suppresses the composite alarm's actions until t6\. Because the suppressor alarm changes states from `OK` to `ALARM` at t3, the `WaitPeriod` that started at t2 is discarded\. At t6, the suppressor alarm changes to `OK`\. An `ExtensionPeriod` starts at t6 and ends at t9\. After the `ExtensionPeriod` expires, the composite alarm takes its actions\. 

 ** Example 6: State transition when actions are suppressed by `ExtensionPeriod` ** 

![\[sixth example of action suppression\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/cwasexamplesixrborder.png)

 In the image, the composite alarm changes states from `OK` to `ALARM` at t2\. A `WaitPeriod` starts at t2 and ends at t4\. This gives the suppressor alarm time to change states from `OK` to `ALARM` at t3 before it suppresses the composite alarm's actions until t6\. Because the suppressor alarm changes states from `OK` to `ALARM` at t3, the `WaitPeriod` that started at t2 is discarded\. At t6, the suppressor alarm changes back to `OK`\. An `ExtensionPeriod` starts at t6 and ends at t8\. When the composite alarm changes back to `OK` at t7, the `ExtensionPeriod` is discarded, and a new `WaitPeriod` starts at t7 and ends at t9\. After the new `WaitPeriod` expires, the composite alarm can take its actions\. 

**Tip**  
 If you replace the action suppressor alarm, any active `WaitPeriod` or `ExtensionPeriod` is discarded\. 
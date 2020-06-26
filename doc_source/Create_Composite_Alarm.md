# Creating a Composite Alarm<a name="Create_Composite_Alarm"></a>

Composite alarms are alarms that determine their alarm state by watching the alarm states of other alarms\.

Using composite alarms can help you reduce alarm noise\. If you set up a composite alarm to notify you of state changes, but set up the underlying metric alarms to not send notifications themselves, you will be notified only when the alarm state of the composite alarm changes\. For example, you could create metric alarms based on both CPU utilization and disk read operations, and specify for these alarms to never take actions\. You could then create a composite alarm that goes into ALARM state and notifies you only when both of those metric alarms are in ALARM state\.

In a composite alarm, all underlying alarms must be in the same AWS Region and the same account\.

Currently, the only alarm actions that can be taken by composite alarms are notifying SNS topics\.

**Rule Expressions**

Each composite alarms includes a *rule expression*, which specifies which other alarms are to be evaluated to determine the composite alarm's state\. For each alarm that you reference in the rule expression, you designate a function that specifies whether that alarm needs to be in ALARM state, OK state, or INSUFFICIENT\_DATA state\. You can use operators \(AND, OR and NOT\) to combine multiple functions in a single expression\. You can use parenthesis to logically group the functions in your expression\.

A rule expression can refer to both metric alarms and other composite alarms\.

Functions can include the following:
+ `ALARM("alarm-name or alarm-ARN")` is TRUE if the named alarm is in ALARM state\.
+ `OK("alarm-name or alarm-ARN")` is TRUE if the named alarm is in OK state\.
+ `INSUFFICIENT_DATA("alarm-name or alarm-ARN")` is TRUE if the named alarm is in INSUFFICIENT\_DATA state\.
+ `TRUE` always evaluates to TRUE\.
+ `FALSE` always evaluates to FALSE\.

TRUE and FALSE are useful for testing a complex `AlarmRule` structure, and for testing your alarm actions\.

The following are some examples of `AlarmRule`:
+ `ALARM(CPUUtilizationTooHigh) AND ALARM(DiskReadOpsTooHigh)` specifies that the composite alarm goes into ALARM state only if both CPUUtilizationTooHigh and DiskReadOpsTooHigh alarms are in ALARM state\.
+ `ALARM(CPUUtilizationTooHigh) AND NOT ALARM(DeploymentInProgress)` specifies that the alarm goes to ALARM state if CPUUtilizationTooHigh is in ALARM state and DeploymentInProgress is not in ALARM state\. This example reduces alarm noise during a known deployment window\.
+ `(ALARM(CPUUtilizationTooHigh) OR ALARM(DiskReadOpsTooHigh)) AND OK(NetworkOutTooHigh)` goes into ALARM state if CPUUtilizationTooHigh OR DiskReadOpsTooHigh is in ALARM state, and if NetworkOutTooHigh is in OK state\. This provides another example of using a composite alarm to prevent noise\. This rule ensures that you are not notified with an alarm action on high CPU or disk usage if a known network problem is also occurring\.

An `AlarmRule` expression can specify as many as 100 "child" alarms\. The `AlarmRule` expression can have as many as 500 elements\. Elements are child alarms, TRUE or FALSE statements, and parentheses\. A pair of parentheses counts as one element\.

A rule expression must contain at least one child alarm or at least one TRUE statement or FALSE statement\.

**To create a composite alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. In the list of alarms, select the check boxes next to each of the existing alarms that you want to reference in your new composite alarm\. Then choose **Create composite alarm**\.

1. In the **Conditions** box, specify the rule expression that the composite alarm will use\. Initially, the alarms you selected are listed, joined by the OR logical operator\. Each of these alarms has the ALARM state specified\.

   You can modify the alarm conditions for the composite alarm that you are creating:

   1. For each underlying alarm listed, you can change the required state from ALARM to OK or INSUFFICENT\_DATA\.

   1. You can change each OR operator to AND or NOT\. You can also add parentheses to group the logical operators\.

   1. You can add more alarms to the composite alarm conditions\. You can also delete alarms currently listed in the **Conditions** box\.

   For example, you could specify the following conditions to create a composite alarm that goes into ALARM state if `CPUUtilizationTooHigh` OR `DiskReadOpsTooHigh` is in ALARM state, at the same time that `NetworkOutTooHigh` is in OK state\.

   ```
   (ALARM("CPUUtilizationTooHigh") OR 
   ALARM("DiskReadOpsTooHigh")) AND 
   OK("NetworkOutTooHigh")
   ```

1. When you are satisfied with the alarm conditions, choose **Next**\.

1. Under **Notification**, select an SNS topic to notify when the composite alarm changes state\.

   To have the alarm not send notifications or take any actions at all, choose **Remove**\.

   To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\.

1. When finished, choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then choose **Next**\.

1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Create alarm**\.

**Note**  
It is possible to create a loop or cycle of composite alarms, where composite alarm A depends on composite alarm B, and composite alarm B also depends on composite alarm A\. In this scenario, you can't delete any composite alarm that is part of the cycle because there is always still a composite alarm that depends on that alarm that you want to delete\.  
To get out of such a situation, you must break the cycle by changing the rule of one of the composite alarms in the cycle to remove a dependency that creates the cycle\. The simplest change to make to break a cycle is to change the `AlarmRule` of one of the alarms to `False`\.   
Additionally, the evaluation of composite alarms stops if CloudWatch detects a cycle in the evaluation path\. 
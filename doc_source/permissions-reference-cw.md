# Amazon CloudWatch permissions reference<a name="permissions-reference-cw"></a>

The following table lists each CloudWatch API operation and the corresponding actions for which you can grant permissions to perform the action\. You specify the actions in the policy's `Action` field, and you specify a wildcard character \(\*\) as the resource value in the policy's `Resource` field\.

You can use AWS\-wide condition keys in your CloudWatch policies to express conditions\. For a complete list of AWS\-wide keys, see [AWS Global and IAM Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `cloudwatch:` prefix followed by the API operation name\. For example: `cloudwatch:GetMetricData`, `cloudwatch:ListMetrics`, or `cloudwatch:*` \(for all CloudWatch actions\)\.

**Topics**
+ [CloudWatch API operations and required permissions for actions](#cw-permissions-table)
+ [CloudWatch Contributor Insights API operations and required permissions for actions](#cw-contributor-insights-permissions-table)
+ [CloudWatch Events API operations and required permissions for actions](#cwe-permissions-table)
+ [CloudWatch Logs API operations and required permissions for actions](#cwl-permissions-table)
+ [Amazon EC2 API operations and required permissions for actions](#cw-ec2-permissions-table)
+ [Amazon EC2 Auto Scaling API operations and required permissions for actions](#cw-as-permissions-table)

## CloudWatch API operations and required permissions for actions<a name="cw-permissions-table"></a>


| CloudWatch API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  [DeleteAlarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DeleteAlarms.html)  |  `cloudwatch:DeleteAlarms` Required to delete an alarm\.  | 
|  [DeleteDashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DeleteDashboards.html)  |  `cloudwatch:DeleteDashboards` Required to delete a dashboard\.  | 
|  [DeleteMetricStream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DeleteMetricStream.html)  |  `cloudwatch:DeleteMetricStream` Required to delete a metric stream\.  | 
|  [DescribeAlarmHistory](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarmHistory.html)  |  `cloudwatch:DescribeAlarmHistory` Required to view alarm history\. To retrieve information about composite alarms, your `cloudwatch:DescribeAlarmHistory` permission must have a `*` scope\. You can't return information about composite alarms if your `cloudwatch:DescribeAlarmHistory` permission has a narrower scope\.  | 
|  [DescribeAlarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarms.html)  |  `cloudwatch:DescribeAlarms` Required to retrieve information about alarms\. To retrieve information about composite alarms, your `cloudwatch:DescribeAlarms` permission must have a `*` scope\. You can't return information about composite alarms if your `cloudwatch:DescribeAlarms` permission has a narrower scope\.  | 
|  [DescribeAlarmsForMetric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarmsForMetric.html)  |  `cloudwatch:DescribeAlarmsForMetric` Required to view alarms for a metric\.  | 
|  [DisableAlarmActions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DisableAlarmActions.html)  |  `cloudwatch:DisableAlarmActions` Required to disable an alarm action\.  | 
|  [EnableAlarmActions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_EnableAlarmActions.html)  |  `cloudwatch:EnableAlarmActions` Required to enable an alarm action\.  | 
|  [GetDashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetDashboard.html)  |  `cloudwatch:GetDashboard` Required to display data about existing dashboards\.  | 
|  [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)  |  `cloudwatch:GetMetricData` Required to graph metric data in the CloudWatch console, to retrieve large batches of metric data, and perform metric math on that data\.  | 
|  [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html)  |  `cloudwatch:GetMetricStatistics` Required to view graphs in other parts of the CloudWatch console and in dashboard widgets\.  | 
|  [GetMetricStream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStream.html)  |  `cloudwatch:GetMetricStream` Required to view information about a metric stream\.  | 
|  [GetMetricWidgetImage](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricWidgetImage.html)  |  `cloudwatch:GetMetricWidgetImage` Required to retrieve a snapshot graph of one or more CloudWatch metrics as a bitmap image\.  | 
|  [ListDashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListDashboards.html)  |  `cloudwatch:ListDashboards` Required to view the list of CloudWatch dashboards in your account\.  | 
|  [ListMetrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListMetrics.html)  |  `cloudwatch:ListMetrics` Required to view or search metric names within the CloudWatch console and in the CLI\. Required to select metrics on dashboard widgets\.  | 
|  [ListMetricStreams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListMetricStreams.html)  |  `cloudwatch:ListMetricStreams` Required to view or search the list of metric streams in the account\.  | 
|  [PutCompositeAlarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutCompositeAlarm.html)  |  `cloudwatch:PutCompositeAlarm` Required to create a composite alarm\. To create a composite alarm, your `cloudwatch:PutCompositeAlarm` permission must have a `*` scope\. You can't return information about composite alarms if your `cloudwatch:PutCompositeAlarm` permission has a narrower scope\.  | 
|  [PutDashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutDashboard.html)  |  `cloudwatch:PutDashboard` Required to create a dashboard or update an existing dashboard\.  | 
|  [PutMetricAlarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html)  |  `cloudwatch:PutMetricAlarm` Required to create or update an alarm\.  | 
|  [PutMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html)  |  `cloudwatch:PutMetricData` Required to create metrics\.  | 
|  [PutMetricStream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricStream.html)  |  `cloudwatch:PutMetricStream` Required to create a metric stream\.  | 
|  [SetAlarmState](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_SetAlarmState.html)  |  `cloudwatch:SetAlarmState` Required to manually set an alarm's state\.  | 
|  [StartMetricStreams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_StartMetricStreams.html)  |  `cloudwatch:StartMetricStreams` Required to start the flow of metrics in a metric stream\.  | 
|  [StopMetricStreams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_StartMetricStreams.html)  |  `cloudwatch:StopMetricStreams` Required to temporarily stop the flow of metrics in a metric stream\.  | 
|  [TagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_TagResource.html)  |  `cloudwatch:TagResource` Required to add or update tags on CloudWatch resources such as alarms and Contributor Insights rules\.  | 
|  [UntagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_UntagResource.html)  |  `cloudwatch:UntagResource` Required to remove tags from CloudWatch resources \.  | 

## CloudWatch Contributor Insights API operations and required permissions for actions<a name="cw-contributor-insights-permissions-table"></a>

**Important**  
When you grant a user the `cloudwatch:PutInsightRule` permission, by default that user can create a rule that evaluates any log group in CloudWatch Logs\. You can add IAM policy conditions that limit these permissions for a user to include and exclude specific log groups\. For more information, see [Using condition keys to limit Contributor Insights users' access to log groups](iam-cw-condition-keys-contributor.md)\.


| CloudWatch Contributor Insights API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  [DeleteInsightRules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DeleteInsightRules.html)  |  `cloudwatch:DeleteInsightRules` Required to delete Contributor Insights rules\.  | 
|  [DescribeInsightRules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeInsightRules.html)  |  `cloudwatch:DescribeInsightRules` Required to view the Contributor Insights rules in your account\.  | 
|  [EnableInsightRules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_EnableInsightRules.html)  |  `cloudwatch:EnableInsightRules` Required to enable Contributor Insights rules\.  | 
|  [GetInsightRuleReport](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetInsightRuleReport.html)  |  `cloudwatch:GetInsightRuleReport` Required to retrieve time series data and other statistics collectd by Contributor Insights rules\.  | 
|  [PutInsightRule](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutInsightRule.html)  |  `cloudwatch:PutInsightRule` Required to create Contributor Insights rules\. See the **Important** note at the beginning of this table\.  | 

## CloudWatch Events API operations and required permissions for actions<a name="cwe-permissions-table"></a>


| CloudWatch Events API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  [DeleteRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_DeleteRule.html)  |  `events:DeleteRule` Required to delete a rule\.  | 
|  [DescribeRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_DescribeRule.html)  |  `events:DescribeRule` Required to list the details about a rule\.  | 
|  [DisableRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_DisableRule.html)  |  `events:DisableRule` Required to disable a rule\.  | 
|  [EnableRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_EnableRule.html)  |  `events:EnableRule` Required to enable a rule\.  | 
|  [ListRuleNamesByTarget](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_ListRuleNamesByTarget.html)  |  `events:ListRuleNamesByTarget` Required to list rules associated with a target\.  | 
|  [ListRules](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_ListRules.html)  |  `events:ListRules` Required to list all rules in your account\.  | 
|  [ListTargetsByRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_ListTargetsByRule.html)  |  `events:ListTargetsByRule` Required to list all targets associated with a rule\.  | 
|  [PutEvents](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_PutEvents.html)  |  `events:PutEvents` Required to add custom events that can be matched to rules\.  | 
|  [PutRule](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_PutRule.html)  |  `events:PutRule` Required to create or update a rule\.  | 
|  [PutTargets](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_PutTargets.html)  |  `events:PutTargets` Required to add targets to a rule\.  | 
|  [RemoveTargets](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_RemoveTargets.html)  |  `events:RemoveTargets` Required to remove a target from a rule\.  | 
|  [TestEventPattern](https://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/API_TestEventPattern.html)  |  `events:TestEventPattern` Required to test an event pattern against a given event\.  | 

## CloudWatch Logs API operations and required permissions for actions<a name="cwl-permissions-table"></a>


| CloudWatch Logs API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  [CancelExportTask](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CancelExportTask.html)  |  `logs:CancelExportTask` Required to cancel a pending or running export task\.  | 
|  [CreateExportTask](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html)  |  `logs:CreateExportTask` Required to export data from a log group to an Amazon S3 bucket\.  | 
|  [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html)  |  `logs:CreateLogGroup` Required to create a new log group\.  | 
|  [CreateLogStream](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogStream.html)  |  `logs:CreateLogStream` Required to create a new log stream in a log group\.  | 
|  [DeleteDestination](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteDestination.html)  |  `logs:DeleteDestination` Required to delete a log destination and disables any subscription filters to it\.  | 
|  [DeleteLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteLogGroup.html)  |  `logs:DeleteLogGroup` Required to delete a log group and any associated archived log events\.  | 
|  [DeleteLogStream](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteLogStream.html)  |  `logs:DeleteLogStream` Required to delete a log stream and any associated archived log events\.  | 
|  [DeleteMetricFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteMetricFilter.html)  |  `logs:DeleteMetricFilter` Required to delete a metric filter associated with a log group\.  | 
|  [DeleteQueryDefinition](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteQueryDefinition.html)  |  `logs:DeleteQueryDefinition` Required to delete a saved query definition in CloudWatch Logs Insights\.  | 
|  [DeleteResourcePolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteResourcePolicy.html)  |  `logs:DeleteResourcePolicy` Required to delete a CloudWatch Logs resource policy\.  | 
|  [DeleteRetentionPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteRetentionPolicy.html)  |  `logs:DeleteRetentionPolicy` Required to delete a log group's retention policy\.  | 
|  [DeleteSubscriptionFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DeleteSubscriptionFilter.html)  |  `logs:DeleteSubscriptionFilter` Required to delete the subscription filter associated with a log group\.  | 
|  [DescribeDestinations](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeDestinations.html)  |  `logs:DescribeDestinations` Required to view all destinations associated with the account\.  | 
|  [DescribeExportTasks](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeExportTasks.html)  |  `logs:DescribeExportTasks` Required to view all export tasks associated with the account\.  | 
|  [DescribeLogGroups](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeLogGroups.html)  |  `logs:DescribeLogGroups` Required to view all log groups associated with the account\.  | 
|  [DescribeLogStreams](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeLogStreams.html)  |  `logs:DescribeLogStreams` Required to view all log streams associated with a log group\.  | 
|  [DescribeMetricFilters](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeMetricFilters.html)  |  `logs:DescribeMetricFilters` Required to view all metrics associated with a log group\.  | 
|  [DescribeQueryDefinitions](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeQueryDefinitions.html)  |  `logs:DescribeQueryDefinitions` Required to see the list of saved query definitions in CloudWatch Logs Insights\.  | 
|  [DescribeQueries](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeQueries.html)  |  `logs:DescribeQueries` Required to see the list of CloudWatch Logs Insights queries that are scheduled, executing, or have recently excecuted\.  | 
|  [DescribeResourcePolicies](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeResourcePolicies.html)  |  `logs:DescribeResourcePolicies` Required to view a list of CloudWatch Logs resource policies\.  | 
|  [DescribeSubscriptionFilters](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_DescribeSubscriptionFilters.html)  |  `logs:DescribeSubscriptionFilters` Required to view all subscription filters associated with a log group\.  | 
|  [FilterLogEvents](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_FilterLogEvents.html)  |  `logs:FilterLogEvents` Required to sort log events by log group filter pattern\.  | 
|  [GetLogEvents](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogEvents.html)  |  `logs:GetLogEvents` Required to retrieve log events from a log stream\.  | 
|  [GetLogGroupFields](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogGroupFields.html)  |  `logs:GetLogGroupFields` Required to retrieve the list of fields that are included in the log events in a log group\.  | 
|  [GetLogRecord](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogRecord.html)  |  `logs:GetLogRecord` Required to retrieve the details from a single log event\.  | 
|  [GetQueryResults](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetQueryResults.html)  |  `logs:GetQueryResults` Required to retrieve the results of CloudWatch Logs Insights queries\.  | 
|  [ListTagsLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_ListTagsLogGroup.html)  |  `logs:ListTagsLogGroup` Required to list the tags associated with a log group\.  | 
|  [PutDestination](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutDestination.html)  |  `logs:PutDestination` Required to create or update a destination log stream \(such as an Kinesis stream\)\.  | 
|  [PutDestinationPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutDestinationPolicy.html)  |  `logs:PutDestinationPolicy` Required to create or update an access policy associated with an existing log destination\.  | 
|  [PutLogEvents](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutLogEvents.html)  |  `logs:PutLogEvents` Required to upload a batch of log events to a log stream\.  | 
|  [PutMetricFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutMetricFilter.html)  |  `logs:PutMetricFilter` Required to create or update a metric filter and associate it with a log group\.  | 
|  [PutQueryDefinition](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutQueryDefinition.html)  |  `logs:PutQueryDefinition` Required to save a query in CloudWatch Logs Insights\.  | 
|  [PutResourcePolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutResourcePolicy.html)  |  `logs:PutResourcePolicy` Required to create a CloudWatch Logs resource policy\.  | 
|  [PutRetentionPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutRetentionPolicy.html)  |  `logs:PutRetentionPolicy` Required to set the number of days to keep log events \(retention\) in a log group\.  | 
|  [PutSubscriptionFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html)  |  `logs:PutSubscriptionFilter` Required to create or update a subscription filter and associate it with a log group\.  | 
|  [StartQuery](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_StartQuery.html)  |  `logs:StartQuery` Required to start CloudWatch Logs Insights queries\.  | 
|  [StopQuery](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_StopQuery.html)  |  `logs:StopQuery` Required to stop a CloudWatch Logs Insights query that is in progress\.  | 
|  [TagLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_TagLogGroup.html)  |  `logs:TagLogGroup` Required to add or update log group tags\.  | 
|  [TestMetricFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_TestMetricFilter.html)  |  `logs:TestMetricFilter` Required to test a filter pattern against a sampling of log event messages\.  | 

## Amazon EC2 API operations and required permissions for actions<a name="cw-ec2-permissions-table"></a>


| Amazon EC2 API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  [DescribeInstanceStatus](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html)  |  `ec2:DescribeInstanceStatus` Required to view EC2 instance status details\.  | 
|  [DescribeInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html)  |  `ec2:DescribeInstances` Required to view EC2 instance details\.  | 
|  [RebootInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RebootInstances.html)  |  `ec2:RebootInstances` Required to reboot an EC2 instance\.  | 
|  [StopInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_StopInstances.html)  |  `ec2:StopInstances` Required to stop an EC2 instance\.  | 
|  [TerminateInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_TerminateInstances.html)  |  `ec2:TerminateInstances` Required to terminate an EC2 instance\.  | 

## Amazon EC2 Auto Scaling API operations and required permissions for actions<a name="cw-as-permissions-table"></a>


| Amazon EC2 Auto Scaling API operations | Required permissions \(API actions\) | 
| --- | --- | 
|  Scaling  |  `autoscaling:Scaling` Required to scale an Auto Scaling group\.  | 
|  Trigger  |  `autoscaling:Trigger` Required to trigger an Auto Scaling action\.  | 
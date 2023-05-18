# Create a Contributor Insights rule<a name="ContributorInsights-CreateRule"></a>

You can create rules to analyze log data\. Any logs in JSON or Common Log Format \(CLF\) can be evaluated\. This includes your custom logs that follow one of these formats and logs from AWS services such as Amazon VPC flow logs, Amazon Route 53 DNS query logs, Amazon ECS container logs, and logs from AWS CloudTrail, Amazon SageMaker, Amazon RDS, AWS AppSync and API Gateway\.

In a rule, when you specify field names or values, all matching is case sensitive\.

You can use built\-in sample rules when you create a rule or you can create your own rule from scratch\. Contributor Insights includes sample rules for the following types of logs:
+ Amazon API Gateway logs
+ Amazon Route 53 public DNS query logs
+ Amazon Route 53 resolver query logs
+ CloudWatch Container Insights logs
+ VPC flow logs

If you are signed in to an account that is set up as a monitoring account in CloudWatch cross\-account observability, you can create Contributor Insights rules for log groups in the source accounts linked to this monitoring account, in addition to creating rules for log groups in the monitoring account\. You can also set up a single rule that monitors log groups in different accounts\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\.

**Important**  
When you grant a user the `cloudwatch:PutInsightRule` permission, by default that user can create a rule that evaluates any log group in CloudWatch Logs\. You can add IAM policy conditions that limit these permissions for a user to include and exclude specific log groups\. For more information, see [Using condition keys to limit Contributor Insights users' access to log groups](iam-cw-condition-keys-contributor.md)\.

**To create a rule using a built\-in sample rule**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Insights**, **Contributor Insights**\.

1. Choose **Create rule**\.

   

1.  For **Select log group\(s\)**, select the log group\(s\) that you want your rule to monitor\. You can select as many as 20 log groups\. If you are signed in to a monitoring account that is set up for CloudWatch cross\-account observability, you can select log groups in source accounts, and you can also set up a single rule to analyze log groups in different accounts\. 

   1.  \(Optional\) To select all log groups that have names beginning with a specific string, choose the **Select by prefix match** dropdown, and then enter the prefix\. If this is a monitoring account, you can optionally select the accounts to search in, otherwise all accounts are selected\. 
**Note**  
 You incur charges for each log event that matches your rule\. If you choose the **Select by prefix match** dropdown, be aware of how many log groups the prefix can match\. If you search more log groups than you want, you might incur unexpected charges\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\. 

1. For **Rule type**, choose **Sample rule**\. Then choose **Select sample rule** and select the rule\.

1.  The sample rule has filled out the **Log format**, **Contribution**, **Filters**, and **Aggregate on** fields\. You can adjust those values, if you like\. 

1. Choose **Next**\.

1. For **Rule name**, enter a name\. Valid characters are A\-Z, a\-z, 0\-9, \(hyphen\), \(underscore\), and \(period\)\.

1. Choose whether to create the rule in a disabled or enabled state\. If you choose to enable it, the rule immediately starts analyzing your data\. You incur costs when you run enabled rules\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

   Contributor Insights analyzes only new log events after a rule is created\. A rule cannot process logs events that were previously processed by CloudWatch Logs\.

1. \(Optional\) For **Tags**, add one or more key\-value pairs as tags for this rule\. Tags can help you identify and organize your AWS resources and track your AWS costs\. For more information, see [Tagging your Amazon CloudWatch resources](CloudWatch-Tagging.md)\.

1. Choose **Create**\.

**To create a rule from scratch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Contributor Insights**\.

1. Choose **Create rule**\.

   

1.  For **Select log group\(s\)**, select the log group\(s\) that you want your rule to monitor\. You can select as many as 20 log groups\. If you are signed in to a monitoring account that is set up for CloudWatch cross\-account observability, you can select log groups in source accounts, and you can also set up a single rule to analyze log groups in different accounts\. 

   1.  \(Optional\) To select all log groups that have names beginning with a specific string, choose the **Select by prefix match** dropdown, and then enter the prefix\. 
**Note**  
 You incur charges for each log event that matches your rule\. If you choose the **Select by prefix match** dropdown, be aware of how many log groups the prefix can match\. If you search more log groups than you want, you might incur unexpected charges\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\. 

1. For **Rule type**, choose **Custom rule**\.

1. For **Log format**, choose **JSON** or **CLF**\.

1. You can finish creating the rule by using the wizard or by choosing the **Syntax** tab and specifying your rule syntax manually\.

   To continue using the wizard, do the following:

   1. For **Contribution**, **Key**, enter a contributor type that you want to report on\. The report displays the top\-N values for this contributor type\.

      Valid entries are any log field that has values\. Examples include **requestId**, **sourceIPaddress**, and **containerID**\.

      For information about finding the log field names for the logs in a certain log group, see [Finding Log Fields](#finding_log_fields)\.

      Keys larger than 1 KB are truncated to 1KB\.

   1. \(Optional\) Choose **Add new key** to add more keys\. You can include as many as four keys in a rule\. If you enter more than one key, the contributors in the report are defined by unique value combinations of the keys\. For example, if you specify three keys, each unique combination of values for the three keys is counted as a unique contributor\.

   1. \(Optional\) If you want to add a filter that narrows the scope of your results, choose **Add filter**\. For **Match**, enter the name of the log field that you want to filter on\. For **Condition**, choose a comparison operator, and enter a value that you want to filter for\. 

      You can add as many as four filters in a rule\. Multiple filters are joined by AND logic, so only log events that match all filters are evaluated\.
**Note**  
Arrays that follow comparison operators, such as `In`, `NotIn`, or `StartsWith`, can include as many as 10 string values\. For more information about the Contributor Insights rules syntax, see [Contributor Insights rule syntax](ContributorInsights-RuleSyntax.md)\.

   1. For **Aggregate on**, choose **Count** or **Sum**\. Choosing **Count** causes the contributor ranking to be based on the number of occurrences\. Choosing **Sum** causes the ranking to be based on the aggregated sum of the values of the field that you specify for **Contribution**, **Value**\.

1. To enter your rule as a JSON object instead of using the wizard, do the following:

   1. Choose the **Syntax** tab\.

   1. In **Rule body**, enter the JSON object for your rule\. For information about rule syntax, see [Contributor Insights rule syntax](ContributorInsights-RuleSyntax.md)\. 

1. Choose **Next**\.

1. For **Rule name**, enter a name\. Valid characters are A\-Z, a\-z, 0\-9, "\-", "\_', and "\."\.

1. Choose whether to create the rule in a disabled or enabled state\. If you choose to enable it, the rule immediately starts analyzing your data\. You incur costs when you run enabled rules\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

   Contributor Insights analyzes only new log events after a rule is created\. A rule cannot process logs events that were previously processed by CloudWatch Logs\.

1. \(Optional\) For **Tags**, add one or more key\-value pairs as tags for this rule\. Tags can help you identify and organize your AWS resources and track your AWS costs\. For more information, see [Tagging your Amazon CloudWatch resources](CloudWatch-Tagging.md)\.

1. Choose **Next**\.

1. Confirm the settings that you entered, and choose **Create rule**\.

You can disable, enable, or delete rules that you have created\.

**To enable, disable, or delete a rule in Contributor Insights**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Contributor Insights**\.

1. In the list of rules, select the check box next to a single rule\.

   Built\-in rules are created by AWS services and can't be edited, disabled, or deleted\.

1. Choose **Actions**, and then choose the option you want\.<a name="finding_log_fields"></a>

**Finding log fields**

When you create a rule, you need to know the names of fields in the log entries in a log group\.

**To find the log fields in a log group**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, under **Logs**, choose **Insights**\.

1. Above the query editor, select one or more log groups to query\.

   When you select a log group, CloudWatch Logs Insights automatically detects fields in the data in the log group and displays them in the right pane in **Discovered fields**\. 
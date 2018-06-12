# AWS Trusted Advisor Metrics and Dimensions<a name="trusted-advisor-metricscollected"></a>

Trusted Advisor sends metrics to CloudWatch\. For more information, see [Creating Trusted Advisor Metrics in CloudWatch](http://docs.aws.amazon.com/awssupport/latest/user/cloudwatch-metrics-ta.html) in the *AWS Support User Guide*\.

The `AWS/TrustedAdvisor` namespace includes the following metrics\.

## Trusted Advisor Check\-Level Metrics<a name="trusted-advisor-checklevel-metrics"></a>


| Metric | Description | 
| --- | --- | 
| RedResources |  The number of resources identifed by a Trusted Advisor check that are in a “red” \(ERROR\) state\.  | 
| YellowResources |  The number of resources identifed by a Trusted Advisor check that are in a “yellow” \(WARN\) state\.   | 

## Trusted Advisor Category\-Level Metrics<a name="trusted-advisor-categorylevel-metrics"></a>


| Metric | Description | 
| --- | --- | 
| GreenChecks |  The number of Trusted Advisor checks in a "green" \(OK\) state\.  | 
| RedChecks |  The number of Trusted Advisor checks in a "red" \(ERROR\) state\.  | 
| YellowChecks |  The number of Trusted Advisor checks in a "yellow" \(WARN\) state\.  | 

## Trusted Advisor Service\-Level Metrics<a name="trusted-advisor-servicelevel-metrics"></a>


| Metric | Description | 
| --- | --- | 
| ServiceLimitUsage |  The percentage of resource utilization against a service limit\.  | 

## Dimensions for Check\-Level Metrics<a name="trusted-advisor-checklevel-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
| CheckName |  The name of a Trusted Advisor check\. For more information about checks, see [Cost Optimization]( https://aws.amazon.com/premiumsupport/trustedadvisor/best-practices/#cost-optimizing)\.  | 

## Dimensions for Category\-Level Metrics<a name="trusted-advisor-categorylevel-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
| Category |  The name of a Trusted Advisor check category\. For more information about check categories, see [Trusted Advisor Best Practices \(Checks\)](https://aws.amazon.com/premiumsupport/trustedadvisor/best-practices)\.  | 

## Dimensions for Service Limit Metrics<a name="trusted-advisor-servicelevel-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
| Region |  The region for a given service limit\.  | 
| ServiceName |  The name of a service\.  | 
| ServiceLimit |  The name of service limit\. For more information about checks, see [Service Limits Check Questions](https://aws.amazon.com/premiumsupport/ta-faqs/#service-limits-check-questions)\.  | 
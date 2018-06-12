# AWS WAF Metrics and Dimensions<a name="waf-metricscollected"></a>

AWS WAF sends data to CloudWatch every minute\. For more information, see [Testing Web ACLs](http://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html) in the *AWS WAF Developer Guide*\.

## AWS WAF Metrics<a name="waf-metrics"></a>

The `WAF` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| `AllowedRequests` |  The number of allowed web requests\. Reporting criteria: There is a nonzero value Valid statistics: Sum  | 
| `BlockedRequests` |  The number of blocked web requests\. Reporting criteria: There is a nonzero value Valid statistics: Sum  | 
| `CountedRequests` |  The number of counted web requests\. Reporting criteria: There is a nonzero value A counted web request is one that matches all of the conditions in a particular rule\. Counted web requests are typically used for testing\. Valid statistics: Sum  | 
| `PassedRequests` |  The number of passed requests for a rule group\.  Reporting criteria: There is a nonzero value Passed requests are requests that did not match any rule contained in the rule group\. Valid statistics: Sum  | 

## AWS WAF Dimensions<a name="waf-metricdimensions"></a>

AWS WAF for CloudFront can use the following dimension combinations:
+ Rule, WebACL
+ RuleGroup, WebACL
+ Rule, RuleGroup

AWS WAF for Application Load Balancer can use the following dimension combinations:
+ Region, Rule, WebACL
+ Region, RuleGroup, WebACL
+ Region, Rule, RuleGroup


| Dimension | Description | 
| --- | --- | 
| `Rule` |  One of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/waf-metricscollected.html)  | 
| `RuleGroup` |  The metric name of the RuleGroup\.  | 
| `WebACL` |  The metric name of the WebACL\.  | 
| `Region` |  The region of the application load balancer\.  | 
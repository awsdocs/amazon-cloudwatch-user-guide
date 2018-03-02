# AWS WAF Dimensions<a name="waf-metricdimensions"></a>

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
| `Rule` |  One of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/waf-metricdimensions.html)  | 
| `RuleGroup` |  The metric name of the RuleGroup\.  | 
| `WebACL` |  The metric name of the WebACL\.  | 
| `Region` |  The region of the application load balancer\.  | 
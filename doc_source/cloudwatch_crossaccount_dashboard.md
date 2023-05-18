# CloudWatch cross\-account observability dashboard<a name="cloudwatch_crossaccount_dashboard"></a>

If you have multiple AWS accounts, you can set up *CloudWatch cross\-account observability* and then create rich cross\-account dashboards in your monitoring accounts\. You can seamlessly search, visualize, and analyze your metrics, logs, and traces without account boundaries\.

For more information about setting up CloudWatch cross\-account observability, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\.

With CloudWatch cross\-account observability, you can do the following in a dashboard in a monitoring account:
+ Search, view, and create graphs of metrics that reside in source accounts\. A single graph can include metrics from multiple accounts\.
+ Create alarms in the monitoring account that watch metrics in source accounts\.
+ View the log events from log groups located in source accounts, and run CloudWatch Logs Insights queries of log groups in source accounts\. A single CloudWatch Logs Insights query in a monitoring account can query multiple log groups in multiple source accounts at once\.
+ View nodes from source accounts in service maps in both X\-Ray and CloudWatch ServiceLens\. You can then filter the map to specific source accounts\.

When you are signed in to a monitoring account, a blue **Monitoring account** badge appears at the top right of every page that supports CloudWatch cross\-account observability functionality\.
# Query your metrics with CloudWatch Metrics Insights<a name="query_with_cloudwatch-metrics-insights"></a>

CloudWatch Metrics Insights is a powerful high\-performance SQL query engine that you can use to query your metrics at scale\. You can identify trends and patterns within all of your CloudWatch metrics in real time\.

You can also set alarms on any Metrics Insights queries that return a single time series\. This can be especially useful to create alarms that watch aggregated metrics across a fleet of your infrastructure or applications\. Create the alarm once, and it dynamically adjusts as resources are added to or removed from the fleet\.

You can perform a Metrics Insights query using the CloudWatch console, or by using `GetMetricData` or `PutDashboard` using the AWS CLI or the AWS SDKs\. Queries run in the console are free of charge\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

When you use the CloudWatch console, you can choose from a wide variety of pre\-built sample queries and also create your own queries\. When you create a query, you can use both a builder view that interactively prompts you and lets you browse through your existing metrics and dimensions to easily build a query, and an editor view to write queries from scratch, edit the queries you build in the builder view, and edit sample queries to customize them\.

With Metrics Insights you can run queries at scale\. By using the **GROUP BY** clause, you can flexibly group your metrics in real time into separate time series per specific dimension value, based on your use cases\. Because Metrics Insights queries include an **ORDER BY** ability, you can use Metrics Insights to make "Top N" type queries that can scan millions of metrics in your account, and return the top 10 most CPU consuming instances, for example, to help you pinpoint and remedy latency issues in your applications\.

**Topics**
+ [Build your queries](cloudwatch-metrics-insights-buildquery.md)
+ [Metrics Insights query components and syntax](cloudwatch-metrics-insights-querylanguage.md)
+ [Create alarms on Metrics Insights queries](cloudwatch-metrics-insights-alarms.md)
+ [Use Metrics Insights queries with metric math](cloudwatch-metrics-insights-math.md)
+ [SQL inference](cloudwatch-metrics-insights-inference.md)
+ [Metrics Insights sample queries](cloudwatch-metrics-insights-queryexamples.md)
+ [Metrics Insights limits](cloudwatch-metrics-insights-limits.md)
+ [Metrics Insights glossary](cloudwatch-metrics-insights-glossary.md)
+ [Troubleshooting Metrics Insights](cloudwatch-metrics-insights-troubleshooting.md)
# Prometheus metric type conversion by the CloudWatch Agent<a name="ContainerInsights-Prometheus-metrics-conversion"></a>

The Prometheus client libraries offer four core metric types: 
+ Counter
+ Gauge
+ Summary
+ Histogram

The CloudWatch agent supports the counter, gauge, and summary metric types\. Support for histogram metrics is planned for an upcoming release\.

 The Prometheus metrics with the unsupported histogram metric type are dropped by the CloudWatch agent\. For more information, see [Logging Dropped Prometheus Metrics](ContainerInsights-Prometheus-troubleshooting-EKS.md#ContainerInsights-Prometheus-troubleshooting-droppedmetrics)\.

**Gauge metrics**

A Prometheus gauge metric is a metric that represents a single numerical value that can arbitrarily go up and down\. The CloudWatch agent scrapes gauge metrics and send these values out directly\.

**Counter metrics**

A Prometheus counter metric is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero\. The CloudWatch agent calculates a delta from the previous scrape and sends the delta value as the metric value in the log event\. So the CloudWatch agent will start to produce one log event from the second scrape and continue with subsequent scrapes, if any\.

**Summary metrics**

A Prometheus summary metric is a complex metric type which is represented by multiple data points\. It provides a total count of observations and a sum of all observed values\. It calculates configurable quantiles over a sliding time window\.

The sum and count of a summary metric are cumulative, but the quantiles are not\. The following example shows the variance of quantiles\.

```
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 7.123e-06
go_gc_duration_seconds{quantile="0.25"} 9.204e-06
go_gc_duration_seconds{quantile="0.5"} 1.1065e-05
go_gc_duration_seconds{quantile="0.75"} 2.8731e-05
go_gc_duration_seconds{quantile="1"} 0.003841496
go_gc_duration_seconds_sum 0.37630427
go_gc_duration_seconds_count 9774
```

The CloudWatch agent handles the sum and count of a summary metric in the same way as it handles counter metrics, as described in the previous section\. The CloudWatch agent preserves the quantile values as they are originally reported\.
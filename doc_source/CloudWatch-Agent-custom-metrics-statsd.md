# Retrieve Custom Metrics with StatsD<a name="CloudWatch-Agent-custom-metrics-statsd"></a>

You can retrieve custom metrics from your applications or services using the CloudWatch agent with the StatsD protocol\. StatsD is supported on both Linux servers and servers running Windows Server\. CloudWatch supports the following StatsD format:

```
MetricName:value|type|@sample_rate|#tag1:
  value,tag1...
```
+ `MetricName` – A string with no colons, bars, \# characters, or @ characters\.
+ `value` – This can be either integer or float\.
+ `type` – Specify `c` for counter, `g` for gauge, `ms` for timer, `h` for histogram, or `s` for set\.
+ `sample_rate` – \(Optional\) A float between 0 and 1, inclusive\. Use only for counter, histogram, and timer metrics\. The default is 1 \(sampling 100% of the time\)\.
+ `tags` – \(Optional\) A comma\-separated list of tags\. StatsD tags are similar to dimensions in CloudWatch\. Use colons for key/value tags, such as `env:prod`\.

You can use any StatsD client that follows this format to send the metrics to the CloudWatch agent\. For more information about some of the available StatsD clients, see the [StatsD client page on GitHub](https://github.com/etsy/statsd/wiki#client-implementations)\. 

To collect these custom metrics, add a **"statsd": \{\}** line to the **metrics\_collected** section of the agent configuration file\. You can add this line manually\. If you use the wizard to create the configuration file, it is done for you\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

The StatsD default configuration works for most users\. There are three optional fields you can add to the **statsd** section of the agent configuration file as needed:
+ **service\_address:** The service address to which the CloudWatch agent should listen\. The format is `ip:port`\. If you omit the IP address, the agent listens on all available interfaces\. Only the UDP format is supported, so you do not need to specify a UDP prefix\. 

  The default is `:8125`\.
+ **metrics\_collection\_interval:** How often in seconds that the StatsD plugin runs and collects metrics\. The default is 10 seconds\. The range is 1 to 172,000\.
+ **metrics\_aggregation\_interval:** How often in seconds CloudWatch aggregates metrics into single data points\. The default is 60 seconds\.

  For example, if `metrics_collection_interval` is 10 and `metrics_aggregation_interval` is 60, CloudWatch collects data every 10 seconds\. After each minute, the six data readings from that minute are aggregated into a single data point, which is sent to CloudWatch\.

  The range is 0 to 172,000\. Setting `metrics_aggregation_interval` to 0 disables the aggregation of StatsD metrics\.

The following is an example of the **statsd** section of the agent configuration file, using the default port and custom collection and aggregation intervals\.

```
{
   "metrics":{
      "metrics_collected":{
         "statsd":{
            "service_address":":8125",
            "metrics_collection_interval":60,
            "metrics_aggregation_interval":300
         }
      }
   }
}
```
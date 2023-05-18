# Collect network performance metrics<a name="CloudWatch-Agent-network-performance"></a>

EC2 instances running on Linux that use the Elastic Network Adapter \(ENA\) publish network performance metrics\. Version 1\.246396\.0 and later of the CloudWatch agent enable you to import these network performance metrics into CloudWatch\. When you import these network performance metrics into CloudWatch, they are charged as CloudWatch custom metrics\.

For more information about the ENA driver, see [ Enabling enhanced networking with the Elastic Network Adapter \(ENA\) on Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking-ena.html) and [ Enabling enhanced networking with the Elastic Network Adapter \(ENA\) on Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/enhanced-networking-ena.html)\.

How you set up the collection of network performance metrics differs on Linux servers and Windows servers\.

The following table lists these network performance metrics enabled by the ENA adapter\. When the CloudWatch agent imports these metrics into CloudWatch from Linux instances, it prepends `ethtool_` at the beginning of each of these metric names\.


| Metric | Description | 
| --- | --- | 
|  Name on Linux servers: `bw_in_allowance_exceeded` Name on Windows servers: `Aggregate inbound BW allowance exceeded`  |  The number of packets queued and/or dropped because the inbound aggregate bandwidth exceeded the maximum for the instance\.\. This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 
|   Name on Linux servers: `bw_out_allowance_exceeded` Name on Windows servers: `Aggregate outbound BW allowance exceeded` |  The number of packets queued and/or dropped because the outbound aggregate bandwidth exceeded the maximum for the instance\. This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 
|  Name on Linux servers:`conntrack_allowance_available`  |  Reports the number of tracked connections that can be established by the instance before hitting the Connections Tracked allowance of that instance type\. This metric is available only on Nitro\-based EC2 instances using the Linux driver for Elastic Network Adapter \(ENA\) starting from version 2\.8\.1 This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 
|  Name on Linux servers:`conntrack_allowance_exceeded` Name on Windows servers: `Connection tracking allowance exceeded` |  The number of packets dropped because connection tracking exceeded the maximum for the instance and new connections could not be established\. This can result in packet loss for traffic to or from the instance\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 
|  Name on Linux servers: `linklocal_allowance_exceeded` Name on Windows servers: `Link local packet rate allowance exceeded`  |  The number of packets dropped because the PPS of the traffic to local proxy services exceeded the maximum for the network interface\. This impacts traffic to the DNS service, the Instance Metadata Service, and the Amazon Time Sync Service\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 
|  Name on Linux servers: `pps_allowance_exceeded` Name on Windows servers: `PPS allowance exceeded` |  The number of packets queued and/or dropped because the bidirectional PPS exceeded the maximum for the instance\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](#CloudWatch-Agent-network-performance) Unit: None  | 

## Linux setup<a name="CloudWatch-Agent-network-performance-Linux"></a>

On Linux servers, the *ethtool plugin* enables you to import the network performance metrics into CloudWatch\.

ethtool is a standard Linux utility that can collect statistics about Ethernet devices on Linux servers\. The statistics it collects depend on the network device and driver\. Examples of these statistics include `tx_packets`, `rx_bytes`, `tx_errors`, and `align_errors`\. When you use the ethtool plugin with the CloudWatch agent, you can also import these statistics into CloudWatch, along with the EC2 network performance metrics listed earlier in this section\.

When the CloudWatch agent imports metrics into CloudWatch, it adds an `ethtool_` prefix to the names of all imported metrics\. So the standard ethtool statistic `rx_bytes` is called `ethtool_rx_bytes` in CloudWatch, and the EC2 network performance metric `bw_in_allowance_exceeded` is called `ethtool_bw_in_allowance_exceeded` in CloudWatch\.

On Linux servers, to import ethtool metrics, add an `ethtool` section to the `metrics_collected` section of the CloudWatch agent configuration file\. The `ethtool` section can include the following subsections:
+ **interface\_include**— Including this section causes the agent to collect metrics from only the interfaces that have names listed in this section\. If you omit this section, metrics are collected from all Ethernet interfaces that aren't listed in `interface_exclude`\.

  The default ethernet interface is `eth0`\.
+ **interface\_exclude**— If you include this section, list the Ethernet interfaces that you don't want to collect metrics from\.

  The ethtool plugin always ignores loopback interfaces\.
+ **metrics\_include**— This section lists the metrics to import into CloudWatch\. It can include both standard statistics collected by ethtool and Amazon EC2 high\-resolution network metrics\.

The following example displays part of the CloudWatch agent configuration file\. This configuration collects the standard ethtool metrics `rx_packets` and `tx_packets`, and the Amazon EC2 network performance metrics from only the `eth1` interface\.

For more information about the CloudWatch agent configuration file, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

```
"metrics": {
    "append_dimensions": {
      "InstanceId": "${aws:InstanceId}"
    },
    "metrics_collected": {
      "ethtool": {
        "interface_include": [
          "eth1"
        ],
        "metrics_include": [
          "rx_packets",
          "tx_packets",
          "bw_in_allowance_exceeded",
          "bw_out_allowance_exceeded",
          "conntrack_allowance_exceeded",
          "linklocal_allowance_exceeded",
          "pps_allowance_exceeded"
         ]
      }
   }
}
```

## Windows setup<a name="CloudWatch-Agent-network-performance-Windows"></a>

On Windows servers, the network performance metrics are available through Windows Performance Counters, which the CloudWatch agent already collects metrics from\. So you do not need a plugin to collect these metrics from Windows servers\.

The following is a sample configuration file to collect network performance metrics from Windows\. For more information about editing the CloudWatch agent configuration file, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

```
{
    "metrics": {
        "append_dimensions": {
            "InstanceId": "${aws:InstanceId}"
        },
        "metrics_collected": {
            "ENA Packets Shaping": {
                "measurement": [
                    "Aggregate inbound BW allowance exceeded",
                    "Aggregate outbound BW allowance exceeded",
                    "Connection tracking allowance exceeded",
                    "Link local packet rate allowance exceeded",
                    "PPS allowance exceeded"
                ],
                "metrics_collection_interval": 60,
                "resources": [
                    "*"
                ]
            }
        }
    }
}
```

## Viewing network performance metrics<a name="CloudWatch-view-ENA-metrics"></a>

After importing network performance metrics into CloudWatch, you can view these metrics as time series graphs, and create alarms that can watch these metrics and notify you if they breach a threshold that you specify\. The following procedure shows how to view ethtool metrics as a time series graph\. For more information about setting alarms, see [ Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md)\.

Because all of these metrics are aggregate counters, you can use CloudWatch metric math functions such as `RATE(METRICS())` to calculate the rate for these metrics in graphs or use them to set alarms\. For more information about metric math functions, see [Use metric math](using-metric-math.md)\.

**To view network performance metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose the namespace for the metrics collected by the agent\. By default, this is **CWAgent**, but you may have specified a different namespace in the CloudWatch agent configuration file\.

1. Choose a metric dimension \(for example, **Per\-Instance Metrics**\)\.

1. The **All metrics** tab displays all metrics for that dimension in the namespace\. You can do the following:

   1. To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To sort the table, use the column heading\.

   1. To filter by resource, choose the resource ID, and then choose **Add to search**\.

   1. To filter by metric, choose the metric name, and then choose **Add to search**\.

1. \(Optional\) To add this graph to a CloudWatch dashboard, choose **Actions**, and then choose **Add to dashboard**\.
# Choosing a city\-network maximum limit<a name="IMCityNetworksMaximum"></a>

Amazon CloudWatch Internet Monitor can monitor your application traffic for all the locations where clients access your application resources, and all the ASNs \(typically internet service providers\) that they access your application through—the city\-networks for your application internet traffic\. You choose a percentage of application traffic to monitor when you create your monitor, which you can update at any time by editing the monitor\.

In addition to setting a traffic percentage, you can also set a maximum limit for the number of city\-networks monitored, to help prevent unexpected costs in your bill\. This is useful, for example, if your traffic patterns vary widely\. Billing costs increase for each city\-network that is monitored, after the first 100 city\-networks, which are included \(across all monitors per account\)\. If you set a city\-networks maximum limit, it caps the number if city\-networks that Internet Monitor monitors for your application, regardless of the percentage of traffic that you choose to monitor\.

You only pay for the number of city\-networks that are actually monitored\. The city\-network maximum limit that you choose lets you set a cap on the total that can be included when Internet Monitor monitors traffic with your monitor\. You can change the maximum limit at any time by editing your monitor\. 

To explore options, on the [Pricing calculator for CloudWatch](https://calculator.aws/#/addService/CloudWatch) page, scroll down to Internet Monitor\. For more information on Internet Monitor pricing, see the Internet Monitor section on the [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing/) page\.

To help you decide on a maximum city\-networks limit to select, consider how much traffic you want to monitor for your application\. The following Internet Monitor metrics can help you analyze your traffic usage and coverage after you create your monitor: `CityNetworksMonitored`, `TrafficMonitoredPercent`, and one or more of the `CityNetworksForNNPercentTraffic` metrics, where *NN* is a percentage value that is one of the following: 25, 50, 90, 95, 99, or 100\. \(To review definitions for these metrics, and all other Internet Monitor metrics, see [Using CloudWatch Metrics with Amazon CloudWatch Internet Monitor](CloudWatch-IM-view-cw-tools-metrics-dashboard.md)\.\)

To see an overview graph of your internet traffic coverage, go to the **Traffic insights** tab on the CloudWatch dashboard and see **City\-networks traffic monitoring**\. The graph shown in the section displays the actual number of city\-networks that are monitored for your application, as well as the maximum city\-networks limit to set in your monitor if you want to monitor 100% of internet traffic for your application\.

To explore your options in more detail, you can use the Internet Monitor metrics, as described in the following examples\. These examples show how to select a maximum city\-networks limit that is best for you, depending on the breadth of application internet traffic coverage you want\. Using the [queries for Internet Monitor metrics in CloudWatch Metrics](CloudWatch-IM-view-cw-tools-metrics-dashboard.md) can help you understand more about your application internet traffic coverage\.

As an example, say that you've set a monitoring maximum limit of 100 city\-networks and that your application is accessed by clients across 2637 city\-networks\. In CloudWatch Metrics, you'd see the following Internet Monitor metrics returned:

```
CityNetworksMonitored 100
TrafficMonitoredPercent  12.5
CityNetworksFor90PercentTraffic  2143
CityNetworksFor100PercentTraffic  2637
```

From this example, you can see that you're currently monitoring 12\.5% of your internet traffic, with the maximum limit set to 100 city\-networks\. If you want to monitor 90% of your traffic, the next metric provides information about that: `CityNetworksFor90PercentTraffic` indicates that you would need to monitor 2,143 city\-networks for 90% coverage\. To do that, you would update your monitor and set the maximum city\-networks limit to 2,143\.

Similarly, say you'd like to have 100% internet traffic monitoring for your application\. The next metric, `CityNetworksFor100PercentTraffic`, indicates that to do this, you should update your monitor to set the maximum city\-networks limit to 2,637\.

If you now set the maximum to 5,000 city\-networks, since that's greater than 2,637, you see the following metrics returned:

```
CityNetworksMonitored 2637
TrafficMonitoredPercent  100
CityNetworksFor90PercentTraffic  2143
CityNetworksFor100PercentTraffic  2637
```

From these metrics, you can see that with the higher limit, you monitor all 2,637 city\-networks, which is 100% of your internet traffic\.

To update your monitor and change the maximum city\-networks limit, see [Edit or delete an Internet Monitor monitor](CloudWatch-IM-get-started.edit-monitor.md)\.

The maximum that you set for the number of city\-networks helps to make sure that your bill is predictable\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing/)\. You can also learn how different values for the number of city\-networks monitored can affect your bill by using the CloudWatch price calculator\. To explore options, on the [Pricing calculator for CloudWatch page](https://calculator.aws/#/addService/CloudWatch), scroll down to Internet Monitor\.
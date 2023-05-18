# Using Contributor Insights with Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-view-cw-tools-contributor-insights"></a>

CloudWatch Contributor Insights can help you identify top client locations and networks \(ASNs or internet service providers\) for your application\. Use the following sample Contributor Insights rules to get started with rules that are useful with Amazon CloudWatch Internet Monitor\. For more information, see [Create a Contributor Insights rule](ContributorInsights-CreateRule.md)\.

**Note**  
Internet Monitor publishes data every five minutes, so after you set up a Contributor Insights rule, you must adjust the period to five minutes to see a graph\.

**View top locations and ASNs impacted by an availability impact**

To view top client locations and ASNs impacted by a drop in availability, you can use the following Contributor Insights rule in the Syntax editor\. Replace *monitor\-name* with your own monitor name\.

```
{
    "Schema": {
        "Name": "CloudWatchLogRule",
        "Version": 1
    },
    "AggregateOn": "Sum",
    "Contribution": {
        "Filters": [
            {
                "Match": "$.clientLocation.city",
                "IsPresent": true
            }
        ],
        "Keys": [
            "$.clientLocation.city",
            "$.clientLocation.networkName"
        ],
        "ValueOf": "$.awsInternetHealth.availability.percentageOfTotalTrafficImpacted"
    },
    "LogFormat": "JSON",
    "LogGroupNames": [
        "/aws/internet-monitor/monitor-name/byCity"
    ]
}
```

**View top client locations and ASNs impacted by a latency impact**

To view top client locations and ASNs impacted by an increase in round\-trip time \(latency\), you can use the following Contributor Insights rule in the Syntax editor\. Replace *monitor\-name* with your own monitor name\.

```
{
    "Schema": {
        "Name": "CloudWatchLogRule",
        "Version": 1
    },
    "AggregateOn": "Sum",
    "Contribution": {
        "Filters": [            {
                "Match": "$.clientLocation.city",
                "IsPresent": true
            }
        ],
        "Keys": [
            "$.clientLocation.city",
            "$.clientLocation.networkName"
        ],
        "ValueOf": "$.awsInternetHealth.performance.percentageOfTotalTrafficImpacted"
    },
    "LogFormat": "JSON",
    "LogGroupNames": [
        "/aws/internet-monitor/monitor-name/byCity"
    ]
}
```

**View top client locations and ASNs impacted by total percentage of traffic**

To view top client locations and ASNs impacted by total percentage of traffic, you can use the following Contributor Insights rule in the Syntax editor\. Replace *monitor\-name* with your own monitor name\.

```
{
    "Schema": {
        "Name": "CloudWatchLogRule",
        "Version": 1
    },
    "AggregateOn": "Sum",
    "Contribution": {
        "Filters": [
            {
                "Match": "$.clientLocation.city",
                "IsPresent": true
            }
        ],
        "Keys": [
            "$.clientLocation.city",
            "$.clientLocation.networkName"
        ],
        "ValueOf": "$.percentageOfTotalTraffic"
    },
    "LogFormat": "JSON",
    "LogGroupNames": [
        "/aws/internet-monitor/monitor-name/byCity"
    ]
}
```
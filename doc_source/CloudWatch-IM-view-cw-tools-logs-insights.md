# Using CloudWatch Logs Insights with Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-view-cw-tools-logs-insights"></a>

Amazon CloudWatch Internet Monitor publishes granular measurements of availability and round\-trip time to CloudWatch Logs, and you can use CloudWatch Logs Insights queries to filter a subset of logs for a specific city or geography \(client location\), client ASN \(ISP\), and AWS source location\. 

The examples in this section can help you create CloudWatch Logs Insights queries to learn more about your own application traffic measurements and metrics\. If you use these examples in CloudWatch Logs Insights, replace *monitorName* with your own monitor name\.

**View traffic optimization suggestions**

On the **Traffic insights** tab in Internet Monitor, you can view traffic optimization suggestions, filtered by a location\. To see the same information that is displayed in the **Traffic optimization suggestions** section on that tab, but without the location granularity filter, you can use the following CloudWatch Logs Insights query\. 

1. In the AWS Management Console, navigate to CloudWatch Logs Insights\.

1. For **Log Group**, select `/aws/internet-monitor/monitorName/byCity` and `/aws/internet-monitor/monitorName/byCountry`, and then specify a time range\. 

1. Add the following query, and then run the query\. 

```
fields @timestamp, 
clientLocation.city as @city, clientLocation.subdivision as @subdivision, clientLocation.country as @country,
`trafficInsights.timeToFirstByte.currentExperience.serviceName` as @serviceNameField,
concat(@serviceNameField, ` (`, `serviceLocation`, `)`) as @currentExperienceField,
concat(`trafficInsights.timeToFirstByte.ec2.serviceName`, ` (`, `trafficInsights.timeToFirstByte.ec2.serviceLocation`, `)`) as @ec2Field,
`trafficInsights.timeToFirstByte.cloudfront.serviceName` as @cloudfrontField,
concat(`clientLocation.networkName`, ` (AS`, `clientLocation.asn`, `)`) as @networkName
| filter ispresent(`trafficInsights.timeToFirstByte.currentExperience.value`)
| stats avg(`trafficInsights.timeToFirstByte.currentExperience.value`) as @averageTTFB,
avg(`trafficInsights.timeToFirstByte.ec2.value`) as @ec2TTFB,
avg(`trafficInsights.timeToFirstByte.cloudfront.value`) as @cloudfrontTTFB,
sum(`bytesIn` + `bytesOut`) as @totalBytes,
latest(@ec2Field) as @ec2,
latest(@currentExperienceField) as @currentExperience,
latest(@cloudfrontField) as @cloudfront,
count(*) by @networkName, @city, @subdivision, @country
| display @city, @subdivision, @country, @networkName, @totalBytes, @currentExperience, @averageTTFB, @ec2, @ec2TTFB, @cloudfront, @cloudfrontTTFB
| sort @totalBytes desc
```

**View internet availability and RTT \(p50, p90, and p95\)**

To view the internet availability and round\-trip time \(p50, p90, and p95\) for traffic, you can use the following CloudWatch Logs Insights query\.

**End user geography: ** Chicago, IL, United States

**End user network \(ASN\): ** AS7018 

**AWS service location: ** US East \(N\. Virginia\) Region

To view the logs, do the following:

1. In the AWS Management Console, navigate to CloudWatch Logs Insights\.

1. For **Log Group**, select `/aws/internet-monitor/monitorName/byCity` and `/aws/internet-monitor/monitorName/byCountry`, and then specify a time range\. 

1. Add the following query, and then run the query\. 

The query returns all the performance data for users connecting from AS7018 in Chicago, IL towards US East \(N\. Virginia\) Region over the selected time period\.

```
fields @timestamp, 
internetHealth.availability.experienceScore as availabilityExperienceScore, 
internetHealth.availability.percentageOfTotalTrafficImpacted as percentageOfTotalTrafficImpacted,
internetHealth.performance.experienceScore as performanceExperienceScore,
internetHealth.performance.roundTripTime.p50 as roundTripTimep50, 
internetHealth.performance.roundTripTime.p90 as roundTripTimep90, 
internetHealth.performance.roundTripTime.p95 as roundTripTimep95
 | filter clientLocation.country == `United States` 
 and clientLocation.city == `Chicago` 
 and serviceLocation == `us-east-1` 
 and clientLocation.asn == 7018
```

For more information, see [Analyzing log data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\.
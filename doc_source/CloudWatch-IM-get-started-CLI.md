# Examples of using the CLI with Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-get-started-CLI"></a>

This section includes examples for using the AWS Command Line Interface with Amazon CloudWatch Internet Monitor operations\. 

Before you begin, make sure that you log in to use the AWS CLI with the same AWS account that has the Amazon Virtual Private Clouds \(VPCs\), Amazon CloudFront distributions, or WorkSpaces directories that you want to monitor\. Internet Monitor doesn't support accessing resources across accounts\. For more information about using the AWS CLI, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)\. For more information about using API actions with Amazon CloudWatch Internet Monitor, see the [Amazon CloudWatch Internet Monitor API Reference Guide](https://docs.aws.amazon.com/internet-monitor/latest/api/Welcome.html)\.

**Topics**
+ [Create a monitor](#CloudWatch-IM-get-started-CLI-create-mon)
+ [View monitor details](#CloudWatch-IM-get-started-CLI-mon-details)
+ [List health events](#CloudWatch-IM-get-started-CLI-list-events)
+ [View specific health event](#CloudWatch-IM-get-started-CLI-view-event-specific)
+ [View monitor list](#CloudWatch-IM-get-started-CLI-monitor-list)
+ [Edit monitor](#CloudWatch-IM-get-started-CLI-edit-monitor)
+ [Delete monitor](#CloudWatch-IM-get-started-CLI-delete-monitor)

## Create a monitor<a name="CloudWatch-IM-get-started-CLI-create-mon"></a>

When you create a monitor in Internet Monitor, you provide a name and associate resources with the monitor to show where your application's internet traffic is\. You specify a traffic percentage that defines how much of your application traffic is monitored\. That also determines the number of city\-networks, that is, client locations and ASNs, typically internet service providers or ISPs, that are monitored\. You can also opt to set a limit for the maximum number of city\-networks to monitor for your application resources, to help control your bill\. For more information, see [Choosing a city\-network maximum limit](IMCityNetworksMaximum.md)\.

Finally, you can choose if you want to publish all internet measurements for your application to Amazon S3\. Internet measurements for the top 500 city\-networks \(by traffic volume\) are automatically published to CloudWatch Logs by Internet Monitor, but you can choose to publish all measurements to S3 as well\.

To create a monitor with the AWS CLI, you use the `create-monitor` command\. The following command creates a monitor that monitors 100% of traffic but sets a maximum city\-networks limit of 10,000, adds a VPC resource, and opts to publish internet measurements to Amazon S3\.

**Note**  
Internet Monitor publishes to CloudWatch Logs internet measurements every five minutes for the top 500 city\-networks \(client locations and ASNs, typically internet service providers or ISPs\) that send traffic to each monitor\. Optionally, you can choose to publish internet measurements for all monitored city\-networks \(up to the 500,000 city\-networks service limit\) to an Amazon S3 bucket\. For more information, see [Publishing internet measurements to Amazon S3 in Amazon CloudWatch Internet Monitor](CloudWatch-IM-get-started.Publish-to-S3.md)\.

```
aws internetmonitor --create-monitor monitor-name "TestMonitor" \
				--traffic-percentage-to-monitor 100 \
				--max-city-networks-to-monitor 10000 \
				--resources "arn:aws:ec2:us-east-1:111122223333:vpc/vpc-11223344556677889" \
				--internet-measurements-log-delivery S3Config="{BucketName=MyS3Bucket,LogDeliveryStatus=ENABLED}"
```

```
{
"Arn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor",
 "Status": "ACTIVE"
}
```

**Note**  
You can't change the name of a monitor\.

## View monitor details<a name="CloudWatch-IM-get-started-CLI-mon-details"></a>

To view information about a monitor with the AWS CLI, you use the `get-monitor` command\.

```
aws internetmonitor get-monitor --monitor-name "TestMonitor"
```

```
{
    "ClientLocationType": "city",
    "CreatedAt": "2022-09-22T19:27:47Z",
    "ModifiedAt": "2022-09-22T19:28:30Z",
    "MonitorArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor",
    "MonitorName": "TestMonitor",
    "ProcessingStatus": "OK",
    "ProcessingStatusInfo": "The monitor is actively processing data",
    "Resources": [
        "arn:aws:ec2:us-east-1:111122223333:vpc/vpc-11223344556677889"
    ],
    "MaxCityNetworksToMonitor": 10000,
    "Status": "ACTIVE"
}
```

## List health events<a name="CloudWatch-IM-get-started-CLI-list-events"></a>

When performance degrades for your application's internet traffic, Internet Monitor creates health events in your monitor\. To see a list of current health events with the AWS CLI, use the `list-health-events` command

```
aws internetmonitor list-health-events --monitor-name "TestMonitor"
```

```
{
    "HealthEvents": [
        {
            "EventId": "2022-06-20T01-05-05Z/latency", 
            "Status": "RESOLVED", 
            "EndedAt": "2022-06-20T01:15:14Z", 
            "ServiceLocations": [
                {
                    "Name": "us-east-1"
                }
            ], 
            "PercentOfTotalTrafficImpacted": 1.21, 
            "ClientLocations": [
                {
                    "City": "Lockport", 
                    "PercentOfClientLocationImpacted": 60.370000000000005, 
                    "PercentOfTotalTraffic": 2.01, 
                    "Country": "United States", 
                    "Longitude": -78.6913, 
                    "AutonomousSystemNumber": 26101, 
                    "Latitude": 43.1721, 
                    "Subdivision": "New York", 
                    "NetworkName": "YAHOO-BF1"
                }
            ], 
            "StartedAt": "2022-06-20T01:05:05Z", 
            "ImpactType": "PERFORMANCE", 
            "EventArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor/health-event/2022-06-20T01-05-05Z/latency"
        }, 
        {
            "EventId": "2022-06-20T01-17-56Z/latency", 
            "Status": "RESOLVED", 
            "EndedAt": "2022-06-20T01:30:23Z", 
            "ServiceLocations": [
                {
                    "Name": "us-east-1"
                }
            ], 
            "PercentOfTotalTrafficImpacted": 1.29, 
            "ClientLocations": [
                {
                    "City": "Toronto", 
                    "PercentOfClientLocationImpacted": 75.32, 
                    "PercentOfTotalTraffic": 1.05, 
                    "Country": "Canada", 
                    "Longitude": -79.3623, 
                    "AutonomousSystemNumber": 14061, 
                    "Latitude": 43.6547, 
                    "Subdivision": "Ontario", 
                    "CausedBy": {
                        "Status": "ACTIVE", 
                        "Networks": [
                            {
                                "AutonomousSystemNumber": 16509, 
                                "NetworkName": "Amazon.com"
                            }
                        ], 
                        "NetworkEventType": "AWS"
                    }, 
                    "NetworkName": "DIGITALOCEAN-ASN"
                }, 
                {
                    "City": "Lockport", 
                    "PercentOfClientLocationImpacted": 22.91, 
                    "PercentOfTotalTraffic": 2.01, 
                    "Country": "United States", 
                    "Longitude": -78.6913, 
                    "AutonomousSystemNumber": 26101, 
                    "Latitude": 43.1721, 
                    "Subdivision": "New York", 
                    "NetworkName": "YAHOO-BF1"
                }, 
                {
                    "City": "Hangzhou", 
                    "PercentOfClientLocationImpacted": 2.88, 
                    "PercentOfTotalTraffic": 0.7799999999999999, 
                    "Country": "China", 
                    "Longitude": 120.1612, 
                    "AutonomousSystemNumber": 37963, 
                    "Latitude": 30.2994, 
                    "Subdivision": "Zhejiang", 
                    "NetworkName": "Hangzhou Alibaba Advertising Co.,Ltd."
                }
            ], 
            "StartedAt": "2022-06-20T01:17:56Z", 
            "ImpactType": "PERFORMANCE", 
            "EventArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor/health-event/2022-06-20T01-17-56Z/latency"
        }, 
        {
            "EventId": "2022-06-20T01-34-20Z/latency", 
            "Status": "RESOLVED", 
            "EndedAt": "2022-06-20T01:35:04Z", 
            "ServiceLocations": [
                {
                    "Name": "us-east-1"
                }
            ], 
            "PercentOfTotalTrafficImpacted": 1.15, 
            "ClientLocations": [
                {
                    "City": "Lockport", 
                    "PercentOfClientLocationImpacted": 39.45, 
                    "PercentOfTotalTraffic": 2.01, 
                    "Country": "United States", 
                    "Longitude": -78.6913, 
                    "AutonomousSystemNumber": 26101, 
                    "Latitude": 43.1721, 
                    "Subdivision": "New York", 
                    "NetworkName": "YAHOO-BF1"
                }, 
                {
                    "City": "Toronto", 
                    "PercentOfClientLocationImpacted": 29.770000000000003, 
                    "PercentOfTotalTraffic": 1.05, 
                    "Country": "Canada", 
                    "Longitude": -79.3623, 
                    "AutonomousSystemNumber": 14061, 
                    "Latitude": 43.6547, 
                    "Subdivision": "Ontario", 
                    "CausedBy": {
                        "Status": "ACTIVE", 
                        "Networks": [
                            {
                                "AutonomousSystemNumber": 16509, 
                                "NetworkName": "Amazon.com"
                            }
                        ], 
                        "NetworkEventType": "AWS"
                    }, 
                    "NetworkName": "DIGITALOCEAN-ASN"
                },
                {
                    "City": "Hangzhou", 
                    "PercentOfClientLocationImpacted": 2.88, 
                    "PercentOfTotalTraffic": 0.7799999999999999, 
                    "Country": "China", 
                    "Longitude": 120.1612, 
                    "AutonomousSystemNumber": 37963, 
                    "Latitude": 30.2994, 
                    "Subdivision": "Zhejiang", 
                    "NetworkName": "Hangzhou Alibaba Advertising Co.,Ltd."
                }
            ], 
            "StartedAt": "2022-06-20T01:34:20Z", 
            "ImpactType": "PERFORMANCE", 
            "EventArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor/health-event/2022-06-20T01-34-20Z/latency"
        }
    ]
}
```

## View specific health event<a name="CloudWatch-IM-get-started-CLI-view-event-specific"></a>

To see a more detailed information about a specific health event with the CLI, run the `get-health-event` command with your monitor name and a health event ID\.

```
aws internetmonitor get-monitor --monitor-name "TestMonitor" --event-id "health-event/TestMonitor/2021-06-03T01:02:03Z/latency" 
```

```
{
    "EventId": "2022-06-20T01-34-20Z/latency", 
    "Status": "RESOLVED", 
    "EndedAt": "2022-06-20T01:35:04Z", 
    "ServiceLocations": [
        {
            "Name": "us-east-1"
        }
    ], 
    "EventArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor/health-event/2022-06-20T01-34-20Z/latency", 
    "LastUpdatedAt": "2022-06-20T01:35:04Z", 
    "ClientLocations": [
        {
            "City": "Lockport", 
            "PercentOfClientLocationImpacted": 39.45, 
            "PercentOfTotalTraffic": 2.01, 
            "Country": "United States", 
            "Longitude": -78.6913, 
            "AutonomousSystemNumber": 26101, 
            "Latitude": 43.1721, 
            "Subdivision": "New York", 
            "NetworkName": "YAHOO-BF1"
        }, 
        {
            "City": "Toronto", 
            "PercentOfClientLocationImpacted": 29.770000000000003, 
            "PercentOfTotalTraffic": 1.05, 
            "Country": "Canada", 
            "Longitude": -79.3623, 
            "AutonomousSystemNumber": 14061, 
            "Latitude": 43.6547, 
            "Subdivision": "Ontario", 
            "CausedBy": {
                "Status": "ACTIVE", 
                "Networks": [
                    {
                        "AutonomousSystemNumber": 16509, 
                        "NetworkName": "Amazon.com"
                    }
                ], 
                "NetworkEventType": "AWS"
            }, 
            "NetworkName": "DIGITALOCEAN-ASN"
        }, 
        {
            "City": "Shenzhen", 
            "PercentOfClientLocationImpacted": 4.07, 
            "PercentOfTotalTraffic": 0.61, 
            "Country": "China", 
            "Longitude": 114.0683, 
            "AutonomousSystemNumber": 37963, 
            "Latitude": 22.5455, 
            "Subdivision": "Guangdong", 
            "NetworkName": "Hangzhou Alibaba Advertising Co.,Ltd."
        }, 
        {
            "City": "Hangzhou", 
            "PercentOfClientLocationImpacted": 2.88, 
            "PercentOfTotalTraffic": 0.7799999999999999, 
            "Country": "China", 
            "Longitude": 120.1612, 
            "AutonomousSystemNumber": 37963, 
            "Latitude": 30.2994, 
            "Subdivision": "Zhejiang", 
            "NetworkName": "Hangzhou Alibaba Advertising Co.,Ltd."
        }
    ], 
    "StartedAt": "2022-06-20T01:34:20Z", 
    "ImpactType": "PERFORMANCE", 
    "PercentOfTotalTrafficImpacted": 1.15
}
```

## View monitor list<a name="CloudWatch-IM-get-started-CLI-monitor-list"></a>

To see a list of all monitors in your account with the CLI, run the `list-monitors` command\.

```
aws internetmonitor list-monitors
```

```
{
    "Monitors": [
        {
            "MonitorName": "TestMonitor",
            "ProcessingStatus": "OK",
            "Status": "ACTIVE"
        }
    ],
    "NextToken": " zase12"
}
```

## Edit monitor<a name="CloudWatch-IM-get-started-CLI-edit-monitor"></a>

To update information about your monitor by using the CLI, use the `update-monitor` command and specify the name of the monitor to update\. You can update the limit of the maximum number of city\-networks to monitor, add or remove the resources that Internet Monitor uses to monitor traffic, and change the monitor status from `ACTIVE` to `INACTIVE`, or vice versa\. Note that you can't change the name of the monitor\.

The response for an `update-monitor` call returns just the `MonitorArn` and the `Status`\.

The following example shows how to use the `update-monitor` command to change the maximum number of city\-networks to monitor to `50000`:

```
aws internetmonitor update-monitor --monitor-name "TestMonitor" --max-city-networks-to-monitor 50000
```

```
{
    "MonitorArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor",
    "Status": " ACTIVE "
}
```

The following example shows how to add and remove resources:

```
aws internetmonitor update-monitor --monitor-name "TestMonitor" \
				--resources-to-add "arn:aws:ec2:us-east-1:111122223333:vpc/vpc-11223344556677889" \
				--resources-to-remove "arn:aws:ec2:us-east-1:111122223333:vpc/vpc-2222444455556666"
```

```
{
    "MonitorArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor",
    "Status": "ACTIVE"
}
```

The following example shows how to use the `update-monitor` command to change the monitor status to `INACTIVE`:

```
aws internetmonitor update-monitor --monitor-name "TestMonitor" --status "INACTIVE"
```

```
{
    "MonitorArn": "arn:aws:internetmonitor:us-east-1:111122223333:monitor/TestMonitor",
    "Status": "INACTIVE"
}
```

## Delete monitor<a name="CloudWatch-IM-get-started-CLI-delete-monitor"></a>

You can delete a monitor with the CLI by using the `delete-monitor` command\. First, you must set the monitor to be inactive\. To do that, use the `update-monitor` command to change the status to `INACTIVE`\. Confirm that the monitor is inactive by using the `get-monitor` command and checking the status\.

When the monitor status is `INACTIVE`, then you can use the CLI to run the `delete-monitor` command to delete the monitor\. The response for a successful `delete-monitor` call is empty\.

```
aws internetmonitor delete-monitor --monitor-name "TestMonitor"
```

```
{}
```
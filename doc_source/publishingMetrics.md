# Publishing custom metrics<a name="publishingMetrics"></a>

You can publish your own metrics to CloudWatch using the AWS CLI or an API\. You can view statistical graphs of your published metrics with the AWS Management Console\.

 CloudWatch stores data about a metric as a series of data points\. Each data point has an associated time stamp\. You can even publish an aggregated set of data points called a *statistic set*\.

**Topics**
+ [High\-resolution metrics](#high-resolution-metrics)
+ [Using dimensions](#usingDimensions)
+ [Publishing single data points](#publishingDataPoints)
+ [Publishing statistic sets](#publishingDataPoints1)
+ [Publishing the value zero](#publishingZero)

## High\-resolution metrics<a name="high-resolution-metrics"></a>

Each metric is one of the following:
+ Standard resolution, with data having a one\-minute granularity
+ High resolution, with data at a granularity of one second

Metrics produced by AWS services are standard resolution by default\. When you publish a custom metric, you can define it as either standard resolution or high resolution\. When you publish a high\-resolution metric, CloudWatch stores it with a resolution of 1 second, and you can read and retrieve it with a period of 1 second, 5 seconds, 10 seconds, 30 seconds, or any multiple of 60 seconds\.

High\-resolution metrics can give you more immediate insight into your application's sub\-minute activity\. Keep in mind that every `PutMetricData` call for a custom metric is charged, so calling `PutMetricData` more often on a high\-resolution metric can lead to higher charges\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

If you set an alarm on a high\-resolution metric, you can specify a high\-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds\. There is a higher charge for high\-resolution alarms with a period of 10 or 30 seconds\.

## Using dimensions<a name="usingDimensions"></a>

In custom metrics, the `--dimensions` parameter is common\. A dimension further clarifies what the metric is and what data it stores\. You can have up to 10 dimensions in one metric, and each dimension is defined by a name and value pair\.

How you specify a dimension is different when you use different commands\. With [put\-metric\-data](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-data.html), you specify each dimension as *MyName*=*MyValue*, and with [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) or [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) you use the format `Name=`*MyName*, `Value=`*MyValue*\. For example, the following command publishes a `Buffers` metric with two dimensions named `InstanceId` and `InstanceType`\.

```
aws cloudwatch put-metric-data --metric-name Buffers --namespace MyNameSpace --unit Bytes --value 231434333 --dimensions InstanceId=1-23456789,InstanceType=m1.small
```

This command retrieves statistics for that same metric\. Separate the Name and Value parts of a single dimension with commas, but if you have multiple dimensions, use a space between one dimension and the next\.

```
aws cloudwatch get-metric-statistics --metric-name Buffers --namespace MyNameSpace --dimensions Name=InstanceId,Value=1-23456789 Name=InstanceType,Value=m1.small --start-time 2016-10-15T04:00:00Z --end-time 2016-10-19T07:00:00Z --statistics Average --period 60
```

If a single metric includes multiple dimensions, you must specify a value for every defined dimension when you use [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html)\. For example, the Amazon S3 metric `BucketSizeBytes` includes the dimensions `BucketName` and `StorageType`, so you must specify both dimensions with [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html)\.

```
aws cloudwatch get-metric-statistics --metric-name BucketSizeBytes --start-time 2017-01-23T14:23:00Z --end-time 2017-01-26T19:30:00Z --period 3600 --namespace AWS/S3 --statistics Maximum --dimensions Name=BucketName,Value=MyBucketName Name=StorageType,Value=StandardStorage --output table
```

To see what dimensions are defined for a metric, use the [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command\.

## Publishing single data points<a name="publishingDataPoints"></a>

To publish a single data point for a new or existing metric, use the [put\-metric\-data](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-data.html) command with one value and time stamp\. For example, the following actions each publish one data point\.

```
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 2 --timestamp 2016-10-20T12:00:00.000Z
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 4 --timestamp 2016-10-20T12:00:01.000Z
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 5 --timestamp 2016-10-20T12:00:02.000Z
```

If you call this command with a new metric name, CloudWatch creates a metric for you\. Otherwise, CloudWatch associates your data with the existing metric that you specified\.

**Note**  
When you create a metric, it can take up to 2 minutes before you can retrieve statistics for the new metric using the [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) command\. However, it can take up to 15 minutes before the new metric appears in the list of metrics retrieved using the [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command\.

Although you can publish data points with time stamps as granular as one\-thousandth of a second, CloudWatch aggregates the data to a minimum granularity of 1 second\. CloudWatch records the average \(sum of all items divided by number of items\) of the values received for each period, as well as the number of samples, maximum value, and minimum value for the same time period\. For example, the `PageViewCount` metric from the previous examples contains three data points with time stamps just seconds apart\. If you have your period set to 1 minute, CloudWatch aggregates the three data points because they all have time stamps within a 1\-minute period\. 

You can use the get\-metric\-statistics command to retrieve statistics based on the data points that you published\.

```
aws cloudwatch get-metric-statistics --namespace MyService --metric-name PageViewCount \ 
--statistics "Sum" "Maximum" "Minimum" "Average" "SampleCount" \ 
--start-time 2016-10-20T12:00:00.000Z --end-time 2016-10-20T12:05:00.000Z --period 60
```

The following is example output\.

```
{
    "Datapoints": [
        {
            "SampleCount": 3.0, 
            "Timestamp": "2016-10-20T12:00:00Z", 
            "Average": 3.6666666666666665, 
            "Maximum": 5.0, 
            "Minimum": 2.0, 
            "Sum": 11.0, 
            "Unit": "None"
        }
    ], 
    "Label": "PageViewCount"
}
```

## Publishing statistic sets<a name="publishingDataPoints1"></a>

You can aggregate your data before you publish to CloudWatch\. When you have multiple data points per minute, aggregating data minimizes the number of calls to put\-metric\-data\. For example, instead of calling put\-metric\-data multiple times for three data points that are within 3 seconds of each other, you can aggregate the data into a statistic set that you publish with one call, using the `--statistic-values` parameter\.

```
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --statistic-values Sum=11,Minimum=2,Maximum=5,SampleCount=3 --timestamp 2016-10-14T12:00:00.000Z
```

CloudWatch needs raw data points to calculate percentiles\. If you publish data using a statistic set instead, you can't retrieve percentile statistics for this data unless one of the following conditions is true:
+ The `SampleCount` of the statistic set is 1
+ The `Minimum` and the `Maximum` of the statistic set are equal

## Publishing the value zero<a name="publishingZero"></a>

 When your data is more sporadic and you have periods that have no associated data, you can choose to publish the value zero \(`0`\) for that period or no value at all\. If you use periodic calls to `PutMetricData` to monitor the health of your application, you might want to publish zero instead of no value\. For example, you can set a CloudWatch alarm to notify you if your application fails to publish metrics every five minutes\. You want such an application to publish zeros for periods with no associated data\. 

 You might also publish zeros if you want to track the total number of data points or if you want statistics such as minimum and average to include data points with the value 0\. 
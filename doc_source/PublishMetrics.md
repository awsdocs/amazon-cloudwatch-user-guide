# Scenario: Publish Metrics to CloudWatch<a name="PublishMetrics"></a>

In this scenario, you'll use the AWS Command Line Interface \(AWS CLI\) to publish a single metric for a hypothetical application named *GetStarted*\. If you haven't already installed and configured the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

**Topics**
+ [Step 1: Define the Data Configuration](#define-data-domain)
+ [Step 2: Add Metrics to CloudWatch](#add-metrics-to-scenario)
+ [Step 3: Get Statistics from CloudWatch](#GetStatistics)
+ [Step 4: View Graphs with the Console](#ViewGraphs)

## Step 1: Define the Data Configuration<a name="define-data-domain"></a>

In this scenario, you'll publish data points that track the request latency for the application\. Choose names for your metric and namespace that make sense to you\. For this example, name the metric *RequestLatency* and place all of the data points into the *GetStarted* namespace\. 

You'll publish several data points that collectively represent three hours of latency data\. The raw data comprises fifteen request latency readings distributed over three hours\. Each reading is in milliseconds: 
+ Hour one: 87, 51, 125, 235
+ Hour two: 121, 113, 189, 65, 89
+ Hour three: 100, 47, 133, 98, 100, 328

You can publish data to CloudWatch as single data points or as an aggregated set of data points called a *statistic set*\. You can aggregate metrics to a granularity as low as one minute\. You can publish the aggregated data points to CloudWatch as a set of statistics with four predefined keys: `Sum`, `Minimum`, `Maximum`, and `SampleCount`\.

You'll publish the data points from hour one as single data points\. For the data from hours two and three, you'll aggregate the data points and publish a statistic set for each hour\. The key values are shown in the following table\.


| Hour | Raw Data | Sum | Minimum | Maximum | SampleCount | 
| --- | --- | --- | --- | --- | --- | 
| `1` | `87` |  |  |  |  | 
| `1` | `51` |  |  |  |  | 
| `1` | `125` |  |  |  |  | 
| `1` | `235` |  |  |  |  | 
| `2` | `121, 113, 189, 65, 89` | `577` | `65` | `189` | `5` | 
| `3` | `100, 47, 133, 98, 100, 328` | `806` | `47` | `328` | `6` | 

## Step 2: Add Metrics to CloudWatch<a name="add-metrics-to-scenario"></a>

After you have defined your data configuration, you are ready to add data\.

**To publish data points to CloudWatch**

1. At a command prompt, run the following [put\-metric\-data](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-data.html) commands to add data for the first hour\. Replace the example time stamp with a time stamp that is two hours in the past, in Universal Coordinated Time \(UTC\)\.

   ```
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --timestamp 2016-10-14T20:30:00Z --value 87 --unit Milliseconds
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --timestamp 2016-10-14T20:30:00Z --value 51 --unit Milliseconds
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --timestamp 2016-10-14T20:30:00Z --value 125 --unit Milliseconds
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --timestamp 2016-10-14T20:30:00Z --value 235 --unit Milliseconds
   ```

1. Add data for the second hour, using a time stamp that is one hour later than the first hour\.

   ```
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --timestamp 2016-10-14T21:30:00Z --statistic-values Sum=577,Minimum=65,Maximum=189,SampleCount=5 --unit Milliseconds
   ```

1. Add data for the third hour, omitting the time stamp to default to the current time\. 

   ```
   aws cloudwatch put-metric-data --metric-name RequestLatency --namespace GetStarted \
   --statistic-values Sum=806,Minimum=47,Maximum=328,SampleCount=6 --unit Milliseconds
   ```

## Step 3: Get Statistics from CloudWatch<a name="GetStatistics"></a>

Now that you have published metrics to CloudWatch, you can retrieve statistics based on those metrics using the [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) command as follows\. Be sure to specify `--start-time` and `--end-time` far enough in the past to cover the earliest time stamp that you published\.

```
aws cloudwatch get-metric-statistics --namespace GetStarted --metric-name RequestLatency --statistics Average \
--start-time 2016-10-14T00:00:00Z --end-time 2016-10-15T00:00:00Z --period 60
```

The following is example output:

```
{
	"Datapoints": [],
	"Label": "Request:Latency"
}
```

## Step 4: View Graphs with the Console<a name="ViewGraphs"></a>

After you have published metrics to CloudWatch, you can use the CloudWatch console to view statistical graphs\.

**To view graphs of your statistics on the console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the **Navigation** pane, choose **Metrics**\.

1. In the **All metrics** tab, in the search box, type **RequestLatency** and press Enter\.

1. Select the check box for the **RequestLatency** metric\. A graph of the metric data is displayed in the upper pane\.

For more information, see [Graph Metrics](graph_metrics.md)\.
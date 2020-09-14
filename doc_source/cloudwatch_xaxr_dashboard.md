# Cross\-Account Cross\-Region Dashboards<a name="cloudwatch_xaxr_dashboard"></a>

You can create *cross\-account cross\-Region dashboards*, which summarize your CloudWatch data from multiple AWS accounts and multiple Regions into one dashboard\. From this high\-level dashboard you can get a view of your entire application, and also drill down into more specific dashboards without having to log in and out of accounts or switch Regions\.

You can create cross\-account cross\-Region dashboards in the AWS Management Console and programmatically\.

**Pre\-Requisite**

Before you can create a cross\-account cross\-Region dashboard, you must enable at least one sharing account and at least one monitoring account\. Additionally, to be able to use the CloudWatch console to create a cross\-account dashboard, you must enable the console for cross\-account functionality\. For more information, see [Cross\-Account Cross\-Region CloudWatch Console](Cross-Account-Cross-Region.md)\.

## Creating and Using a Cross\-Account Cross\-Region Dashboard With the AWS Management Console<a name="create_xaxr_dashboard"></a>

You can use the AWS Management Console to create a cross\-account cross\-Region dashboard\.

**To create a cross\-account cross\-Region dashboard**

1. Log in to the monitoring account\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose a dashboard, or create a new one\.

1. At the top of the screen, use the boxes by **View data for** to switch between accounts and Regions\.

   As you create your dashboard, you can include widgets from multiple accounts and Regions\. Widgets include graphs, alarms, and CloudWatch Logs Insights widgets\.

**Creating a graph with metrics from different accounts and Regions**

1. Log in to the monitoring account\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Under **All metrics**, use the boxes by **View data for** to choose the account and Region that you want to add metrics from\.

1. Add the metrics you want to the graph\. For more information, see [Graphing Metrics](graph_metrics.md)\.

1. Repeat steps 4\-5 to add metrics from other accounts and Regions\.

1. \(Optional\) Choose the **Graphed metrics** tab and add a metric math function that uses the metrics that you have chosen\. For more information, see [Using Metric Math](using-metric-math.md)\.

   You can also set up a single graph to include multiple `SEARCH` functions\. Each search can refer to a different account or Region\.

1. When you are finished with the graph, choose **Actions**, **Add to dashboard**\.

   Select your cross\-account dashboard, and choose **Add to dashboard**\.

**Adding an alarm from a different account to your cross\-account dashboard**

1. Log in to the monitoring account\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Use the boxes by **View data for** to choose the account where the alarm is located\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm that you want to add, and choose **Add to dashboard**\.

1. Select the cross\-account dashboard that you want to add it to, and choose **Add to dashboard**\.

## Create a Cross\-Account Cross\-Region Dashboard Programmatically<a name="create_xaxr_dashboard_API"></a>

You can use the AWS APIs and SDKs to create dashboards programmatically\. For more information, see [ PutDashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutDashboard)\.

To enable cross\-account cross\-Region dashboards, we have added new parameters to the dashboard body structure, as shown in the following table and examples\. For more information about overall dashboard body structure, see [ Dashboard Body Structure and Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/CloudWatch-Dashboard-Body-Structure.html)\.


| Parameter | Use | Scope | Default | 
| --- | --- | --- | --- | 
|  `accountId` | Specifies the ID of the account where the widget or the metric is located\. |  Widget or metric |  Account that is currently logged in  | 
|  `region` | Specifies the Region of the metric\. |  Widget or metric |  Current Region selected in the console  | 

The following examples illustrate the JSON source for widgets in a cross\-account cross\-Region dashboard\.

This example sets the `accountId` field to the ID of the sharing account at the widget level\. This specifies that all metrics in this widget will come from that sharing account and Region\.

```
{
    "widgets": [
        {
            ...
            "properties": {
 	            "metrics": [
                     …..
                     ],	 
                    "accountId": "111122223333",
                    "region": "us-east-1"
           }
       }
  ]
}
```

This example sets the `accountId` field differently at the level of each metric\. In this example, the different metrics in this metric math expression come from different sharing accounts and different Regions\.

```
{
    "widgets": [
        {
            ...
            "properties": {
                "metrics": [
                    [ { "expression": "SUM(METRICS())", "label": "[avg: ${AVG}] Expression1", "id": "e1", "stat": "Sum" } ],
                    [ "AWS/EC2", "CPUUtilization", { "id": "m2", "accountId": "5555666677778888", "region": "us-east-1", "label": "[avg: ${AVG}] ApplicationALabel " } ],
                    [ ".", ".", { "id": "m1", "accountId": "9999000011112222", "region": "eu-west-1", "label": "[avg: ${AVG}] ApplicationBLabel" } ]
                ],
                "view": "timeSeries",
                "stacked": false,
                "stat": "Sum",
                "period": 300,
                "title": "Cross account example"
            }
        }
    ]
}
```

This example shows an alarm widget\.

```
{
    "type": "metric",
    "x": 6,
    "y": 0,
    "width": 6,
    "height": 6,
    "properties": {
        "accountID": "111122223333",
        "title": "over50",
        "annotations": {
            "alarms": [
                "arn:aws:cloudwatch:us-east-1:379642911888:alarm:over50"
            ]
        },
        "view": "timeSeries",
        "stacked": false
    }
}
```

This example is for a CloudWatch Logs Insights widget\.

```
{
    "type": "log",
    "x": 0,
    "y": 6,
    "width": 24,
    "height": 6,
    "properties": {
        "query": "SOURCE 'route53test' | fields @timestamp, @message\n| sort @timestamp desc\n| limit 20",
        "accountId": "111122223333",
        "region": "us-east-1",
        "stacked": false,
        "view": "table"
    }
}
```

Another way to create dashboards programmatically is to first create one in the AWS Management Console, and then copy the JSON source of this dashboard\. To do so, load the dashboard and choose **Actions**, **View/edit source**\. You can then copy this dashboard JSON to use as a template to create similar dashboards\.
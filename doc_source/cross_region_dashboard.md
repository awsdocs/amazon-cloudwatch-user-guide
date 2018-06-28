# Monitor Resources in Multiple Regions Using a CloudWatch Dashboard<a name="cross_region_dashboard"></a>

You can monitor AWS resources in multiple Regions using a single CloudWatch dashboard\. For example, you can create a dashboard that shows CPU utilization for an EC2 instance located in the `us-west-2` Region with your billing metrics, which are located in the `us-east-1` Region\.

**To monitor resources in multiple Regions in one dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the navigation bar, select a Region\.

1. Select the metrics to add to your dashboard\.

1. For **Actions**, choose **Add to dashboard**\.

1. For **Add to**, type a name for the new dashboard and choose **Add to dashboard**\.

   Alternatively, to add to an existing dashboard, choose **Existing dashboard**, select a dashboard, and then choose **Add to dashboard**\.

1. To add metrics from another Region, select the next Region and repeat these steps\.

1. Choose **Save dashboard**\.
# Modify experiment traffic<a name="CloudWatch-Evidently-modifyexperimenttraffic"></a>

You can modify the sampling rate for an experiment at any time, including while the experiment is ongoing\. However, you can't update the treatment weights after an experiment is running\. Therefore, you can change the total traffic exposed to the experiment after an experiment is running, but not the relative allocation to each treatment\. If you modify the traffic of an ongoing experiment, we recommend that you only increase the traffic allocation, so that you don't introduce bias\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/evidently_experiment_split.png)

**To modify the traffic allocation for an experiment**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project that contains the launch\.

1. Choose the **Experiments** tab\.

1. Choose the name of the launch\.

1. Choose **Modify experiment traffic**\.

1. Enter a percentage or use the slider to specify how much of the available traffic to allocate to this experiment\. The available traffic is the total audience minus the traffic that is allocated to a current launch, if there is one\. The traffic that is not allocated to the launch or experiment is served the default variation\.

1. Choose **Modify**\.
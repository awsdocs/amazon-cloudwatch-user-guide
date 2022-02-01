# See the current evaluation rules and audience traffic for a feature<a name="CloudWatch-Evidently-featuretraffic"></a>

You can use the CloudWatch Evidently console to see how the feature's evaluation rules are allocating the audience traffic among the feature's current launches, experiments, and variations\.

**To view the audience traffic for a feature**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project that contains the feature\.

1. Choose the **Features** tab\.

1. Choose the name of the feature\.

   In the **Evaluation rules** tab, you can see the flow of audience traffic for your feature, as follows:
   + First, the overrides are evaluated\. These specify that certain users are always served a specific variation\. 
   + Next, the remaining traffic is available for the ongoing launch, if there is one\. If there is a launch in progress, the table in the **Launches** section displays the launch name and the launch traffic split among the feature variations\. On the right side of the **Launches** section, a **Traffic** indicator displays how much of the available audience \(after overrides\) is allocated to this launch\. The rest of the traffic not allocated to the launch flows to the experiment \(if any\) and then the default variation\.
   + Next, the remaining traffic is available for the ongoing experiment, if there is one\. If there is an experiment in progress, the table in the **Experiments** section displays the experiment name and progress\. On the right side of the **Experiments** section, a **Traffic** indicator displays how much of the available audience \(after overrides and launches\) is allocated to this experiment\. The rest of the traffic not allocated to the launch or the experiment is served the default variation of the feature\.
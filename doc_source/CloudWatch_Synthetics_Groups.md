# Groups<a name="CloudWatch_Synthetics_Groups"></a>

You can create *groups* to associate canaries with each other, including cross\-Region canaries\. Using groups can help you with managing and automating your canaries, and you can also view aggregated run results and statistics for all canaries in a group\. 

Groups are global resources\. When you create a group, it is replicated across all AWS Regions that support groups, and you can add canaries from any of these Regions to it, and view it in any of these Regions\. Although the group ARN format reflects the Region name where it was created, a group is not constrained to any Region\. This means that you can put canaries from multiple Regions into the same group, and then use that group to view and manage all of those canaries in a single view\.

Groups are supported in all Regions except the Regions that are disabled by default\. For more information about these Regions, see [Enabling a Region ](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html#rande-manage-enable)\.

Each group can contain as many as 10 canaries\. You can have as many as 20 groups in your account\. Any single canary can be a member of up to 10 groups\.

**To create a group**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Synthetics Canaries**\.

   

1. Choose **Create Group**\.

1. Under **Group Name**, enter a name for the group\. 

1. Select canaries to associate with this group\. To select a canary, type its complete name in **Exact canary name** and choose **Search**\. Then select the check box next to the canary name\. If there are multiple canaries with the same name in different Regions, be sure to select the canaries that you want\.

   You can repeat this step to associate as many as 10 canaries with the group\.

1. \(Optional\) Under **Tags**, add one or more key\-value pairs as tags for this group\. Tags can help you identify and organize your AWS resources and track your AWS costs\. For more information, see [Tagging your Amazon CloudWatch resources](CloudWatch-Tagging.md)\.

1. Choose **Create Group**\.
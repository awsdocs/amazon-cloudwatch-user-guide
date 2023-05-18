# Tagging your Amazon CloudWatch resources<a name="CloudWatch-Tagging"></a>

A *tag* is a custom attribute label that you or AWS assigns to an AWS resource\. Each tag has two parts:
+ A *tag key* \(for example, `CostCenter`, `Environment`, or `Project`\)\. Tag keys are case sensitive\.
+ An optional field known as a *tag value* \(for example, `111122223333` or `Production`\)\. Omitting the tag value is the same as using an empty string\. Like tag keys, tag values are case sensitive\.

Tags help you do the following:
+ Identify and organize your AWS resources\. Many AWS services support tagging, so you can assign the same tag to resources from different services to indicate that the resources are related\. For example, you can assign the same tag to a CloudWatch rule that you assign to an EC2 instance\.
+ Track your AWS costs\. You activate these tags on the AWS Billing and Cost Management dashboard\. AWS uses the tags to categorize your costs and deliver a monthly cost allocation report to you\. For more information, see [Use Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the [AWS Billing User Guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

The following sections provide more information about tags for CloudWatch\.

## Supported resources in CloudWatch<a name="supported-resources"></a>

The following resources in CloudWatch support tagging: 
+ Alarms – You can tag alarms using the [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/tag-resource.html) AWS CLI command and the [TagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_TagResource.html) API\.
+ Canaries – You can tag canaries using the CloudWatch console\. For more information, see [Creating a canary](CloudWatch_Synthetics_Canaries_Create.md)\.
+ Contributor Insights rules – You can tag Contributor Insights rules when you create them by using the [put\-insight\-rule](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-insight-rule.html) AWS CLI command and the [PutInsightRule](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutInsightRule.html) API\. You can add tags to existing rules by using the [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/tag-resource.html) AWS CLI command and the [TagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_TagResource.html) API\.
+ Metric streams – You can tag metric streams when you create them by using the [put\-metric\-stream](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-stream.html) AWS CLI command and the [PutMetricStream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricStream.html) API\. You can add tags to existing metric streams by using the [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/tag-resource.html) AWS CLI command and the [TagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_TagResource.html) API\.

For information about adding and managing tags, see [Managing tags](#CloudWatch-tagging-add-edit-delete)\.

## Managing tags<a name="CloudWatch-tagging-add-edit-delete"></a>

Tags consist of the `Key` and `Value` properties on a resource\. You can use the CloudWatch console, the AWS CLI, or the CloudWatch API to add, edit, or delete the values for these properties\. For information about working with tags, see the following:
+ [TagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_TagResource.html), [UntagResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_UntagResource.html), and [ListTagsForResource](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListTagsForResource.html) in the *Amazon CloudWatch API Reference*
+ [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/tag-resource.html), [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/untag-resource.html), and [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-tags-for-resource.html) in the *Amazon CloudWatch CLI Reference*
+ [Working with Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) in the *Resource Groups User Guide*

## Tag naming and usage conventions<a name="tagging-restrictions"></a>

The following basic naming and usage conventions apply to using tags with CloudWatch resources:
+ Each resource can have a maximum of 50 tags\.
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ The maximum tag key length is 128 Unicode characters in UTF\-8\.
+ The maximum tag value length is 256 Unicode characters in UTF\-8\.
+ Allowed characters are letters, numbers, spaces representable in UTF\-8, and the following characters: ***\. : \+ = @ \_ / \-*** \(hyphen\)\.
+ Tag keys and values are case sensitive\. As a best practice, decide on a strategy for capitalizing tags and consistently implement that strategy across all resource types\. For example, decide whether to use `Costcenter`, `costcenter`, or `CostCenter` and use the same convention for all tags\. Avoid using similar tags with inconsistent case treatment\. 
+ The `aws:` prefix is prohibited for tags because it's reserved for AWS use\. You can't edit or delete tag keys or values with this prefix\. Tags with this prefix don't count against your tags per resource limit\.
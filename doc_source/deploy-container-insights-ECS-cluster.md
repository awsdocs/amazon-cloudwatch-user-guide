# Setting up Container Insights on Amazon ECS for cluster\- and service\-level metrics<a name="deploy-container-insights-ECS-cluster"></a>

You can enable Container Insights on new and existing Amazon ECS clusters\. Container Insights collects metrics at the cluster, task, and service levels\. For existing clusters, you use the AWS CLI\. For new clusters, use either the Amazon ECS console or the AWS CLI\.

If you're using Amazon ECS on an Amazon EC2 instance, and you want to collect network and storage metrics from Container Insights, launch that instance using an AMI that includes Amazon ECS agent version 1\.29\. For information about updating your agent version, see [Updating the Amazon ECS Container Agent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-update.html)

You can use the AWS CLI to set account\-level permission to enable Container Insights for any new Amazon ECS clusters created in your account\. To do so, enter the following command\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

## Setting up Container Insights on existing Amazon ECS clusters<a name="deploy-container-insights-ECS-existing"></a>

To enable Container Insights on an existing Amazon ECS cluster, enter the following command\. You must be running version 1\.16\.200 or later of the AWS CLI for the following command to work\.

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=enabled
```

## Setting up Container Insights on new Amazon ECS clusters<a name="deploy-container-insights-ECS-new"></a>

There are two ways to enable Container Insights on new Amazon ECS clusters\. You can configure Amazon ECS so that all new clusters are enabled for Container Insights by default\. Otherwise, you can enable a new cluster when you create it\.

### Using the AWS Management Console<a name="deploy-container-insights-ECS-new-console"></a>

You can turn on Container Insights on all new clusters by default, or on an individual cluster as you create it\.

**To turn on Container Insights on all new clusters by default**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation page, choose **Account Settings**\.

1. Choose **Update**\.

1.  To use CloudWatch Container Insights by default for clusters, under **CloudWatch Container Insights**, select or clear **CloudWatch Container Insights**\.

1. Choose **Save changes**\.

If you haven't used the preceding procedure to enable Container Insights on all new clusters by default, use the following steps to create a cluster with Container Insights enabled\.

**To create a cluster with Container Insights turned on**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter a unique name\.

   The name can contain up to 255 letters \(uppercase and lowercase\), numbers, and hyphens\.

1. To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

You can now create task definitions, run tasks, and launch services in the cluster\. For more information, see the following:
+ [Creating a task definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)
+ [Running tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_run_task-v2.html)
+ [Creating a service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service-console-v2.html)

### Setting up Container Insights on new Amazon ECS clusters using the AWS CLI<a name="deploy-container-insights-ECS-CLI"></a>

To enable Container Insights on all new clusters by default, enter the following command\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

If you didn't use the preceding command to enable Container Insights on all new clusters by default, enter the following command to create a new cluster with Container Insights enabled\. You must be running version 1\.16\.200 or later of the AWS CLI for the following command to work\.

```
aws ecs create-cluster --cluster-name myCICluster --settings "name=containerInsights,value=enabled"
```

## Disabling Container Insights on Amazon ECS clusters<a name="deploy-container-insights-ECS-disable"></a>

To disable Container Insights on an existing Amazon ECS cluster, enter the following command\.

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=disabled
```
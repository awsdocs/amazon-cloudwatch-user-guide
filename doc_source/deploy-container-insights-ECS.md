# Setting Up Container Insights on Amazon ECS<a name="deploy-container-insights-ECS"></a>

You can enable Container Insights on existing Amazon ECS clusters and on new clusters that you create\. For existing clusters, you use the AWS CLI\. For new clusters, use either the Amazon ECS console or the AWS CLI\.

If you're using Amazon ECS on an Amazon EC2 instance, to collect network and storage metrics from Container Insights you must launch that instance using an AMI that includes Amazon ECS agent version 1\.29\. For information about updating your agent version, see [Updating the Amazon ECS Container Agent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-update.html)

Currently, you can't enable Container Insights when you use AWS CloudFormation to create a new cluster\. As a workaround, you can use the AWS CLI to set account\-level permission to enable Container Insights for any new Amazon ECS clusters created in your account\. To do so, enter the following command\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

## Setting Up Container Insights on Existing Amazon ECS Clusters<a name="deploy-container-insights-ECS-existing"></a>

To enable Container Insights on an existing Amazon ECS cluster, enter the following command\. You must be running version 1\.16\.200 or later of the AWS CLI for the following command to work\.

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=enabled
```

## Setting Up Container Insights on New Amazon ECS Clusters<a name="deploy-container-insights-ECS-new"></a>

There are two ways to enable Container Insights on new Amazon ECS clusters\. You can configure Amazon ECS so that all new clusters are enabled for Container Insights by default, or you can enable a new cluster when you create it\.

### Using the AWS Management Console<a name="deploy-container-insights-ECS-new-console"></a>

You can enable Container Insights on all new clusters by default, or on an individual cluster as you create it\.

**To enable Container Insights on all new clusters by default**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Account Settings**\.

1. Select the check box at the bottom of the page to enable the Container Insights default\.

If you haven't used the preceding procedure to enable Container Insights on all new clusters by default, use the following steps to create a cluster with Container Insights enabled\.

**To create a cluster with Container Insights enabled**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Create cluster**\.

1. On the next page, do the following:

   1. Name your cluster\.

   1. If you don’t have a VPC already, select the check box to create one\. You can use the default values for the VPC\.

   1. Fill out all other needed information, including instance type\.

   1. Select **Enabled Container Insights**\.

   1. Choose **Create**\.

You can now create task definitions, run tasks, and launch services in the cluster\. For more information, see the following:
+ [Creating a Task Definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)
+ [Running Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_run_task.html)
+ [Creating a Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service.html)

### Setting Up Container Insights on New Amazon ECS Clusters Using the AWS CLI<a name="deploy-container-insights-ECS-CLI"></a>

To enable Container Insights on all new clusters by default, enter the following\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

If you didn't use the preceding command to enable Container Insights on all new clusters by default, enter the following command to create a new cluster with Container Insights enabled\. You must be running version 1\.16\.200 or later of the AWS CLI for the following command to work\.

```
aws ecs create-cluster --cluster-name myCICluster --settings "name=containerInsights,value=enabled"
```

## Disabling Container Insights on Amazon ECS Clusters<a name="deploy-container-insights-ECS-disable"></a>

To disable Container Insights on an existing Amazon ECS cluster, enter the following command\.

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=disabled
```
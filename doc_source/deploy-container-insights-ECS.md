# Setting Up Container Insights on Amazon ECS<a name="deploy-container-insights-ECS"></a>

You can use either the Amazon ECS console or the AWS CLI to enable Container Insights on Amazon ECS\.

During this public preview, only new clusters can be enabled for Container Insights\. Additionally, if you're using Amazon ECS on an Amazon EC2 instance, to collect network and storage metrics from Container Insights you must launch that instance using an AMI that includes Amazon ECS agent version 1\.29\. For information on updating your agent version, see [Updating the Amazon ECS Container Agent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-update.html)

During this public preview, you can't enable Container Insights when you use AWS CloudFormation to create a new cluster\. As a workaround, you can use the AWS CLI to set account level permission to enable Container Insights for any new Amazon ECS clusters created in your account\. To do so, enter the following command\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

## Setting Up Container Insights on Amazon ECS Using the Console<a name="deploy-container-insights-ECS-console"></a>

There are two ways to enable Container Insights\. You can configure Amazon ECS so that all new clusters are enabled for Container Insights by default, or you can enable a new cluster when you create it\.

**To enable Container Insights on new clusters by default**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Account Settings**\.

1. Select the check box at the bottom of the page to enable the Container Insights default Opt\-In\.

If you haven't used the preceding procedure to enable Container Insights on all new clusters by default, you can use the following steps to create a cluster with Container Insights enabled\.

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

## Setting Up Container Insights on Amazon ECS Using the CLI<a name="deploy-container-insights-ECS-CLI"></a>

To enable Container Insights on all new clusters by default, enter the following\.

```
aws ecs put-account-setting --name "containerInsights" --value "enabled"
```

If you didn't use the preceding command to enable Container Insights on all new clusters by default, enter the following command to create a new cluster with Container Insights enabled\.

```
aws ecs create-cluster --cluster-name myCICluster --settings "name=containerInsights,value=enabled"
```
# Sample App Mesh Workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-Sample-Workloads-ECS-appmesh"></a>

To collect metrics from a sample Prometheus workload for Amazon ECS, you must be running Container Insights in the cluster\. For information about installing Container Insights, see [Setting Up Container Insights on Amazon ECS](deploy-container-insights-ECS.md)\.

First, follow this [ walkthrough](https://github.com/aws/aws-app-mesh-examples/tree/master/examples/apps/colorapp#app-mesh-walkthrough-deploy-the-color-app-on-ecs) to deploy the sample color app on your Amazon ECS cluster\. After you finish, you will have App Mesh Prometheus metrics exposed on port 9901\.

Next, follow these steps to install the CloudWatch agent with Prometheus monitoring on the same Amazon ECS cluster where you installed the color app\. The steps in this section install the CloudWatch agent in bridge network mode\. 

The environment variables `ENVIRONMENT_NAME`, `AWS_PROFILE`, and `AWS_DEFAULT_REGION` that you set in the walkthrough will also be used in the following steps\.

**To install the CloudWatch agent with Prometheus monitoring for testing**

1. Download the AWS CloudFormation template by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-bridge-host.yaml
   ```

1. Set the network mode by entering the following commands\.

   ```
   export ECS_CLUSTER_NAME=${ENVIRONMENT_NAME}
   export ECS_NETWORK_MODE=bridge
   ```

1. Create the AWS CloudFormation stack by entering the following commands\.

   ```
   aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
       --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=${ECS_CLUSTER_NAME} \
                    ParameterKey=CreateIAMRoles,ParameterValue=True \
                    ParameterKey=ECSNetworkMode,ParameterValue=${ECS_NETWORK_MODE} \
                    ParameterKey=TaskRoleName,ParameterValue=CWAgent-Prometheus-TaskRole-${ECS_CLUSTER_NAME} \
                    ParameterKey=ExecutionRoleName,ParameterValue=CWAgent-Prometheus-ExecutionRole-${ECS_CLUSTER_NAME} \
       --capabilities CAPABILITY_NAMED_IAM \
       --region ${AWS_DEFAULT_REGION} \
       --profile ${AWS_PROFILE}
   ```

1. \(Optional\) When the AWS CloudFormation stack is created, you see a CREATE\_COMPLETE message\. If you to check the status before you see that message, enter the following command\.

   ```
   aws cloudformation describe-stacks \
   --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
   --query 'Stacks[0].StackStatus' \
   --region ${AWS_DEFAULT_REGION} \
   --profile ${AWS_PROFILE}
   ```

**Troubleshooting**

The steps in the walkthrough use jq to parse the output result of the AWS CLI\. For more information about installing jq, see [ jq](https://stedolan.github.io/jq/)\. Use the following command to set the default output format of your AWS CLI to JSON so jq can parse it correctly\. 

```
$ aws configure
```

When the response gets to Default output format, enter **json**\.

## Uninstall the CloudWatch agent with Prometheus monitoring<a name="ContainerInsights-Prometheus-Sample-Workloads-ECS-appmesh-uninstall"></a>

When you are finished testing, enter the following command to uninstall the CloudWatch agent by deleting the AWS CloudFormation stack\.

```
aws cloudformation delete-stack \
--stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
--region ${AWS_DEFAULT_REGION} \
--profile ${AWS_PROFILE}
```
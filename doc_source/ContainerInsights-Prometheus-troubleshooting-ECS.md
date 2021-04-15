# Prometheus Metrics Troubleshooting on Amazon ECS<a name="ContainerInsights-Prometheus-troubleshooting-ECS"></a>

This section provides help for troubleshooting your Prometheus metrics setup on Amazon ECS clusters\. 

## I don't see Prometheus metrics sent to CloudWatch Logs<a name="ContainerInsights-Prometheus-troubleshooting-ECS-nometrics"></a>

The Prometheus metrics should be ingested as log events in the log group **/aws/ecs/containerinsights/cluster\-name/Prometheus**\. If the log group is not created or the Prometheus metrics are not sent to the log group, you will need to first check whether the Prometheus targets have been successfully discovered by the CloudWatch agent\. And next check the security group and permission settings of the CloudWatch agent\. The following steps guide you to do the debugging\.

**Step 1: Enable the CloudWatch agent debugging mode**

First, change the CloudWatch agent to debug mode by adding the following bold lines to your AWS CloudFormation template file, `cwagent-ecs-prometheus-metric-for-bridge-host.yaml` or `cwagent-ecs-prometheus-metric-for-awsvpc.yaml`\. Then save the file\.

```
cwagentconfig.json: |
    {
      "agent": {
        "debug": true
      },
      "logs": {
        "metrics_collected": {
```

Create a new AWS CloudFormation changeset against the existing stack\. Set other parameters in the changeset to the same values as in your existing AWS CloudFormation stack\. The following example is for a CloudWatch agent installed in an Amazon ECS cluster using the EC2 launch type and the bridge network mode\.

```
ECS_NETWORK_MODE=bridge
 CREATE_IAM_ROLES=True
ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name
NEW_CHANGESET_NAME=your_selected_ecs_execution_role_name

aws cloudformation create-change-set --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
    --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
    --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                 ParameterKey=CreateIAMRoles,ParameterValue=$CREATE_IAM_ROLES \
                 ParameterKey=ECSNetworkMode,ParameterValue=$ECS_NETWORK_MODE \
                 ParameterKey=TaskRoleName,ParameterValue=$ECS_TASK_ROLE_NAME \
                 ParameterKey=ExecutionRoleName,ParameterValue=$ECS_EXECUTION_ROLE_NAME \
    --capabilities CAPABILITY_NAMED_IAM \
    --region $AWS_REGION \
    --change-set-name $NEW_CHANGESET_NAME
```

Go to the AWS CloudFormation console to review the new changeset, `$NEW_CHANGESET_NAME`\. There should be one change applied to the **CWAgentConfigSSMParameter** resource\. Execute the changeset and restart the CloudWatch agent task by entering the following commands\.

```
aws ecs update-service --cluster $ECS_CLUSTER_NAME \
--desired-count 0 \
--service your_service_name_here \
--region $AWS_REGION
```

Wait about 10 seconds and then enter the following command\.

```
aws ecs update-service --cluster $ECS_CLUSTER_NAME \
--desired-count 1 \
--service your_service_name_here \
--region $AWS_REGION
```

**Step 2: Check the ECS service discovery logs**

The ECS task definition of the CloudWatch agent enables the logs by default in the section below\. The logs are sent to CloudWatch Logs in the log group **/ecs/ecs\-cwagent\-prometheus**\.

```
LogConfiguration:
  LogDriver: awslogs
    Options:
      awslogs-create-group: 'True'
      awslogs-group: "/ecs/ecs-cwagent-prometheus"
      awslogs-region: !Ref AWS::Region
      awslogs-stream-prefix: !Sub 'ecs-${ECSLaunchType}-awsvpc'
```

Filter the logs by the string `ECS_SD_Stats` to get the metrics related to the ECS service discovery, as shown in the following example\.

```
2020-09-1T01:53:14Z D! ECS_SD_Stats: AWSCLI_DescribeContainerInstances: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: AWSCLI_DescribeInstancesRequest: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: AWSCLI_DescribeTaskDefinition: 2
2020-09-1T01:53:14Z D! ECS_SD_Stats: AWSCLI_DescribeTasks: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: AWSCLI_ListTasks: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: Exporter_DiscoveredTargetCount: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: LRUCache_Get_EC2MetaData: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: LRUCache_Get_TaskDefinition: 2
2020-09-1T01:53:14Z D! ECS_SD_Stats: LRUCache_Size_ContainerInstance: 1
2020-09-1T01:53:14Z D! ECS_SD_Stats: LRUCache_Size_TaskDefinition: 2
2020-09-1T01:53:14Z D! ECS_SD_Stats: Latency: 43.399783ms
```

The meaning of each metric for a particular ECS service discovery cycle is as follows:
+ **AWSCLI\_DescribeContainerInstances** – the number of `ECS::DescribeContainerInstances` API calls made\.
+ **AWSCLI\_DescribeInstancesRequest** – the number of `ECS::DescribeInstancesRequest` API calls made\.
+ **AWSCLI\_DescribeTaskDefinition** – the number of `ECS::DescribeTaskDefinition` API calls made\.
+ **AWSCLI\_DescribeTasks** – the number of `ECS::DescribeTasks` API calls made\.
+ **AWSCLI\_ListTasks** – the number of `ECS::ListTasks` API calls made\.
+ **ExporterDiscoveredTargetCount** – the number of Prometheus targets that were discovered and successfully exported into the target result file within the container\.
+ **LRUCache\_Get\_EC2MetaData** – the number of times that container instances metadata was retrieved from the cache\.
+ **LRUCache\_Get\_TaskDefinition** – the number of times that ECS task definition metadata was retrieved from the cache\.
+ **LRUCache\_Size\_ContainerInstance** – the number of unique container instance's metadata cached in memory\.
+ **LRUCache\_Size\_TaskDefinition** – the number of unique ECS task definitions cached in memory\.
+ **Latency** – how long the service discovery cycle takes\.

Check the value of `ExporterDiscoveredTargetCount` to see whether the discovered Prometheus targets match your expectations\. If not, the possible reasons are as follows:
+ The configuration of ECS service discovery might not match your application's setting\. For the docker label\-based service discovery, your target containers may not have the necessary docker label configured in the CloudWatch agent to auto discover them\. For the ECS task definition ARN regular expression\-based service discovery, the regex setting in the CloudWatch agent may not match your application’s task definition\. 
+ The CloudWatch agent’s ECS task role might not have permission to retrieve the metadata of ECS tasks\. Check that the CloudWatch agent has been granted the following read\-only permissions:
  + `ec2:DescribeInstances`
  + `ecs:ListTasks`
  + `ecs:DescribeContainerInstances`
  + `ecs:DescribeTasks`
  + `ecs:DescribeTaskDefinition`

**Step 3: Check the network connection and the ECS task role policy**

If there are still no log events sent to the target CloudWatch Logs log group even though the value of `Exporter_DiscoveredTargetCount` indicates that there are discovered Prometheus targets, this could be caused by one of the following:
+ The CloudWatch agent might not be able to connect to the Prometheus target ports\. Check the security group setting behind the CloudWatch agent\. The private IP should alow the CloudWatch agent to connect to the Prometheus exporter ports\. 
+ The CloudWatch agent’s ECS task role might not have the **CloudWatchAgentServerPolicy** managed policy\. The CloudWatch agent’s ECS task role needs to have this policy to be able to send the Prometheus metrics as log events\. If you used the sample AWS CloudFormation template to create the IAM roles automatically, both the ECS task role and the ECS execution role are granted with the least privilege to perform the Prometheus monitoring\. 
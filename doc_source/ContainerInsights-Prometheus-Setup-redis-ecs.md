# Tutorial for scraping Redis Prometheus metrics on Amazon ECS Fargate<a name="ContainerInsights-Prometheus-Setup-redis-ecs"></a>

This tutorial provides a hands\-on introduction to scrape the Prometheus metrics of a sample Redis application in an Amazon ECS Fargate cluster\. The Redis Prometheus exporter target will be auto\-discovered by the CloudWatch agent with Prometheus metric support based on the container’s docker labels\.

Redis \(https://redis\.io/\) is an open source \(BSD licensed\), in\-memory data structure store, used as a database, cache and message broker\. For more information, see [ redis](https://redis.io/)\.

redis\_exporter \(MIT License licensed\) is used to expose the Redis prometheus metrics on the specified port \(default: 0\.0\.0\.0:9121\)\. For more information, see [ redis\_exporter](https://github.com/oliver006/redis_exporter)\.

The Docker images in the following two Docker Hub repositories are used in this tutorial: 
+ [ redis](https://hub.docker.com/_/redis?tab=description)
+ [ redis\_exporter](https://hub.docker.com/r/oliver006/redis_exporter)

**Prerequisite**

To collect metrics from a sample Prometheus workload for Amazon ECS, you must be running Container Insights in the cluster\. For information about installing Container Insights, see [Setting up Container Insights on Amazon ECS](deploy-container-insights-ECS.md)\.

**Topics**
+ [Set the Amazon ECS Fargate cluster environment variable](#ContainerInsights-Prometheus-Setup-redis-ecs-variable)
+ [Set the network environment variables for the Amazon ECS Fargate cluster](#ContainerInsights-Prometheus-Setup-redis-ecs-variable2)
+ [Install the sample Redis workload](#ContainerInsights-Prometheus-Setup-redis-ecs-install-workload)
+ [Configure the CloudWatch agent to scrape Redis Prometheus metrics](#ContainerInsights-Prometheus-Setup-redis-ecs-agent)
+ [Viewing your Redis metrics](#ContainerInsights-Prometheus-Setup-redis-view)

## Set the Amazon ECS Fargate cluster environment variable<a name="ContainerInsights-Prometheus-Setup-redis-ecs-variable"></a>

**To set the Amazon ECS Fargate cluster environment variable**

1. Install the Amazon ECS CLI if you haven't already done so\. For more information, see [ Installing the Amazon ECS CLI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html)\.

1. Set the new Amazon ECS cluster name and Region\. For example:

   ```
   ECS_CLUSTER_NAME=ecs-fargate-redis-tutorial
   AWS_DEFAULT_REGION=ca-central-1
   ```

1. \(Optional\) If you don't already have an Amazon ECS Fargate cluster where you want to install the sample Redis workload and CloudWatch agent, you can create one by entering the following command\.

   ```
   ecs-cli up --capability-iam \
   --cluster $ECS_CLUSTER_NAME \
   --launch-type FARGATE \
   --region $AWS_DEFAULT_REGION
   ```

   The expected result of this command is as follows:

   ```
   INFO[0000] Created cluster   cluster=ecs-fargate-redis-tutorial region=ca-central-1
   INFO[0001] Waiting for your cluster resources to be created...
   INFO[0001] Cloudformation stack status   stackStatus=CREATE_IN_PROGRESS
   VPC created: vpc-xxxxxxxxxxxxxxxxx
   Subnet created: subnet-xxxxxxxxxxxxxxxxx
   Subnet created: subnet-xxxxxxxxxxxxxxxxx
   Cluster creation succeeded.
   ```

## Set the network environment variables for the Amazon ECS Fargate cluster<a name="ContainerInsights-Prometheus-Setup-redis-ecs-variable2"></a>

**To set the network environment variables for the Amazon ECS Fargate cluster**

1. Set your VPC and subnet ID of the Amazon ECS cluster\. If you created a new cluster in the previous procedure, you'll see these values in the result of the final command\. Otherwise, use the IDs of the existing cluster that you are going to use with Redis\.

   ```
   ECS_CLUSTER_VPC=vpc-xxxxxxxxxxxxxxxxx
   ECS_CLUSTER_SUBNET_1=subnet-xxxxxxxxxxxxxxxxx
   ECS_CLUSTER_SUBNET_2=subnet-xxxxxxxxxxxxxxxxx
   ```

1. In this tutorial, we are going to install the Redis application and the CloudWatch agent in the default security group of the Amazon ECS cluster’s VPC\. The default security group allows all network connection within the same security group so the CloudWatch agent can scrape the Prometheus metrics exposed on the Redis containers\. In a real production environment, you might want to create dedicated security groups for the Redis application and CloudWatch agent and set customized permissions for them\. 

   Enter the following command to get the default security group ID\.

   ```
   aws ec2 describe-security-groups \
   --filters Name=vpc-id,Values=$ECS_CLUSTER_VPC  \
   --region $AWS_DEFAULT_REGION
   ```

   Then set the Fargate cluster deafult security group variable by entering the following command, replacing *my\-default\-security\-group* with the value you found from the previous command\.

   ```
   ECS_CLUSTER_SECURITY_GROUP=my-default-security-group
   ```

## Install the sample Redis workload<a name="ContainerInsights-Prometheus-Setup-redis-ecs-install-workload"></a>

**To install the sample Redis workload which exposes the Prometheus metrics**

1. Download the Redis AWS CloudFormation template by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/sample_traffic/redis/redis-traffic-sample.yaml
   ```

1. Set the IAM role names to be created for Redis by entering the following commands\.

   ```
   REDIS_ECS_TASK_ROLE_NAME=redis-prometheus-demo-ecs-task-role-name
   REDIS_ECS_EXECUTION_ROLE_NAME=redis-prometheus-demo-ecs-execution-role-name
   ```

1. Install the sample Redis workload by entering the following command\.

   ```
   aws cloudformation create-stack --stack-name Redis-Prometheus-Demo-ECS-$ECS_CLUSTER_NAME-fargate-awsvpc \
       --template-body file://redis-traffic-sample.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=SecurityGroupID,ParameterValue=$ECS_CLUSTER_SECURITY_GROUP \
                    ParameterKey=SubnetID,ParameterValue=$ECS_CLUSTER_SUBNET_1 \
                    ParameterKey=TaskRoleName,ParameterValue=$REDIS_ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$REDIS_ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region $AWS_DEFAULT_REGION
   ```

The AWS CloudFormation stack creates four resources:
+ One ECS task role
+ One ECS task execution role
+ One Redis task definition
+ One Redis service

In the Redis task definition, two containers are defined:
+ The primary container runs a simple Redis application and opens port 6379 for access\.
+ The other container runs the Redis exporter process to expose the Prometheus metrics on port 9121\. This is the container to be discovered and scraped by the CloudWatch agent\. The following docker label is defined so that the CloudWatch agent can discover this container based on it\.

  ```
  ECS_PROMETHEUS_EXPORTER_PORT: 9121
  ```

## Configure the CloudWatch agent to scrape Redis Prometheus metrics<a name="ContainerInsights-Prometheus-Setup-redis-ecs-agent"></a>

**To configure the CloudWatch agent to scrape Redis Prometheus metrics**

1. Download the latest version of `cwagent-ecs-prometheus-metric-for-awsvpc.yaml` by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-awsvpc.yaml
   ```

1. Open the file with a text editor, and find the full CloudWatch agent configuration behind the `value` key in the `resource:CWAgentConfigSSMParameter` section\.

   Then, in the `ecs_service_discovery` section shown here, the `docker_label`\-based service discovery is enabled with the default settings which are based on `ECS_PROMETHEUS_EXPORTER_PORT`, which matches the docker label we defined in the Redis ECS task definition\. So we do not need to make any changes in this section:

   ```
   ecs_service_discovery": {
     "sd_frequency": "1m",
     "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
   *  "docker_label": {
     },*
     ...
   ```

   For the `metric_declaration` section, the default setting does not allow any Redis metrics\. Add the following section to allow Redis metrics\. Be sure to follow the existing indentation pattern\.

   ```
   {
     "source_labels": ["container_name"],
     "label_matcher": "^redis-exporter-.*$",
     "dimensions": [["ClusterName","TaskDefinitionFamily"]],
     "metric_selectors": [
       "^redis_net_(in|out)put_bytes_total$",
       "^redis_(expired|evicted)_keys_total$",
       "^redis_keyspace_(hits|misses)_total$",
       "^redis_memory_used_bytes$",
       "^redis_connected_clients$"
     ]
   },
   {
     "source_labels": ["container_name"],
     "label_matcher": "^redis-exporter-.*$",
     "dimensions": [["ClusterName","TaskDefinitionFamily","cmd"]],
     "metric_selectors": [
       "^redis_commands_total$"
     ]
   },
   {
     "source_labels": ["container_name"],
     "label_matcher": "^redis-exporter-.*$",
     "dimensions": [["ClusterName","TaskDefinitionFamily","db"]],
     "metric_selectors": [
       "^redis_db_keys$"
     ]
   },
   ```

1. If you already have the CloudWatch agent deployed in the Amazon ECS cluster by AWS CloudFormation, you can create a change set by entering the following commands\.

   ```
   ECS_LAUNCH_TYPE=FARGATE
   CREATE_IAM_ROLES=True
   ECS_CLUSTER_SUBNET=$ECS_CLUSTER_SUBNET_1
   ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
   ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name
   
   aws cloudformation create-change-set --stack-name CWAgent-Prometheus-ECS-$ECS_CLUSTER_NAME-$ECS_LAUNCH_TYPE-awsvpc \
       --template-body file://cwagent-ecs-prometheus-metric-for-awsvpc.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=CreateIAMRoles,ParameterValue=$CREATE_IAM_ROLES \
                    ParameterKey=ECSLaunchType,ParameterValue=$ECS_LAUNCH_TYPE \
                    ParameterKey=SecurityGroupID,ParameterValue=$ECS_CLUSTER_SECURITY_GROUP \
                    ParameterKey=SubnetID,ParameterValue=$ECS_CLUSTER_SUBNET \
                    ParameterKey=TaskRoleName,ParameterValue=$ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region ${AWS_DEFAULT_REGION} \
       --change-set-name redis-scraping-support
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Review the newly created changeset `redis-scraping-support`\. You should see one change applied to the `CWAgentConfigSSMParameter` resource\. Execute the changeset and restart the CloudWatch agent task by entering the following commands\.

   ```
   aws ecs update-service --cluster $ECS_CLUSTER_NAME \
   --desired-count 0 \
   --service cwagent-prometheus-replica-service-$ECS_LAUNCH_TYPE-awsvpc \
   --region ${AWS_DEFAULT_REGION}
   ```

1. Wait about 10 seconds, and then enter the following command\.

   ```
   aws ecs update-service --cluster $ECS_CLUSTER_NAME \
   --desired-count 1 \
   --service cwagent-prometheus-replica-service-$ECS_LAUNCH_TYPE-awsvpc \
   --region ${AWS_DEFAULT_REGION}
   ```

1. If you are installing the CloudWatch agent with Prometheus metric collecting for the cluster for the first time, please enter the following commands:

   ```
   ECS_LAUNCH_TYPE=FARGATE
   CREATE_IAM_ROLES=True
   ECS_CLUSTER_SUBNET=$ECS_CLUSTER_SUBNET_1
   ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
   ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name
   
   aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-$ECS_CLUSTER_NAME-$ECS_LAUNCH_TYPE-awsvpc \
       --template-body file://cwagent-ecs-prometheus-metric-for-awsvpc.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=CreateIAMRoles,ParameterValue=$CREATE_IAM_ROLES \
                    ParameterKey=ECSLaunchType,ParameterValue=$ECS_LAUNCH_TYPE \
                    ParameterKey=SecurityGroupID,ParameterValue=$ECS_CLUSTER_SECURITY_GROUP \
                    ParameterKey=SubnetID,ParameterValue=$ECS_CLUSTER_SUBNET \
                    ParameterKey=TaskRoleName,ParameterValue=$ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region ${AWS_DEFAULT_REGION}
   ```

## Viewing your Redis metrics<a name="ContainerInsights-Prometheus-Setup-redis-view"></a>

This tutorial sends the following metrics to the **ECS/ContainerInsights/Prometheus** namespace in CloudWatch\. You can use the CloudWatch console to see the metrics in that namespace\.


| Metric Name | Dimensions | 
| --- | --- | 
|  `redis_net_input_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_net_output_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_expired_keys_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_evicted_keys_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_keyspace_hits_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_keyspace_misses_total` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_memory_used_bytes` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_connected_clients` |  ClusterName, TaskDefinitionFamily  | 
|  `redis_commands_total` |  ClusterName, TaskDefinitionFamily, cmd  | 
|  `redis_db_keys` |  ClusterName, TaskDefinitionFamily, db  | 

**Note**  
The value of the **cmd** dimension can be: `append`, `client`, `command`, `config`, `dbsize`, `flushall`, `get`, `incr`, `info`, `latency`, or `slowlog`\.  
The value of the **db** dimension can be `db0` to `db15`\. 

You can also create a CloudWatch dashboard for your Redis Prometheus metrics\.

**To create a dashboard for Redis Prometheus metrics**

1. Create environment variables, replacing the values below to match your deployment\.

   ```
   DASHBOARD_NAME=your_cw_dashboard_name
   ECS_TASK_DEF_FAMILY=redis-prometheus-demo-$ECS_CLUSTER_NAME-fargate-awsvpc
   ```

1. Enter the following command to create the dashboard\.

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/redis/cw_dashboard_redis.json \
   | sed "s/{{YOUR_AWS_REGION}}/${REGION_NAME}/g" \
   | sed "s/{{YOUR_CLUSTER_NAME}}/${CLUSTER_NAME}/g" \
   | sed "s/{{YOUR_NAMESPACE}}/${NAMESPACE}/g" \
   ```
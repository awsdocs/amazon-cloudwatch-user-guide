# Tutorial for adding a new Prometheus scrape target: Memcached on Amazon ECS<a name="ContainerInsights-Prometheus-Setup-memcached-ecs"></a>

This tutorial provides a hands\-on introduction to scrape the Prometheus metrics of a sample Memcached application on an Amazon Amazon ECS cluster with the EC2 launch type\. The Memcached Prometheus exporter target will be auto\-discovered by the CloudWatch agent by ECS task definition\-based service discovery\.

Memcached is a general\-purpose distributed memory caching system\. It is often used to speed up dynamic database\-driven websites by caching data and objects in RAM to reduce the number of times an external data source \(such as a database or API\) must be read\. For more infromation, see [ What is Memcached?](https://www.memcached.org/)

The [ memchached\_exporter](https://github.com/prometheus/memcached_exporter) \(Apache License 2\.0\) is one of the Prometheus official exporters\. By default the memcache\_exporter serves on port 0\.0\.0\.0:9150 at `/metrics.`

The Docker images in the following two Docker Hub repositories are used in this tutorial: 
+ [ Memcached](https://hub.docker.com/_/memcached?tab=description)
+ [ prom/memcached\-exporter](https://hub.docker.com/r/prom/memcached-exporter/)

**Prerequisite**

To collect metrics from a sample Prometheus workload for Amazon ECS, you must be running Container Insights in the cluster\. For information about installing Container Insights, see [Setting up Container Insights on Amazon ECS](deploy-container-insights-ECS.md)\.

**Topics**
+ [Set the Amazon ECS EC2 cluster environment variables](#ContainerInsights-Prometheus-Setup-memcached-ecs-environment)
+ [Install the sample Memcached workload](#ContainerInsights-Prometheus-Setup-memcached-ecs-install-workload)
+ [Configure the CloudWatch agent to scrape Memcached Prometheus metrics](#ContainerInsights-Prometheus-Setup-memcached-ecs-agent)
+ [Viewing your Memcached metrics](#ContainerInsights-Prometheus-ECS-memcached-view)

## Set the Amazon ECS EC2 cluster environment variables<a name="ContainerInsights-Prometheus-Setup-memcached-ecs-environment"></a>

**To set the Amazon ECS EC2 cluster environment variables**

1. Install the Amazon ECS CLI if you haven't already done so\. For more information, see [ Installing the Amazon ECS CLI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html)\.

1. Set the new Amazon ECS cluster name and Region\. For example:

   ```
   ECS_CLUSTER_NAME=ecs-ec2-memcached-tutorial
   AWS_DEFAULT_REGION=ca-central-1
   ```

1. \(Optional\) If you don't already have an Amazon ECS cluster with the EC2 launch type where you want to install the sample Memcached workload and CloudWatch agent, you can create one by entering the following command\.

   ```
   ecs-cli up --capability-iam --size 1 \
   --instance-type t3.medium \
   --cluster $ECS_CLUSTER_NAME \
   --region $AWS_REGION
   ```

   The expected result of this command is as follows:

   ```
   WARN[0000] You will not be able to SSH into your EC2 instances without a key pair. 
   INFO[0000] Using recommended Amazon Linux 2 AMI with ECS Agent 1.44.4 and Docker version 19.03.6-ce 
   INFO[0001] Created cluster                               cluster=ecs-ec2-memcached-tutorial region=ca-central-1
   INFO[0002] Waiting for your cluster resources to be created... 
   INFO[0002] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   INFO[0063] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   INFO[0124] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
   VPC created: vpc-xxxxxxxxxxxxxxxxx
   Security Group created: sg-xxxxxxxxxxxxxxxxx
   Subnet created: subnet-xxxxxxxxxxxxxxxxx
   Subnet created: subnet-xxxxxxxxxxxxxxxxx
   Cluster creation succeeded.
   ```

## Install the sample Memcached workload<a name="ContainerInsights-Prometheus-Setup-memcached-ecs-install-workload"></a>

**To install the sample Memcached workload which exposes the Prometheus metrics**

1. Download the Memcached AWS CloudFormation template by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/sample_traffic/memcached/memcached-traffic-sample.yaml
   ```

1. Set the IAM role names to be created for Memcached by entering the following commands\.

   ```
   MEMCACHED_ECS_TASK_ROLE_NAME=memcached-prometheus-demo-ecs-task-role-name
   MEMCACHED_ECS_EXECUTION_ROLE_NAME=memcached-prometheus-demo-ecs-execution-role-name
   ```

1. Install the sample Memcached workload by entering the following command\. This sample installs the workload in `host` network mode\.

   ```
   MEMCACHED_ECS_NETWORK_MODE=host
   
   aws cloudformation create-stack --stack-name Memcached-Prometheus-Demo-ECS-$ECS_CLUSTER_NAME-EC2-$MEMCACHED_ECS_NETWORK_MODE \
       --template-body file://memcached-traffic-sample.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=ECSNetworkMode,ParameterValue=$MEMCACHED_ECS_NETWORK_MODE \
                    ParameterKey=TaskRoleName,ParameterValue=$MEMCACHED_ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$MEMCACHED_ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region $AWS_REGION
   ```

The AWS CloudFormation stack creates four resources:
+ One ECS task role
+ One ECS task execution role
+ One Memcached task definition
+ One Memcached service

In the Memcached task definition, two containers are defined:
+ The primary container runs a simple Memcached application and opens port 11211 for access\.
+ The other container runs the Redis exporter process to expose the Prometheus metrics on port 9150\. This is the container to be discovered and scraped by the CloudWatch agent\.

## Configure the CloudWatch agent to scrape Memcached Prometheus metrics<a name="ContainerInsights-Prometheus-Setup-memcached-ecs-agent"></a>

**To configure the CloudWatch agent to scrape Memcached Prometheus metrics**

1. Download the latest version of `cwagent-ecs-prometheus-metric-for-awsvpc.yaml` by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-awsvpc.yaml
   ```

1. Open the file with a text editor, and find the full CloudWatch agent configuration behind the `value` key in the `resource:CWAgentConfigSSMParameter` section\.

   Then, in the `ecs_service_discovery` section, add the following configuration into the `task_definition_list` section\.

   ```
   {
       "sd_job_name": "ecs-memcached",
       "sd_metrics_ports": "9150",
       "sd_task_definition_arn_pattern": ".*:task-definition/memcached-prometheus-demo.*:[0-9]+"
   },
   ```

   For the `metric_declaration` section, the default setting does not allow any Memcached metrics\. Add the following section to allow Memcached metrics\. Be sure to follow the existing indentation pattern\.

   ```
   {
     "source_labels": ["container_name"],
     "label_matcher": "memcached-exporter-.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily"]],
     "metric_selectors": [
       "^memcached_current_(bytes|items|connections)$",
       "^memcached_items_(reclaimed|evicted)_total$",
       "^memcached_(written|read)_bytes_total$",
       "^memcached_limit_bytes$",
       "^memcached_commands_total$"
     ]
   },
   {
     "source_labels": ["container_name"],
     "label_matcher": "memcached-exporter-.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily","status","command"], ["ClusterName", "TaskDefinitionFamily","command"]],
     "metric_selectors": [
       "^memcached_commands_total$"
     ]
   },
   ```

1. If you already have the CloudWatch agent deployed in the Amazon ECS cluster by AWS CloudFormation, you can create a change set by entering the following commands\.

   ```
   ECS_NETWORK_MODE=bridge
   CREATE_IAM_ROLES=True
   ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
   ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name
   
   aws cloudformation create-change-set --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
       --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=CreateIAMRoles,ParameterValue=$CREATE_IAM_ROLES \
                    ParameterKey=ECSNetworkMode,ParameterValue=$ECS_NETWORK_MODE \
                    ParameterKey=TaskRoleName,ParameterValue=$ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region $AWS_REGION \
       --change-set-name memcached-scraping-support
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Review the newly created changeset `memcached-scraping-support`\. You should see one change applied to the `CWAgentConfigSSMParameter` resource\. Execute the changeset and restart the CloudWatch agent task by entering the following commands\.

   ```
   aws ecs update-service --cluster $ECS_CLUSTER_NAME \
   --desired-count 0 \
   --service cwagent-prometheus-replica-service-EC2-$ECS_NETWORK_MODE \
   --region $AWS_REGION
   ```

1. Wait about 10 seconds, and then enter the following command\.

   ```
   aws ecs update-service --cluster $ECS_CLUSTER_NAME \
   --desired-count 1 \
   --service cwagent-prometheus-replica-service-EC2-$ECS_NETWORK_MODE \
   --region $AWS_REGION
   ```

1. If you are installing the CloudWatch agent with Prometheus metric collecting for the cluster for the first time, please enter the following commands:

   ```
   ECS_NETWORK_MODEE=bridge
   CREATE_IAM_ROLES=True
   ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
   ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name
   
   aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
       --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
       --parameters ParameterKey=ECSClusterName,ParameterValue=$ECS_CLUSTER_NAME \
                    ParameterKey=CreateIAMRoles,ParameterValue=$CREATE_IAM_ROLES \
                    ParameterKey=ECSNetworkMode,ParameterValue=$ECS_NETWORK_MODE \
                    ParameterKey=TaskRoleName,ParameterValue=$ECS_TASK_ROLE_NAME \
                    ParameterKey=ExecutionRoleName,ParameterValue=$ECS_EXECUTION_ROLE_NAME \
       --capabilities CAPABILITY_NAMED_IAM \
       --region $AWS_REGION
   ```

## Viewing your Memcached metrics<a name="ContainerInsights-Prometheus-ECS-memcached-view"></a>

This tutorial sends the following metrics to the **ECS/ContainerInsights/Prometheus** namespace in CloudWatch\. You can use the CloudWatch console to see the metrics in that namespace\.


| Metric name | Dimensions | 
| --- | --- | 
|  `memcached_current_items` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_current_connections` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_limit_bytes` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_current_bytes` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_written_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_read_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_items_evicted_total` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_items_reclaimed_total` |  ClusterName, TaskDefinitionFamily  | 
|  `memcached_commands_total` |  ClusterName, TaskDefinitionFamily ClusterName, TaskDefinitionFamily, command ClusterName, TaskDefinitionFamily, status, command  | 

**Note**  
The value of the **command** dimension can be: `delete`, `get`, `cas`, `set`, `decr`, `touch`, `incr`, or `flush`\.  
The value of the **status** dimension can be `hit`, `miss`, or `badval`\. 

You can also create a CloudWatch dashboard for your Memcached Prometheus metrics\.

**To create a dashboard for Memcached Prometheus metrics**

1. Create environment variables, replacing the values below to match your deployment\.

   ```
   DASHBOARD_NAME=your_memcached_cw_dashboard_name
   ECS_TASK_DEF_FAMILY=memcached-prometheus-demo-$ECS_CLUSTER_NAME-EC2-$MEMCACHED_ECS_NETWORK_MOD
   ```

1. Enter the following command to create the dashboard\.

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/sample_cloudwatch_dashboards/memcached/cw_dashboard_memcached.json \
   | sed "s/{{YOUR_AWS_REGION}}/$AWS_REGION/g" \
   | sed "s/{{YOUR_CLUSTER_NAME}}/$ECS_CLUSTER_NAME/g" \
   | sed "s/{{YOUR_TASK_DEF_FAMILY}}/$ECS_TASK_DEF_FAMILY/g" \
   | xargs -0 aws cloudwatch put-dashboard --dashboard-name ${DASHBOARD_NAME} --region $AWS_REGION --dashboard-body
   ```
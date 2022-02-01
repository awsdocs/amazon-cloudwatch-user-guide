# Sample NGINX workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-Setup-nginx-ecs"></a>

The NGINX Prometheus exporter can scrape and expose NGINX data as Prometheus metrics\. This example uses the exporter in tandem with the NGINX reverse proxy service for Amazon ECS\.

For more information about the NGINX Prometheus exporter, see [ nginx\-prometheus\-exporter](https://github.com/nginxinc/nginx-prometheus-exporter) on Github\. For more information about the NGINX reverse proxy, see [ ecs\-nginx\-reverse\-proxy](https://github.com/awslabs/ecs-nginx-reverse-proxy) on Github\.

The CloudWatch agent with Prometheus support scrapes the NGINX Prometheus metrics based on the service discovery configuration in the Amazon ECS cluster\. You can configure the NGINX Prometheus Exporter to expose the metrics on a different port or path\. If you change the port or path, update the `ecs_service_discovery` section in the CloudWatch agent configuration file\.

## Install the NGINX reverse proxy sample workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-nginx-ecs-setup"></a>

Follow these steps to install the NGINX reverse proxy sample workload\.

### Create the Docker images<a name="ContainerInsights-Prometheus-nginx-ecs-setup-docker"></a>

**To create the Docker images for the NGINX reverse proxy sample workload**

1. Download the following folder from the NGINX reverse proxy repo: [ https://github\.com/awslabs/ecs\-nginx\-reverse\-proxy/tree/master/reverse\-proxy/](https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master/reverse-proxy/)\.

1. Find the `app` directory and build an image from that directory:

   ```
   docker build -t web-server-app ./path-to-app-directory
   ```

1. Build a custom image for NGINX\. First, create a directory with the following two files:
   + A sample Dockerfile:

     ```
     FROM nginx
     COPY nginx.conf /etc/nginx/nginx.conf
     ```
   + An `nginx.conf` file, modified from [ https://github\.com/awslabs/ecs\-nginx\-reverse\-proxy/tree/master/reverse\-proxy/](https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master/reverse-proxy/):

     ```
     events {
       worker_connections 768;
     }
     
     http {
       # Nginx will handle gzip compression of responses from the app server
       gzip on;
       gzip_proxied any;
       gzip_types text/plain application/json;
       gzip_min_length 1000;
     
       server{
         listen 8080;
         location /stub_status {
             stub_status   on;
         }
       }
     
       server {
         listen 80;
     
         # Nginx will reject anything not matching /api
         location /api {
           # Reject requests with unsupported HTTP method
           if ($request_method !~ ^(GET|POST|HEAD|OPTIONS|PUT|DELETE)$) {
             return 405;
           }
     
           # Only requests matching the whitelist expectations will
           # get sent to the application server
           proxy_pass http://app:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_cache_bypass $http_upgrade;
         }
       }
     }
     ```
**Note**  
`stub_status` must be enabled on the same port that `nginx-prometheus-exporter` is configured to scrape metrics from\. In our example task definition, `nginx-prometheus-exporter` is configured to scrape metrics from port 8080\.

1. Build an image from files in your new directory:

   ```
   docker build -t nginx-reverse-proxy ./path-to-your-directory
   ```

1. Upload your new images to an image repository for later use\.

### Create the task definition to run NGINX and the web server app in Amazon ECS<a name="ContainerInsights-Prometheus-nginx-ecs-setup-task"></a>

Next, you set up the task definition\.

This task definition enables the collection and export of NGINX Prometheus metrics\. The NGINX container tracks input from the app, and exposes that data to port 8080, as set in `nginx.conf`\. The NGINX prometheus exporter container scrapes these metrics, and posts them to port 9113, for use in CloudWatch\.

**To set up the task definition for the NGINX sample Amazon ECS workload**

1. Create a task definition JSON file with the following content\. Replace *your\-customized\-nginx\-iamge* with the image URI for your customized NGINX image, and replace *your\-web\-server\-app\-image* with the image URI for your web server app image\.

   ```
   {
     "containerDefinitions": [
       {
         "name": "nginx",
         "image": "your-customized-nginx-image",
         "memory": 256,
         "cpu": 256,
         "essential": true,
         "portMappings": [
           {
             "containerPort": 80,
             "protocol": "tcp"
           }
         ],
         "links": [
           "app"
         ]
       },
       {
         "name": "app",
         "image": "your-web-server-app-image",
         "memory": 256,
         "cpu": 256,
         "essential": true
       },
       {
         "name": "nginx-prometheus-exporter",
         "image": "docker.io/nginx/nginx-prometheus-exporter:0.8.0",
         "memory": 256,
         "cpu": 256,
         "essential": true,
         "command": [
           "-nginx.scrape-uri",
           "http://nginx:8080/stub_status"
       ],
       "links":[
         "nginx"
       ],
         "portMappings":[
           {
             "containerPort": 9113,
             "protocol": "tcp"
           }
         ]
       }
     ],
     "networkMode": "bridge",
     "placementConstraints": [],
     "family": "nginx-sample-stack"
   }
   ```

1. Register the task definition by entering the following command\.

   ```
   aws ecs register-task-definition --cli-input-json file://path-to-your-task-definition-json
   ```

1. Create a service to run the task by entering the following command:

   Be sure not to change the service name\. We will be running a CloudWatch agent service using a configuration that searches for tasks using the name patterns of the services that started them\. For example, for the CloudWatch agent to find the task launched by this command, you can specify the value of `sd_service_name_pattern` to be `^nginx-service$`\. The next section provides more details\.

   ```
   aws ecs create-service \
    --cluster your-cluster-name \
    --service-name nginx-service \
    --task-definition nginx-sample-stack:1 \
    --desired-count 1
   ```

### Configure the CloudWatch agent to scrape NGINX Prometheus metrics<a name="ContainerInsights-Prometheus-nginx-ecs-setup-agent"></a>

The final step is to configure the CloudWatch agent to scrape the NGINX metrics\. In this example, the CloudWatch agent discovers the task via the service name pattern, and the port 9113, where the exporter exposes the prometheus metrics for NGINX\. With the task discovered and the metrics available, the CloudWatch agent begins posting the collected metrics to the log stream **nginx\-prometheus\-exporter**\. 

**To configure the CloudWatch agent to scrape the NGINX metrics**

1. Download the latest version of the necessary YAML file by entering the following command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-bridge-host.yaml
   ```

1. Open the file with a text editor, and find the full CloudWatch agent confguration in the `value` key in the `resource:CWAgentConfigSSMParameter` section\. Then, in the `ecs_service_discovery` section, add the following `service_name_list_for_tasks` section\.

   ```
   "service_name_list_for_tasks": [
     {
       "sd_job_name": "nginx-prometheus-exporter",
       "sd_metrics_path": "/metrics",
       "sd_metrics_ports": "9113",
       "sd_service_name_pattern": "^nginx-service$"
      }
   ],
   ```

1. In the same file, add the following section in the `metric_declaration` section to allow NGINX metrics\. Be sure to follow the existing indentation pattern\.

   ```
   {
     "source_labels": ["job"],
     "label_matcher": ".*nginx.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily", "ServiceName"]],
     "metric_selectors": [
       "^nginx_.*$"
     ]
   },
   ```

1. If you don't already have the CloudWatch agent deployed in this cluster, skip to step 8\.

   If you already have the CloudWatch agent deployed in the Amazon ECS cluster by using AWS CloudFormation, you can create a change set by entering the following commands:

   ```
   ECS_CLUSTER_NAME=your_cluster_name
   AWS_REGION=your_aws_region
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
       --change-set-name nginx-scraping-support
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Revew the newly\-created changeset **nginx\-scraping\-support**\. You should see one change applied to the **CWAgentConfigSSMParameter** resource\. Run the changeset and restart the CloudWatch agent task by entering the following command:

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

1. If you are installing the CloudWatch agent with Prometheus metric collecting on the cluster for the first time, enter the following commands\.

   ```
   ECS_CLUSTER_NAME=your_cluster_name
   AWS_REGION=your_aws_region
   ECS_NETWORK_MODE=bridge
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

## Viewing your NGINX metrics and logs<a name="ContainerInsights-Prometheus-Setup-nginx-view"></a>

You can now view the NGINX metrics being collected\.

**To view the metrics for your sample NGINX workload**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the Region where your cluster is running, choose **Metrics** in the left navigation pane\. Find the **ContainerInsights/Prometheus** namespace to see the metrics\.

1. To see the CloudWatch Logs events, choose **Log groups** in the navigation pane\. The events are in the log group **/aws/containerinsights/*your\_cluster\_name*/prometheus**, in the log stream *nginx\-prometheus\-exporter*\.
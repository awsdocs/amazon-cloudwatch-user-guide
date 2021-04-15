# Sample NGINX Plus workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-Setup-nginx-plus-ecs"></a>

NGINX Plus is the commerical version of NGINX\. You must have a licence to use it\. For more information, see [ NGINX Plus](https://www.nginx.com/products/nginx/)

The NGINX Prometheus exporter can scrape and expose NGINX data as Prometheus metrics\. This example uses the exporter in tandem with the NGINX Plus reverse proxy service for Amazon ECS\.

For more information about the NGINX Prometheus exporter, see [ nginx\-prometheus\-exporter](https://github.com/nginxinc/nginx-prometheus-exporter) on Github\. For more information about the NGINX reverse proxy, see [ ecs\-nginx\-reverse\-proxy](https://github.com/awslabs/ecs-nginx-reverse-proxy) on Github\.

The CloudWatch agent with Prometheus support scrapes the NGINX Plus Prometheus metrics based on the service discovery configuration in the Amazon ECS cluster\. You can configure the NGINX Prometheus Exporter to expose the metrics on a different port or path\. If you change the port or path, update the `ecs_service_discovery` section in the CloudWatch agent configuration file\.

## Install the NGINX Plus reverse proxy sample workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-nginx-plus-ecs-setup"></a>

Follow these steps to install the NGINX reverse proxy sample workload\.

### Create the docker images<a name="ContainerInsights-Prometheus-nginx-plus-ecs-setup-docker"></a>

**To create the Docker images for the NGINX Plus reverse proxy sample workload**

1. Download the following folder from the NGINX reverse proxy repo: [ https://github\.com/awslabs/ecs\-nginx\-reverse\-proxy/tree/master/reverse\-proxy/](https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master/reverse-proxy/)\.

1. Find the `app` directory and build an image from that directory:

   ```
   docker build -t web-server-app ./path-to-app-directory
   ```

1. Build a custom image for NGINX Plus\. Before you can build the image for NGINX Plus, you need to obtain the key named `nginx-repo.key` and the SSL certificate `nginx-repo.crt` for your licensed NGINX Plus\. Create a directory and store in it your `nginx-repo.key` and `nginx-repo.crt` files\. 

   In the directory that you just created, create the following two files:
   + A sample Dockerfile with the following content\. This docker file is adopted from a sample file provided at [https://docs\.nginx\.com/nginx/admin\-guide/installing\-nginx/installing\-nginx\-docker/\#docker\_plus\_image](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-docker/#docker_plus_image)\. The important change that we make is that we load a separate file, called `nginx.conf`, which will be created in the next step\.

     ```
     FROM debian:buster-slim
     
     LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com>“
     
     # Define NGINX versions for NGINX Plus and NGINX Plus modules
     # Uncomment this block and the versioned nginxPackages block in the main RUN
     # instruction to install a specific release
     # ENV NGINX_VERSION 21
     # ENV NJS_VERSION 0.3.9
     # ENV PKG_RELEASE 1~buster
     
     # Download certificate and key from the customer portal (https://cs.nginx.com (https://cs.nginx.com/))
     # and copy to the build context
     COPY nginx-repo.crt /etc/ssl/nginx/
     COPY nginx-repo.key /etc/ssl/nginx/
     # COPY nginx.conf /etc/ssl/nginx/nginx.conf
     
     RUN set -x \
     # Create nginx user/group first, to be consistent throughout Docker variants
     && addgroup --system --gid 101 nginx \
     && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false --uid 101 nginx \
     && apt-get update \
     && apt-get install --no-install-recommends --no-install-suggests -y ca-certificates gnupg1 \
     && \
     NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
     found=''; \
     for server in \
     ha.pool.sks-keyservers.net (http://ha.pool.sks-keyservers.net/) \
     hkp://keyserver.ubuntu.com:80 \
     hkp://p80.pool.sks-keyservers.net:80 \
     pgp.mit.edu (http://pgp.mit.edu/) \
     ; do \
     echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
     apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
     done; \
     test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
     apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/* \
     # Install the latest release of NGINX Plus and/or NGINX Plus modules
     # Uncomment individual modules if necessary
     # Use versioned packages over defaults to specify a release
     && nginxPackages=" \
     nginx-plus \
     # nginx-plus=${NGINX_VERSION}-${PKG_RELEASE} \
     # nginx-plus-module-xslt \
     # nginx-plus-module-xslt=${NGINX_VERSION}-${PKG_RELEASE} \
     # nginx-plus-module-geoip \
     # nginx-plus-module-geoip=${NGINX_VERSION}-${PKG_RELEASE} \
     # nginx-plus-module-image-filter \
     # nginx-plus-module-image-filter=${NGINX_VERSION}-${PKG_RELEASE} \
     # nginx-plus-module-perl \
     # nginx-plus-module-perl=${NGINX_VERSION}-${PKG_RELEASE} \
     # nginx-plus-module-njs \
     # nginx-plus-module-njs=${NGINX_VERSION}+${NJS_VERSION}-${PKG_RELEASE} \
     " \
     && echo "Acquire::https::plus-pkgs.nginx.com::Verify-Peer \"true\";" >> /etc/apt/apt.conf.d/90nginx \
     && echo "Acquire::https::plus-pkgs.nginx.com::Verify-Host \"true\";" >> /etc/apt/apt.conf.d/90nginx \
     && echo "Acquire::https::plus-pkgs.nginx.com::SslCert \"/etc/ssl/nginx/nginx-repo.crt\";" >> /etc/apt/apt.conf.d/90nginx \
     && echo "Acquire::https::plus-pkgs.nginx.com::SslKey \"/etc/ssl/nginx/nginx-repo.key\";" >> /etc/apt/apt.conf.d/90nginx \
     && printf "deb https://plus-pkgs.nginx.com/debian buster nginx-plus\n" > /etc/apt/sources.list.d/nginx-plus.list \
     && apt-get update \
     && apt-get install --no-install-recommends --no-install-suggests -y \
     $nginxPackages \
     gettext-base \
     curl \
     && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx-plus.list \
     && rm -rf /etc/apt/apt.conf.d/90nginx /etc/ssl/nginx
     
     # Forward request logs to Docker log collector
     RUN ln -sf /dev/stdout /var/log/nginx/access.log \
     && ln -sf /dev/stderr /var/log/nginx/error.log
     
     COPY nginx.conf /etc/nginx/nginx.conf
     
     EXPOSE 80
     
     STOPSIGNAL SIGTERM
     
     CMD ["nginx", "-g", "daemon off;"]
     ```
   + An `nginx.conf` file, modified from [ https://github\.com/awslabs/ecs\-nginx\-reverse\-proxy/tree/master/reverse\-proxy/nginx](https://github.com/awslabs/ecs-nginx-reverse-proxy/tree/master/reverse-proxy/nginx)\.

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
     
       upstream backend {
         zone name 10m;
         server app:3000    weight=2;
         server app2:3000    weight=1;
       }
     
       server{
         listen 8080;
         location /api {
           api write=on;
         }
       }
     
       match server_ok {
         status 100-599;
       }
     
       server {
         listen 80;
         status_zone zone;
         # Nginx will reject anything not matching /api
         location /api {
           # Reject requests with unsupported HTTP method
           if ($request_method !~ ^(GET|POST|HEAD|OPTIONS|PUT|DELETE)$) {
             return 405;
           }
     
           # Only requests matching the whitelist expectations will
           # get sent to the application server
           proxy_pass http://backend;
           health_check uri=/lorem-ipsum match=server_ok;
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

1. Build an image from files in your new directory:

   ```
   docker build -t nginx-plus-reverse-proxy ./path-to-your-directory
   ```

1. Upload your new images to an image repository for later use\.

### Create the task definition to run NGINX Plus and the web server app in Amazon ECS<a name="ContainerInsights-Prometheus-nginx-plus-ecs-setup-task"></a>

Next, you set up the task definition\.

This task definition enables the collection and export of NGINX Plus Prometheus metrics\. The NGINX container tracks input from the app, and exposes that data to port 8080, as set in `nginx.conf`\. The NGINX prometheus exporter container scrapes these metrics, and posts them to port 9113, for use in CloudWatch\.

**To set up the task definition for the NGINX sample Amazon ECS workload**

1. Create a task definition JSON file with the following content\. Replace *your\-customized\-nginx\-plus\-image* with the image URI for your customized NGINX Plus image, and replace *your\-web\-server\-app\-image* with the image URI for your web server app image\.

   ```
   {
     "containerDefinitions": [
       {
         "name": "nginx",
         "image": "your-customized-nginx-plus-image",
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
           "app",
           "app2"
         ]
       },
       {
         "name": "app",
         "image": "your-web-server-app-image",
         "memory": 256,
         "cpu": 128,
         "essential": true
       },
       {
         "name": "app2",
         "image": "your-web-server-app-image",
         "memory": 256,
         "cpu": 128,
         "essential": true
       },
       {
         "name": "nginx-prometheus-exporter",
         "image": "docker.io/nginx/nginx-prometheus-exporter:0.8.0",
         "memory": 256,
         "cpu": 256,
         "essential": true,
         "command": [
           "-nginx.plus",
           "-nginx.scrape-uri",
            "http://nginx:8080/api"
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
     "family": "nginx-plus-sample-stack"
   }
   ```

1. Register the task definition:

   ```
   aws ecs register-task-definition --cli-input-json file://path-to-your-task-definition-json
   ```

1. Create a service to run the task by entering the following command:

   ```
   aws ecs create-service \
    --cluster your-cluster-name \
    --service-name nginx-plus-service \
    --task-definition nginx-plus-sample-stack:1 \
    --desired-count 1
   ```

   Be sure not to change the service name\. We will be running a CloudWatch agent service using a configuration that searches for tasks using the name patterns of the services that started them\. For example, for the CloudWatch agent to find the task launched by this command, you can specify the value of `sd_service_name_pattern` to be `^nginx-plus-service$`\. The next section provides more details\.

### Configure the CloudWatch agent to scrape NGINX Plus Prometheus metrics<a name="ContainerInsights-Prometheus-nginx-plus-ecs-setup-agent"></a>

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
       "sd_job_name": "nginx-plus-prometheus-exporter",
       "sd_metrics_path": "/metrics",
       "sd_metrics_ports": "9113",
       "sd_service_name_pattern": "^nginx-plus.*"
      }
   ],
   ```

1. In the same file, add the following section in the `metric_declaration` section to allow NGINX Plus metrics\. Be sure to follow the existing indentation pattern\.

   ```
   {
     "source_labels": ["job"],
     "label_matcher": "^nginx-plus.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily", "ServiceName"]],
     "metric_selectors": [
       "^nginxplus_connections_accepted$",
       "^nginxplus_connections_active$",
       "^nginxplus_connections_dropped$",
       "^nginxplus_connections_idle$",
       "^nginxplus_http_requests_total$",
       "^nginxplus_ssl_handshakes$",
       "^nginxplus_ssl_handshakes_failed$",
       "^nginxplus_up$",
       "^nginxplus_upstream_server_health_checks_fails$"
     ]
   },
   {
     "source_labels": ["job"],
     "label_matcher": "^nginx-plus.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily", "ServiceName", "upstream"]],
     "metric_selectors": [
       "^nginxplus_upstream_server_response_time$"
     ]
   },
   {
     "source_labels": ["job"],
     "label_matcher": "^nginx-plus.*",
     "dimensions": [["ClusterName", "TaskDefinitionFamily", "ServiceName", "code"]],
     "metric_selectors": [
       "^nginxplus_upstream_server_responses$",
       "^nginxplus_server_zone_responses$"
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
       --change-set-name nginx-plus-scraping-support
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Revew the newly\-created changeset **nginx\-plus\-scraping\-support**\. You should see one change applied to the **CWAgentConfigSSMParameter** resource\. Run the changeset and restrt the CloudWatch agent task by entering the following command:

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

## Viewing your NGINX Plus metrics and logs<a name="ContainerInsights-Prometheus-Setup-nginx-plus-view"></a>

You can now view the NGINX Plus metrics being collected\.

**To view the metrics for your sample NGINX workload**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the Region where your cluster is running, choose **Metrics** in the left navigation pane\. Find the **ContainerInsights/Prometheus** namespace to see the metrics\.

1. To see the CloudWatch Logs events, choose **Log groups** in the navigation pane\. The events are in the log group **/aws/containerinsights/*your\_cluster\_name*/prometheus**, in the log stream *nginx\-plus\-prometheus\-exporter*\.
# Tutorial for adding a new Prometheus scrape target: Redis on Amazon EKS and Kubernetes clusters<a name="ContainerInsights-Prometheus-Setup-redis-eks"></a>

This tutorial provides a hands\-on introduction to scrape the Prometheus metrics of a sample Redis application on Amazon EKS and Kubernetes\. Redis \(https://redis\.io/\) is an open source \(BSD licensed\), in\-memory data structure store, used as a database, cache and message broker\. For more information, see [ redis](https://redis.io/)\.

redis\_exporter \(MIT License licensed\) is used to expose the Redis prometheus metrics on the specified port \(default: 0\.0\.0\.0:9121\)\. For more information, see [ redis\_exporter](https://github.com/oliver006/redis_exporter)\.

The Docker images in the following two Docker Hub repositories are used in this tutorial: 
+ [ redis](https://hub.docker.com/_/redis?tab=description)
+ [ redis\_exporter](https://hub.docker.com/r/oliver006/redis_exporter)

**To install a sample Redis workload which exposes Prometheus metrics**

1. Set the namespace for the sample Redis workload\.

   ```
   REDIS_NAMESPACE=redis-sample
   ```

1. If you are running Redis on a cluster with the Fargate launch type, you need to set up a Fargate profile\. To set up the profile, enter the following command\. Replace *MyCluster* with the name of your cluster\.

   ```
   eksctl create fargateprofile --cluster MyCluster \
   --namespace $REDIS_NAMESPACE --name $REDIS_NAMESPACE
   ```

1. Enter the following command to install the sample Redis workload\.

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/redis/redis-traffic-sample.yaml \
   | sed "s/{{namespace}}/$REDIS_NAMESPACE/g" \
   | kubectl apply -f -
   ```

1. The installation includes a service named `my-redis-metrics` which exposes the Redis Prometheus metric on port 9121 Enter the following command to get the details of the service: 

   ```
   kubectl describe service/my-redis-metrics  -n $REDIS_NAMESPACE
   ```

   In the `Annotations` section of the results, you'll see two annotations which match the Prometheus scrape configuration of the CloudWatch agent, so that it can auto\-discover the workloads:

   ```
   prometheus.io/port: 9121
   prometheus.io/scrape: true
   ```

   The related Prometheus scrape configuration can be found in the `- job_name: kubernetes-service-endpoints` section of `kubernetes-eks.yaml` or `kubernetes-k8s.yaml`\.

**To start collecting Redis Prometheus metrics in CloudWatch**

1. Download the latest version of the of `kubernetes-eks.yaml` or `kubernetes-k8s.yaml` file by entering one of the following commands\. For an Amazon EKS cluster with the EC2 launch type, enter this command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
   ```

   For an Amazon EKS cluster with the Fargate launch type, enter this command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks-fargate.yaml
   ```

   For a Kubernetes cluster running on an Amazon EC2 instance, enter this command\.

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-k8s.yaml
   ```

1. Open the file with a text editor, and find the `cwagentconfig.json` section\. Add the following subsection and save the changes\. Be sure that the indentation follows the existing pattern\.

   ```
   {
     "source_labels": ["pod_name"],
     "label_matcher": "^redis-instance$",
     "dimensions": [["Namespace","ClusterName"]],
     "metric_selectors": [
       "^redis_net_(in|out)put_bytes_total$",
       "^redis_(expired|evicted)_keys_total$",
       "^redis_keyspace_(hits|misses)_total$",
       "^redis_memory_used_bytes$",
       "^redis_connected_clients$"
     ]
   },
   {
     "source_labels": ["pod_name"],
     "label_matcher": "^redis-instance$",
     "dimensions": [["Namespace","ClusterName","cmd"]],
     "metric_selectors": [
       "^redis_commands_total$"
     ]
   },
   {
     "source_labels": ["pod_name"],
     "label_matcher": "^redis-instance$",
     "dimensions": [["Namespace","ClusterName","db"]],
     "metric_selectors": [
       "^redis_db_keys$"
     ]
   },
   ```

   The section you added puts the Redis metrics onto the CloudWatch agent allow list\. For a list of these metrics, see the following section\.

1. If you already have the CloudWatch agent with Prometheus support deployed in this cluster, you must delete it by entering the following command\.

   ```
   kubectl delete deployment cwagent-prometheus -n amazon-cloudwatch
   ```

1. Deploy the CloudWatch agent with your updated configuration by entering one of the following commands\. Replace *MyCluster* and *region* to match your settings\.

   For an Amazon EKS cluster with the EC2 launch type, enter this command\.

   ```
   kubectl apply -f prometheus-eks.yaml
   ```

   For an Amazon EKS cluster with the Fargate launch type, enter this command\.

   ```
   cat prometheus-eks-fargate.yaml \
   | sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" \
   | kubectl apply -f -
   ```

   For a Kubernetes cluster, enter this command\.

   ```
   cat prometheus-k8s.yaml \
   | sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" \
   | kubectl apply -f -
   ```

## Viewing your Redis Prometheus metrics<a name="ContainerInsights-Prometheus-Setup-redis-eks-view"></a>

This tutorial sends the following metrics to the **ContainerInsights/Prometheus** namespace in CloudWatch\. You can use the CloudWatch console to see the metrics in that namespace\.


| Metric name | Dimensions | 
| --- | --- | 
|  `redis_net_input_bytes_total` |  ClusterName, Namespace  | 
|  `redis_net_output_bytes_total` |  ClusterName, Namespace  | 
|  `redis_expired_keys_total` |  ClusterName, Namespace  | 
|  `redis_evicted_keys_total` |  ClusterName, Namespace  | 
|  `redis_keyspace_hits_total` |  ClusterName, Namespace  | 
|  `redis_keyspace_misses_total` |  ClusterName, Namespace  | 
|  `redis_memory_used_bytes` |  ClusterName, Namespace  | 
|  `redis_connected_clients` |  ClusterName, Namespace  | 
|  `redis_commands_total` |  ClusterName, Namespace, cmd  | 
|  `redis_db_keys` |  ClusterName, Namespace, db  | 

**Note**  
The value of the **cmd** dimension can be: `append`, `client`, `command`, `config`, `dbsize`, `flushall`, `get`, `incr`, `info`, `latency`, or `slowlog`\.  
The value of the **db** dimension can be `db0` to `db15`\. 

You can also create a CloudWatch dashboard for your Redis Prometheus metrics\.

**To create a dashboard for Redis Prometheus metrics**

1. Create environment variables, replacing the values below to match your deployment\.

   ```
   DASHBOARD_NAME=your_cw_dashboard_name
   REGION_NAME=your_metric_region_such_as_us-east-1
   CLUSTER_NAME=your_k8s_cluster_name_here
   NAMESPACE=your_redis_service_namespace_here
   ```

1. Enter the following command to create the dashboard\.

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/redis/cw_dashboard_redis.json \
   | sed "s/{{YOUR_AWS_REGION}}/${REGION_NAME}/g" \
   | sed "s/{{YOUR_CLUSTER_NAME}}/${CLUSTER_NAME}/g" \
   | sed "s/{{YOUR_NAMESPACE}}/${NAMESPACE}/g" \
   ```
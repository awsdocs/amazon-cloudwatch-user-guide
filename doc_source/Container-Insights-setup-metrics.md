# Set Up the CloudWatch Agent to Collect Cluster Metrics<a name="Container-Insights-setup-metrics"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

In the following steps, you set up the CloudWatch agent to be able to collect metrics from your clusters\.

## Step 1: Create a Namespace for CloudWatch<a name="create-namespace-metrics"></a>

Use the following steps to create a Kubernetes namespace called `amazon-cloudwatch` for CloudWatch\. You can skip these steps if you have already created this namespace\.

**To create a namespace for CloudWatch**

1. Download the namespace YAML to your `kubectl` client host by running the following command\.

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/kubernetes-monitoring/cloudwatch-namespace.yaml
   ```

1. Run the following command to create the `amazon-cloudwatch` namespace\.

   ```
   kubectl apply -f cloudwatch-namespace.yaml
   ```

## Step 2: Create a Service Account in the Cluster<a name="create-service-account"></a>

Use the following steps to create a service account for the CloudWatch agent, if you do not already have one\.

**To create a service account for the CloudWatch agent**

1. Download the service account YAML to your `kubectl` client host by running the following command\.

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/kubernetes-monitoring/cwagent-serviceaccount.yaml
   ```

1. Create the service account in Amazon EKS by running the following command\.

   ```
   kubectl apply -f cwagent-serviceaccount.yaml
   ```

If you didn't follow the previous steps, but you already have a service account for the CloudWatch agent that you want to use, you must ensure that it has the following rules\. Additionally, in the rest of the steps in the Container Insights installation, you must use the name of that service account instead of `cloudwatch-agent`\.

```
rules:
  - apiGroups: [""]
    resources: ["pods", "nodes", "endpoints"]
    verbs: ["watch", "list"]
  - apiGroups: [""]
    resources: ["nodes/proxy"]
    verbs: ["get"]
  - apiGroups: [""]
    resources: ["nodes/stats", "configmaps", “events”]
    verbs: ["create"]
  - apiGroups: [""]
    resources: ["configmaps"]
    resourceNames: ["cwagent-clusterleader"]
    verbs: ["get", "update"]
  - nonResourceURLs: ["/metrics"]
    verbs: ["get"]
```

## Step 3: Create a ConfigMap for the CloudWatch Agent<a name="create-configmap"></a>

Use the following steps to create a ConfigMap for the CloudWatch agent\.

**To create a ConfigMap for the CloudWatch agent**

1. Download the ConfigMap YAML to your `kubectl` client host by running the following command:

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/kubernetes-monitoring/cwagent-configmap.yaml
   ```

1. Edit the downloaded YAML file, as follows:
   + **cluster\_name** – In the `kubernetes` section, replace `{{cluster-name}}` with the name of your cluster\. Remove the `{{}}` characters\. Alternatively, if you're using an Amazon EKS cluster, you can delete the `"cluster_name"` field and value\. If you do, the CloudWatch agent detects the cluster name from the Amazon EC2 tags\.

1. \(Optional\) Make further changes to the ConfigMap based on your monitoring requirements, as follows:
   + **metrics\_collection\_interval** – In the `kubernetes` section, you can specify how often the agent collects metrics\. The default is 60 seconds\. The default cadvisor collection interval in kubelet is 15 seconds, so don't set this value to less than 15 seconds\.
   + **endpoint\_override** – In the `structuredlogs` section, you can specify the CloudWatch Logs endpoint if you want to override the default endpoint\. You might want to do this if you're publishing from a cluster in a VPC and you want the data to go to a VPC endpoint\.
   + **force\_flush\_interval** – In the `structuredlogs` section, you can specify the interval for batching log events before they are published to CloudWatch Logs\. The default is 5 seconds\.
   + **region** – By default, the agent published metrics to the Region where the worker node is located\. To override this, you can add a `region` field in the `agent` section: for example, `"region":"us-west-2"`\.
   + **statsd** section – If you want the CloudWatch Logs agent to also run as a StatsD listener in each worker node of your cluster, you can add a `statsd` section to the `metrics` section, as in the following example\. For information on other StatsD options for this section, see [Retrieve Custom Metrics with StatsD ](CloudWatch-Agent-custom-metrics-statsd.md)\.

     ```
     "metrics": {
       "metrics_collected": {
         "statsd": {
           "service_address":":8125"
         }
       }
     }
     ```

     A full example of the JSON section is as follows\.

     ```
     {
         "agent": {
             "region": "us-east-1"
         },
         "structuredlogs": {
             "metrics_collected": {
                 "kubernetes": {
                     "cluster_name": "MyCluster",
                     "metrics_collection_interval": 60
                 }
             },
             "force_flush_interval": 5,
             "endpoint_override": "logs.us-east-1.amazonaws.com"
         },
         "metrics": {
             "metrics_collected": {
                 "statsd": {
                     "service_address": ":8125"
                 }
             }
         }
     }
     ```

1. Create the ConfigMap in the cluster by running the following command\.

   ```
   kubectl apply -f cwagent-configmap.yaml
   ```

## Step 4: Deploy the CloudWatch Agent as a DaemonSet<a name="deploy-agent-yaml"></a>

To finish the installation of the CloudWatch agent and begin collecting container metrics, use the following steps\.

**To deploy the CloudWatch agent as a DaemonSet**

1. Download the DaemonSet YAML to your `kubectl` client host by running the following command\.

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/kubernetes-monitoring/cwagent-daemonset.yaml
   ```

1. **statsd** section – \(Optional\) If you want the CloudWatch Logs agent to also run as a StatsD listener in each worker node of your cluster, uncomment the `port` section in the `cwagent-daemonset.yaml` file as in the following: 

   ```
   ports:
     - containerPort: 8125
       hostPort: 8125
       protocol: UDP
   ```

1. Deploy the CloudWatch agent in your cluster by running the following command\.

   ```
   kubectl apply -f cwagent-daemonset.yaml
   ```

1. Validate that the agent is deployed by running the following command\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

When complete, the CloudWatch agent creates a log group named `/aws/containerinsights/Cluster_Name/performance` and sends the performance log events to this log group\. If you also set up the agent as a StatsD listener, the agent also listens for StatsD metrics on port 8125 with the IP address of the node where the application pod is scheduled\.

### Troubleshooting<a name="ContainerInsights-deploy-troubleshooting"></a>

If the agent doesn't deploy correctly, try the following:
+ Run the following command to get the list of pods\.

  ```
  kubectl get pods -n amazon-cloudwatch
  ```
+ Run the following command and check the events at the bottom of the output\.

  ```
  kubectl describe pod pod-name -n amazon-cloudwatch
  ```
+ Run the following command to check the logs\.

  ```
  kubectl logs pod-name  -n amazon-cloudwatch
  ```
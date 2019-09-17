# \(Optional\) Set Up the CloudWatch Agent as a StatsD Endpoint to Send StatsD Metrics to CloudWatch<a name="Container-Insights-setup-StatsD"></a>

This section explains how to set up and configure the CloudWatch agent as a StatsD endpoint on an Amazon EKS cluster or Kubernetes cluster to collect StatsD metrics from the containers and publish them to CloudWatch\. You might want to do this if you're already using StatsD\.

You have three options for how to deploy the CloudWatch agent as a StatsD endpoint in a cluster:

**Deploy a single CloudWatch agent inside the cluster as a service**  
We recommend this option if you want to aggregate all StatsD metrics across the cluster before publishing them to CloudWatch\. This option also scales well, and consumes fewer resources than deploying the agent as a sidecar\.

**Deploy the CloudWatch agent as a DaemonSet to each worker node**  
We recommend this option if you want aggregate StatsD metrics across Kubernetes worker nodes, or if you have concern about the scalability of a single CloudWatch agent serving all StatsD metrics across the whole cluster\. It also consumes fewer resources than deploying the agent as a sidecar\.  
The downside is that you are not able to scale on a single node because only one container will be running on each node\.

**Deploy the CloudWatch agent as a sidecar**  
We recommend this option if you don’t want to aggregate StatsD metrics across pods\. Resources such as CPU, memory, and network are isolated at the pod level so that it’s flexible for customers to plug the agent into their pods\.  
The disadvantages of this method include resource consumption– for example, 1000 pods has 1000 CloudWatch agent containers\. Additionally, metrics coming from different tasks can't be aggregated\.

## Step 1: Create a Namespace for CloudWatch<a name="create-namespace-statsd"></a>

Use the following steps to create a Kubernetes namespace called `amazon-cloudwatch` for CloudWatch\. You can skip these steps if you have already created this namespace\.

**To create a namespace for CloudWatch**
+ Download and apply the namespace YAML to your `kubectl` client host by running the following command\.

  ```
  kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/cloudwatch-namespace.yaml
  ```

## Step 2: Create a ConfigMap for StatsD<a name="Container-Insights-setup-StatsD-ConfigMap"></a>

No matter which option you choose, you must create a ConfigMap for StatsD\.

**To create a ConfigMap for StatsD to send metrics from a cluster**

1. Download the ConfigMap YAML to your `kubectl` client host by running the following command:

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/cwagent-statsd/cwagent-statsd-configmap.yaml
   ```

   This YAML file has a JSON configuration blob embedded\. 

1. Edit the JSON configuration blob\. Be sure that the JSON blob remains in valid JSON format when you're done\.

   1. You can edit the StatsD items in the `statsd` section\. For more information, see [Retrieve Custom Metrics with StatsD ](CloudWatch-Agent-custom-metrics-statsd.md)\.

   1. You can edit general items in the `agent` section\. For more information, see [ CloudWatch Agent Configuration File: Agent Section](CloudWatch-Agent-Configuration-File-Details.md#CloudWatch-Agent-Configuration-File-Agentsection)\.

      In the `agent` section, we recommend that you set `omit_hostname` to `true`\. Otherwise, the agent adds the hostname of the worker node where the agent pod is running as a dimension of the StatsD metrics\. Using this hostname as a dimension can make the viewing of metrics more difficult if the agent pod is rescheduled to a different worker node\.

      By default, CloudWatch publishes the metrics to the AWS Region where the worker node is located\. To override this, you can add a `region` field to the `agent` section\. For example, `"region":"us-west-2"`\.

1. Run the following command to create the ConfigMap in the cluster\.

   ```
   kubectl apply -f cwagent-statsd-configmap.yaml
   ```

## Step 3: Deploy CloudWatch Agent<a name="Container-Insights-setup-StatsD-Deploy"></a>

To deploy the CloudWatch agent to send StatsD metrics, use the steps in one of the following sections\.

### Option 1: Deploy a Single CloudWatch Agent in the Cluster<a name="Container-Insights-setup-StatsD-single"></a>

This section explains how to deploy a single CloudWatch agent in a cluster to send StatsD metrics to CloudWatch\. When the deployment is complete and the agent is running, it starts listening on the URL `cloudwatch-agent-statsd.amazon-cloudwatch.svc:8125`\. Your application inside the cluster can emit StatsD metrics to this URL\.

**To deploy a single CloudWatch agent in a cluster**

1. Download and deploy the deployment YAML by entering the following command\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/cwagent-statsd/cwagent-statsd-deployment.yaml
   ```

1. Validate the deployment by entering the following command\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

### Option 2: Deploy the CloudWatch Agent as a DaemonSet to Each Worker Node<a name="Container-Insights-setup-StatsD-each-worker"></a>

This option is for deploying the CloudWatch agent as a DaemonSet to each worker node\. When the deployment is complete and the agent is running, it starts listening on `NodeIP:8125`\. Your application inside the cluster can emit StatsD metrics to port 8125 with the IP of the node where your application pod is scheduled\.

**To deploy the CloudWatch agent on each worker node**

1. Download and deploy the DaemonSet YAML by running the following command\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/cwagent-statsd/cwagent-statsd-daemonset.yaml
   ```

1. Validate the deployment by running the following command\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

### Option 3: Deploy the CloudWatch Agent as a Sidecar<a name="Container-Insights-setup-StatsD-sidecar"></a>

This section explains how to deploy the CloudWatch agent as a sidecar\. The agent collects the StatsD metrics from the applications running in the same pod\. The following is an example YAML file that starts the CloudWatch agent as a sidecar to an Amazon Linux container\.

```
apiVersion: v1
kind: Pod
metadata:
  namespace: default
  name: amazonlinux
spec:
  containers:
    - name: amazonlinux
      image: amazonlinux
      command: ["/bin/sh"]
      args: ["-c", "sleep 300"]
    - name: cloudwatch-agent
      image: amazon/cloudwatch-agent
      imagePullPolicy: Always
      resources:
        limits:
          cpu:  200m
          memory: 100Mi
        requests:
          cpu: 200m
          memory: 100Mi
      volumeMounts:
        - name: cwagentconfig
          mountPath: /etc/cwagentconfig
  volumes:
    - name: cwagentconfig
      configMap:
        name: cwagentstatsdconfig
  terminationGracePeriodSeconds: 60
```
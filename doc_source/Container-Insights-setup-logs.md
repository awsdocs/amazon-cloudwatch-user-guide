# \(Optional\) Set up FluentD as a DaemonSet to send logs to CloudWatch Logs<a name="Container-Insights-setup-logs"></a>

**Warning**  
Container Insights Support for FluentD is now in maintenance mode, which means that AWS will not provide any further updates for FluentD and that we are planning to deprecate it in near future\. Additionally, the current FluentD configuration for Container Insights is using an old version of the FluentD Image `fluent/fluentd-kubernetes-daemonset:v1.7.3-debian-cloudwatch-1.0` which does not have the latest improvement and security patches\. For the latest FluentD image supported by the open source community, see [fluentd\-kubernetes\-daemonset](https://github.com/fluent/fluentd-kubernetes-daemonset)\.   
We strongly recommend that you migrate to use FluentBit with Container Insights whenever possible\. Using FluentBit as the log forwarder for Container Insights provides significant performance gains\.   
For more information, see [Set up Fluent Bit as a DaemonSet to send logs to CloudWatch Logs](Container-Insights-setup-logs-FluentBit.md) and [ Differences if you're already using Fluentd](Container-Insights-setup-logs-FluentBit.md#Container-Insights-setup-logs-FluentBit-migrate)\.

To set up FluentD to collect logs from your containers, you can follow the steps in [Quick Start setup for Container Insights on Amazon EKS and Kubernetes](Container-Insights-setup-EKS-quickstart.md) or you can follow the steps in this section\. In the following steps, you set up FluentD as a DaemonSet to send logs to CloudWatch Logs\. When you complete this step, FluentD creates the following log groups if they don't already exist\.


| Log group name | Log source | 
| --- | --- | 
|  `/aws/containerinsights/Cluster_Name/application`  |  All log files in `/var/log/containers`  | 
|  `/aws/containerinsights/Cluster_Name/host`  |  Logs from `/var/log/dmesg`, `/var/log/secure`, and `/var/log/messages`  | 
|  `/aws/containerinsights/Cluster_Name/dataplane`  |  The logs in `/var/log/journal` for `kubelet.service`, `kubeproxy.service`, and `docker.service`\.  | 

## Step 1: Create a namespace for CloudWatch<a name="create-namespace-logs"></a>

Use the following step to create a Kubernetes namespace called `amazon-cloudwatch` for CloudWatch\. You can skip this step if you have already created this namespace\.

**To create a namespace for CloudWatch**
+ Enter the following command\.

  ```
  kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/cloudwatch-namespace.yaml
  ```

## Step 2: Install FluentD<a name="ContainerInsights-install-FluentD"></a>

Start this process by downloading FluentD\. When you finish these steps, the deployment creates the following resources on the cluster:
+ A service account named `fluentd` in the `amazon-cloudwatch` namespace\. This service account is used to run the FluentD DaemonSet\. For more information, see [Managing Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) in the Kubernetes Reference\.
+ A cluster role named `fluentd` in the `amazon-cloudwatch` namespace\. This cluster role grants `get`, `list`, and `watch` permissions on pod logs to the `fluentd` service account\. For more information, see [API Overview](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview/) in the Kubernetes Reference\.
+ A ConfigMap named `fluentd-config` in the `amazon-cloudwatch` namespace\. This ConfigMap contains the configuration to be used by FluentD\. For more information, see [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) in the Kubernetes Tasks documentation\.

**To install FluentD**

1. Create a ConfigMap named `cluster-info` with the cluster name and the AWS Region that the logs will be sent to\. Run the following command, updating the placeholders with your cluster and Region names\.

   ```
   kubectl create configmap cluster-info \
   --from-literal=cluster.name=cluster_name \
   --from-literal=logs.region=region_name -n amazon-cloudwatch
   ```

1. Download and deploy the FluentD DaemonSet to the cluster by running the following command\. Make sure that you are using the container image with correct architecture\. The example manifest only works on x86 instances and will enter `CrashLoopBackOff` if you have Advanced RISC Machine \(ARM\) instances in your cluster\. The FluentD daemonSet does not have an official multi\-architecture Docker image that enables you to use one tag for multiple underlying images and let the container runtime pull the right one\. The FluentD ARM image uses a different tag with an `arm64` suffix\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/fluentd/fluentd.yaml
   ```
**Note**  
Because of a recent change to optimize the FluentD configuration and minimize the impact of FluentD API requests on Kubernetes API endpoints, the "Watch" option for Kubernetes filters has been disabled by default\. For more details, see [fluent\-plugin\-kubernetes\_metadata\_filter](https://github.com/fabric8io/fluent-plugin-kubernetes_metadata_filter)\. 

1. Validate the deployment by running the following command\. Each node should have one pod named `fluentd-cloudwatch-*`\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

## Step 3: Verify the FluentD setup<a name="ContainerInsights-verify-FluentD"></a>

To verify your FluentD setup, use the following steps\.

**To verify the FluentD setup for Container Insights**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\. Make sure that you're in the Region where you deployed FluentD to your containers\.

   In the list of log groups in the Region, you should see the following:
   + `/aws/containerinsights/Cluster_Name/application`
   + `/aws/containerinsights/Cluster_Name/host`
   + `/aws/containerinsights/Cluster_Name/dataplane`

   If you see these log groups, the FluentD setup is verified\.

## Multiline log support<a name="ContainerInsights-fluentd-multiline"></a>

On August 19 2019, we added multiline log support for the logs collected by FluentD\.

By default, the multiline log entry starter is any character with no white space\. This means that all log lines that start with a character that does not have white space are considered as a new multiline log entry\.

If your own application logs use a different multiline starter, you can support them by making two changes in the `fluentd.yaml` file\.

First, exclude them from the default multiline support by adding the pathnames of your log files to an `exclude_path` field in the `containers` section of `fluentd.yaml`\. The following is an example\.

```
<source>
  @type tail
  @id in_tail_container_logs
  @label @containers
  path /var/log/containers/*.log
  exclude_path ["full_pathname_of_log_file*", "full_pathname_of_log_file2*"]
```

Next, add a block for your log files to the `fluentd.yaml` file\. The example below is used for the CloudWatch agent's log file, which uses a timestamp regular expression as the multiline starter\. You can copy this block and add it to `fluentd.yaml`\. Change the indicated lines to reflect your application log file name and the multiline starter that you want to use\.

```
<source>
  @type tail
  @id in_tail_cwagent_logs
  @label @cwagentlogs
  path /var/log/containers/cloudwatch-agent*
  pos_file /var/log/cloudwatch-agent.log.pos
  tag *
  read_from_head true
 <parse>
    @type json
    time_format %Y-%m-%dT%H:%M:%S.%NZ
 </parse>
</source>
```

```
<label @cwagentlogs>
  <filter **>
    @type kubernetes_metadata
    @id filter_kube_metadata_cwagent
  </filter>

  <filter **>
    @type record_transformer
    @id filter_cwagent_stream_transformer
    <record>
      stream_name ${tag_parts[3]}
    </record>
 </filter>

  <filter **>
    @type concat
    key log
    multiline_start_regexp /^\d{4}[-/]\d{1,2}[-/]\d{1,2}/
    separator ""
    flush_interval 5
    timeout_label @NORMAL
 </filter>

 <match **>
    @type relabel
    @label @NORMAL
 </match>
</label>
```

## \(Optional\) Reducing the log volume from FluentD<a name="ContainerInsights-fluentd-volume"></a>

By default, we send FluentD application logs and Kubernetes metadata to CloudWatch\. If you want to reduce the volume of data being sent to CloudWatch, you can stop one or both of these data sources from being sent to CloudWatch\.

To stop FluentD application logs, remove the following section from the `fluentd.yaml` file\.

```
<source>
  @type tail
  @id in_tail_fluentd_logs
  @label @fluentdlogs
  path /var/log/containers/fluentd*
  pos_file /var/log/fluentd.log.pos
  tag *
  read_from_head true
   <parse>
   @type json
   time_format %Y-%m-%dT%H:%M:%S.%NZ
  </parse>
</source>  

<label @fluentdlogs>
  <filter **>
    @type kubernetes_metadata
    @id filter_kube_metadata_fluentd
  </filter>

  <filter **>
    @type record_transformer
    @id filter_fluentd_stream_transformer
    <record>
      stream_name ${tag_parts[3]}
    </record>
   </filter>

   <match **>
     @type relabel
     @label @NORMAL
   </match>
</label>
```

To remove Kubernetes metadata from being appended to log events that are sent to CloudWatch, add one line to the `record_transformer` section in the `fluentd.yaml` file\. In the log source where you want to remove this metadata, add the following line\.

```
remove_keys $.kubernetes.pod_id, $.kubernetes.master_url, $.kubernetes.container_image_id, $.kubernetes.namespace_id
```

For example:

```
<filter **>
  @type record_transformer
  @id filter_containers_stream_transformer
  <record>
    stream_name ${tag_parts[3]}
  </record>
  remove_keys $.kubernetes.pod_id, $.kubernetes.master_url, $.kubernetes.container_image_id, $.kubernetes.namespace_id
</filter>
```

## Troubleshooting<a name="ContainerInsights-fluentd-troubleshooting"></a>

If you don't see these log groups and are looking in the correct Region, check the logs for the FluentD DaemonSet pods to look for the error\.

Run the following command and make sure that the status is `Running`\.

```
kubectl get pods -n amazon-cloudwatch
```

In the results of the previous command, note the pod name that starts with `fluentd-cloudwatch`\. Use this pod name in the following command\.

```
kubectl logs pod_name -n amazon-cloudwatch
```

If the logs have errors related to IAM permissions, check the IAM role attached to the cluster nodes\. For more information about the permissions required to run an Amazon EKS cluster, see [Amazon EKS IAM Policies, Roles, and Permissions](https://docs.aws.amazon.com/eks/latest/userguide/IAM_policies.html) in the *Amazon EKS User Guide*\.

If the pod status is `CreateContainerConfigError`, get the exact error by running the following command\.

```
kubectl describe pod pod_name -n amazon-cloudwatch
```

If the pod status is `CrashLoopBackOff`, make sure that the architecture of the Fluentd container image is the same as the node when you installed Fluentd\. If your cluster has both x86 and ARM64 nodes, you can use a kubernetes\.io/arch label to place the images on the correct node\. For more information, see [kuberntes\.io/arch](https://kubernetes.io/docs/reference/kubernetes-api/labels-annotations-taints/#kubernetes-io-arch)\. 
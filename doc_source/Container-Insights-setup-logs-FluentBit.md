# Set up Fluent Bit as a DaemonSet to send logs to CloudWatch Logs<a name="Container-Insights-setup-logs-FluentBit"></a>

The following sections help you deploy Fluent Bit to send logs from containers to CloudWatch Logs\.

**Topics**
+ [Differences if you're already using Fluentd](#Container-Insights-setup-logs-FluentBit-migrate)
+ [Setting up Fluent Bit](#Container-Insights-FluentBit-setup)
+ [Multiline log support](#ContainerInsights-fluentbit-multiline)
+ [\(Optional\) Reducing the log volume from Fluent Bit](#ContainerInsights-fluentbit-volume)
+ [Troubleshooting](#Container-Insights-FluentBit-troubleshoot)
+ [Dashboard](#Container-Insights-FluentBit-dashboard)

## Differences if you're already using Fluentd<a name="Container-Insights-setup-logs-FluentBit-migrate"></a>

If you are already using Fluentd to send logs from containers to CloudWatch Logs, read this section to see the differences between Fluentd and Fluent Bit\. If you are not already using Fluentd with Container Insights, you can skip to [ Setting up Fluent Bit](#Container-Insights-FluentBit-setup)\.

We provide two default configurations for Fluent Bit:
+ **Fluent Bit optimized configuration **— A configuration aligned with Fluent Bit best practices\.
+ **Fluentd\-compatible configuration **— A configuration that is aligned with Fluentd behavior as much as possible\.

The following list explains the differences between Fluentd and each Fluent Bit configuration in detail\.
+ ** Differences in log stream names **— If you use the Fluent Bit optimized configuration, the log stream names will be different\.

  Under `/aws/containerinsights/Cluster_Name/application`
  + Fluent Bit optimized configuration sends logs to `kubernetes-nodeName-application.var.log.containers.kubernetes-podName_kubernetes-namespace_kubernetes-container-name-kubernetes-containerID`
  + Fluentd sends logs to `kubernetes-podName_kubernetes-namespace_kubernetes-containerName_kubernetes-containerID`

  Under `/aws/containerinsights/Cluster_Name/host`
  + Fluent Bit optimized configuration sends logs to `kubernetes-nodeName.host-log-file`
  + Fluentd sends logs to `host-log-file-Kubernetes-NodePrivateIp`

  Under `/aws/containerinsights/Cluster_Name/dataplane`
  + Fluent Bit optimized configuration sends logs to `kubernetes-nodeName.dataplaneServiceLog`
  + Fluentd sends logs to `dataplaneServiceLog-Kubernetes-nodeName`
+ The `kube-proxy` and `aws-node` log files that Container Insights writes are in different locations\. In Fluentd configuration, they are in `/aws/containerinsights/Cluster_Name/application`\. In the Fluent Bit optimized configuration, they are in `/aws/containerinsights/Cluster_Name/dataplane`\.
+ Most metadata such as `pod_name` and `namespace_name` are the same in Fluent Bit and Fluentd, but the following are different\.
  + The Fluent Bit optimized configuration uses `docker_id` and Fluentd use `Docker.container_id`\.
  + Both Fluent Bit configurations do not use the following metadata\. They are present only in Fluentd: `container_image_id`, `master_url`, `namespace_id`, and `namespace_labels`\.

## Setting up Fluent Bit<a name="Container-Insights-FluentBit-setup"></a>

To set up Fluent Bit to collect logs from your containers, you can follow the steps in [Quick Start setup for Container Insights on Amazon EKS and Kubernetes](Container-Insights-setup-EKS-quickstart.md) or you can follow the steps in this section\.

With either method, the IAM role that is attached to the cluster nodes must have sufficient permissions\. For more information about the permissions required to run an Amazon EKS cluster, see [Amazon EKS IAM Policies, Roles, and Permissions](https://docs.aws.amazon.com/eks/latest/userguide/IAM_policies.html) in the *Amazon EKS User Guide*\.

In the following steps, you set up Fluent Bit as a daemonSet to send logs to CloudWatch Logs\. When you complete this step, Fluent Bit creates the following log groups if they don't already exist\.

**Important**  
If you already have FluentD configured in Container Inisghts and the FluentD DaemonSet is not running as expected \(this can happen if you use the `containerd` runtime\), you must uninstall it before installing Fluent Bit to prevent Fluent Bit from processing the FluentD error log messages\. Otherwise, you must uninstall FluentD immediately after you have successfully installed Fluent Bit\. Uninstalling Fluentd after installing Fluent Bit ensures ontinuity in logging during this migration process\. Only one of Fluent Bit or FluentD is needed to send logs to CloudWatch Logs\.


| Log group name | Log source | 
| --- | --- | 
|  `/aws/containerinsights/Cluster_Name/application`  |  All log files in `/var/log/containers`  | 
|  `/aws/containerinsights/Cluster_Name/host`  |  Logs from `/var/log/dmesg`, `/var/log/secure`, and `/var/log/messages`  | 
|  `/aws/containerinsights/Cluster_Name/dataplane`  |  The logs in `/var/log/journal` for `kubelet.service`, `kubeproxy.service`, and `docker.service`\.  | 

**To install Fluent Bit to send logs from containers to CloudWatch Logs**

1. If you don't already have a namespace called `amazon-cloudwatch`, create one by entering the following command:

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/cloudwatch-namespace.yaml
   ```

1. Run the following command to create a ConfigMap named `cluster-info` with the cluster name and the Region to send logs to\. Replace *cluster\-name* and *cluster\-region* with your cluster's name and Region\.

   ```
   ClusterName=cluster-name
   RegionName=cluster-region
   FluentBitHttpPort='2020'
   FluentBitReadFromHead='Off'
   [[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
   [[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'
   kubectl create configmap fluent-bit-cluster-info \
   --from-literal=cluster.name=${ClusterName} \
   --from-literal=http.server=${FluentBitHttpServer} \
   --from-literal=http.port=${FluentBitHttpPort} \
   --from-literal=read.head=${FluentBitReadFromHead} \
   --from-literal=read.tail=${FluentBitReadFromTail} \
   --from-literal=logs.region=${RegionName} -n amazon-cloudwatch
   ```

   In this command, the `FluentBitHttpServer` for monitoring plugin metrics is on by default\. To turn it off, change the third line in the command to `FluentBitHttpPort=''` \(empty string\) in the command\.

   Also by default, Fluent Bit reads log files from the tail, and will capture only new logs after it is deployed\. If you want the opposite, set `FluentBitReadFromHead='On'` and it will collect all logs in the file system\.

1. Download and deploy the Fluent Bit daemonset to the cluster by running one of the following commands\.
   + If you want the Fluent Bit optimized configuration, run this command\.

     ```
     kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/fluent-bit/fluent-bit.yaml
     ```
   + If you want the Fluent Bit configuration that is more similar to Fluentd, run this command\.

     ```
     kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/fluent-bit/fluent-bit-compatible.yaml
     ```

1. Validate the deployment by entering the following command\. Each node should have one pod named **fluent\-bit\-\***\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

The above steps create the following resources in the cluster:
+ A service account named `Fluent-Bit` in the `amazon-cloudwatch` namespace\. This service account is used to run the Fluent Bit daemonSet\. For more information, see [Managing Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) in the Kubernetes Reference\.
+ A cluster role named `Fluent-Bit-role` in the `amazon-cloudwatch` namespace\. This cluster role grants `get`, `list`, and `watch` permissions on pod logs to the `Fluent-Bit` service account\. For more information, see [API Overview](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview/) in the Kubernetes Reference\.
+ A ConfigMap named `Fluent-Bit-config` in the `amazon-cloudwatch` namespace\. This ConfigMap contains the configuration to be used by Fluent Bit\. For more information, see [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) in the Kubernetes Tasks documentation\.

If you want to verify your Fluent Bit setup, follow these steps\.

**Verify the Fluent Bit setup**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Log groups**\.

1. Make sure that you're in the Region where you deployed Fluent Bit\.

1. Check the list of log groups in the Region\. You should see the following:
   + `/aws/containerinsights/Cluster_Name/application`
   + `/aws/containerinsights/Cluster_Name/host`
   + `/aws/containerinsights/Cluster_Name/dataplane`

1. Navigate to one of these log groups and check the **Last Event Time** for the log streams\. If it is recent relative to when you deployed Fluent Bit, the setup is verified\.

   There might be a slight delay in creating the `/dataplane` log group\. This is normal as these log groups only get created when Fluent Bit starts sending logs for that log group\.

## Multiline log support<a name="ContainerInsights-fluentbit-multiline"></a>

By default, the multiline log entry starter is any character with no white space\. This means that all log lines that start with a character that does not have white space are considered as a new multiline log entry\.

If your own application logs use a different multiline starter, you can support them by making two changes in the `Fluent-Bit.yaml` file\.

First, exclude them from the default input by adding the pathnames of your log files to an `exclude_path` field in the `containers` section of `Fluent-Bit.yaml`\. The following is an example\.

```
[INPUT]
        Name                tail
        Tag                 application.*
        Exclude_Path        full_pathname_of_log_file*, full_pathname_of_log_file2*
        Path                /var/log/containers/*.log
```

Next, add a block for your log files to the `Fluent-Bit.yaml` file\. Refer to the cloudwatch\-agent log configuration example below which uses a timestamp regular expression as the multiline starter\.

```
application-log.conf: |
    [INPUT]
        Name                tail
        Tag                 application.*
        Path                /var/log/containers/cloudwatch-agent*
        Docker_Mode         On
        Docker_Mode_Flush   5
        Docker_Mode_Parser  cwagent_firstline
        Parser              docker
        DB                  /fluent-bit/state/flb_cwagent.db
        Mem_Buf_Limit       5MB
        Skip_Long_Lines     On
        Refresh_Interval    10
        
parsers.conf: |
    [PARSER]
        Name                cwagent_firstline
        Format              regex
        Regex               (?<log>(?<="log":")\d{4}[\/-]\d{1,2}[\/-]\d{1,2}[ T]\d{2}:\d{2}:\d{2}(?!\.).*?)(?<!\\)".*(?<stream>(?<="stream":").*?)".*(?<time>\d{4}-\d{1,2}-\d{1,2}T\d{2}:\d{2}:\d{2}\.\w*).*(?=})
        Time_Key            time
        Time_Format         %Y-%m-%dT%H:%M:%S.%LZ
```

## \(Optional\) Reducing the log volume from Fluent Bit<a name="ContainerInsights-fluentbit-volume"></a>

By default, we send Fluent Bit application logs and Kubernetes metadata to CloudWatch\. If you want to reduce the volume of data being sent to CloudWatch, you can stop one or both of these data sources from being sent to CloudWatch\.

To stop Fluent Bit application logs, remove the following section from the `Fluent-Bit.yaml` file\.

```
[INPUT]
        Name                tail
        Tag                 application.*
        Path                /var/log/containers/fluent-bit*
        Parser              docker
        DB                  /fluent-bit/state/flb_log.db
        Mem_Buf_Limit       5MB
        Skip_Long_Lines     On
        Refresh_Interval    10
```

To remove Kubernetes metadata from being appended to log events that are sent to CloudWatch, add the following filters to the `application-log.conf` section in the `Fluent-Bit.yaml` file\.

```
application-log.conf: |
    [FILTER]
        Name                nest
        Match               application.*
        Operation           lift
        Nested_under        kubernetes
        Add_prefix          Kube.

    [FILTER]
        Name                modify
        Match               application.*
        Remove              Kube.<Metadata_1>
        Remove              Kube.<Metadata_2>
        Remove              Kube.<Metadata_3>
    
    [FILTER]
        Name                nest
        Match               application.*
        Operation           nest
        Wildcard            Kube.*
        Nested_under        kubernetes
        Remove_prefix       Kube.
```

## Troubleshooting<a name="Container-Insights-FluentBit-troubleshoot"></a>

If you don't see these log groups and are looking in the correct Region, check the logs for the Fluent Bit daemonSet pods to look for the error\.

Run the following command and make sure that the status is `Running`\.

```
kubectl get pods -n amazon-cloudwatch
```

If the logs have errors related to IAM permissions, check the IAM role that is attached to the cluster nodes\. For more information about the permissions required to run an Amazon EKS cluster, see [Amazon EKS IAM Policies, Roles, and Permissions](https://docs.aws.amazon.com/eks/latest/userguide/IAM_policies.html) in the *Amazon EKS User Guide*\.

If the pod status is `CreateContainerConfigError`, get the exact error by running the following command\.

```
kubectl describe pod pod_name -n amazon-cloudwatch
```

## Dashboard<a name="Container-Insights-FluentBit-dashboard"></a>

You can create a dashboard to monitor metrics of each running plugin\. You can see data for input and output bytes and for record processing rates as well as output errors and retry/failed rates\. To view these metrics, you will need to install the CloudWatch agent with Prometheus metrics collection for Amazon EKS and Kubernetes clusters\. For more information about how to set up the dashboard, see [Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clustersInstall the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters](ContainerInsights-Prometheus-Setup.md)\.

**Note**  
Before you can set up this dashboard, you must set up Container Insights for Prometheus metrics\. For more information, see [Container Insights Prometheus metrics monitoring](ContainerInsights-Prometheus.md)\.

**To create a dashboard for the Fluent Bit Prometheus metrics**

1. Create environment variables, replacing the values on the right in the following lines to match your deployment\.

   ```
   DASHBOARD_NAME=your_cw_dashboard_name
   REGION_NAME=your_metric_region_such_as_us-west-1
   CLUSTER_NAME=your_kubernetes_cluster_name
   ```

1. Create the dashboard by running the following command\.

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/fluent-bit/cw_dashboard_fluent_bit.json \
   | sed "s/{{YOUR_AWS_REGION}}/${REGION_NAME}/g" \
   | sed "s/{{YOUR_CLUSTER_NAME}}/${CLUSTER_NAME}/g" \
   | xargs -0 aws cloudwatch put-dashboard --dashboard-name ${DASHBOARD_NAME} --dashboard-body
   ```
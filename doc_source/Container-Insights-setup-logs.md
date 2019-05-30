# Set Up FluentD as a DaemonSet to Send Logs to CloudWatch Logs<a name="Container-Insights-setup-logs"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights isn't released yet\. It's in preview and is subject to change\. The preview is open to all AWS accounts\. You do not need to request access\. | 

In the following steps, you set up FluentD as a DaemonSet to send logs to CloudWatch Logs\. When you complete this step, FluentD creates the following log groups if they don't already exist\.


| Log Group Name | Log Source | 
| --- | --- | 
|  `/aws/containerinsights/Cluster_Name/application`  |  All log files in `/var/log/containers`  | 
|  `/aws/containerinsights/Cluster_Name/host`  |  Logs from `/var/log/dmesg`, `/var/log/secure`, and `/var/log/messages`  | 
|  `/aws/containerinsights/Cluster_Name/dataplane`  |  Logs from `/var/log/journal`  | 

## Step 1: Create a Namespace for CloudWatch<a name="create-namespace-logs"></a>

Use the following steps to create a Kubernetes namespace called `amazon-cloudwatch` for CloudWatch\. You can skip these steps if you have already created this namespace\.

**To create a namespace for CloudWatch**

1. Download the namespace YAML to your `kubectl` client host by running the following command:

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/kubernetes-monitoring/cloudwatch-namespace.yaml
   ```

1. Run the following command to create the `amazon-cloudwatch` namespace:

   ```
   kubectl apply -f cloudwatch-namespace.yaml
   ```

## Step 2: Install FluentD<a name="ContainerInsights-install-FluentD"></a>

Start this process by downloading FluentD\. When you finish these steps, the deployment creates the following resources on the cluster:
+ A service account named `fluentd` in the `amazon-cloudwatch` namespace\. This service account is used to run the FluentD DaemonSet\. For more information, see [Managing Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) in the Kubernetes Reference\.
+ A cluster role named `fluentd` in the `amazon-cloudwatch` namespace\. This cluster role grants `get`, `list`, and `watch` permissions on pod logs to the `fluentd` service account\. For more information, see [API Overview](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview/) in the Kubernetes Reference\.
+ A ConfigMap named `fluentd-config` in the `amazon-cloudwatch` namespace\. This ConfigMap contains the configuration to be used by FluentD\. For more information, see [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) in the Kubernetes Tasks documentation\.

**To install FluentD**

1. Download the FluentD deployment configuration by running the following command\.

   ```
   curl -O https://s3.amazonaws.com/cloudwatch-agent-k8s-yamls/fluentd/fluentd.yml
   ```

1. Create a ConfigMap named `cluster-info` with the cluster name and the AWS Region that the logs will be sent to\. Run the following command, updating the placeholders with your cluster and Region names\.

   ```
   kubectl create configmap cluster-info \
   --from-literal=cluster.name=cluster_name \
   --from-literal=logs.region=region_name -n amazon-cloudwatch
   ```

1. Deploy the FluentD DaemonSet to the cluster by running the following command\.

   ```
   kubectl apply -f fluentd.yml
   ```

1. Validate the deployment by running the following command\. Each node should have one pod named `fluentd-cloudwatch-*`\.

   ```
   kubectl get pods -n amazon-cloudwatch
   ```

## Step 3: Verify the FluentD Setup<a name="ContainerInsights-verify-FluentD"></a>

To verify your FluentD setup, use the following steps\.

**To verify the FluentD setup for Container Insights**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\. Make sure that you're in the Region where you deployed FluentD to your containers\.

   In the list of log groups in the Region, you should see the following:
   + `/aws/containerinsights/Cluster_Name/application`
   + `/aws/containerinsights/Cluster_Name/host`
   + `/aws/containerinsights/Cluster_Name/dataplane`

   If you see these log groups, the FluentD setup is verified\.

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
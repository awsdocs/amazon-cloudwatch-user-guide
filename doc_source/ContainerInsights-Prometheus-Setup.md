# Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters<a name="ContainerInsights-Prometheus-Setup"></a>

Before following these steps to install the CloudWatch agent for Prometheus metric collection, you must have a cluster running on Amazon EKS or a Kubernetes cluster running on an Amazon EC2 instance\.

**Topics**
+ [Setting Up IAM Roles](#ContainerInsights-Prometheus-Setup-roles)
+ [Installing the CloudWatch Agent to Collect Prometheus Metrics](#ContainerInsights-Prometheus-Setup-install-agent)

## Setting Up IAM Roles<a name="ContainerInsights-Prometheus-Setup-roles"></a>

The first step is to set up the necessary IAM policy in the cluster\. You do this by adding the policy to the IAM role used for the cluster\.

**To set up the IAM policy in a cluster for Prometheus support**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**\.

1. You need to find the prefix of the IAM role name for the cluster\. To do this, select the check box next to the name of an instance that is in the cluster, and choose **Actions**, **Instance Settings**, **Attach/Replace IAM Role**\. Then copy the prefix of the IAM role, such as `eksctl-dev303-workshop-nodegroup`\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Use the search box to find the prefix that you copied earlier in this procedure, and choose that role\.

1. Choose **Attach policies**\.

1. Use the search box to find **CloudWatchAgentServerPolicy**\. Select the check box next to **CloudWatchAgentServerPolicy**, and choose **Attach policy**\.

## Installing the CloudWatch Agent to Collect Prometheus Metrics<a name="ContainerInsights-Prometheus-Setup-install-agent"></a>

You must install the CloudWatch agent in the cluster to collect the metrics\. How to install the agent differs for Amazon EKS clusters and Kubernetes clusters\.

**Delete Previous Versions of the CloudWatch Agent with Prometheus Support**

If you have already installed a version of the CloudWatch agent with Prometheus support in your cluster, you must delete that version by entering the following command\. This is necessary only for previous versions of the agent with Prometheus support\. You do not need to delete the CloudWatch agent that enables Container Insights without Prometheus support\.

```
kubectl delete deployment cwagent-prometheus -n amazon-cloudwatch
```

### Installing the CloudWatch Agent on Amazon EKS<a name="ContainerInsights-Prometheus-Setup-install-agent-EKS"></a>

To install the CloudWatch agent with Prometheus support on an Amazon EKS cluster, follow these steps\.

**To install the CloudWatch agent with Prometheus support on an Amazon EKS cluster**

1. Enter the following command to check whether the `amazon-cloudwatch` namespace has already been created:

   ```
   kubectl get namespace
   ```

1. If `amazon-cloudwatch` is not displayed in the results, create it by entering the following command:

   ```
   kubectl create namespace amazon-cloudwatch
   ```

1. To deploy the agent with the default configuration and have it send data to the AWS Region that it is installed in, enter the following command:

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
   ```

   To have the agent send data to a different Region instead, follow these steps:

   1. Download the YAML file for the agent by entering the following command:

      ```
      curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
      ```

   1. Open the file with a text editor, and search for the `cwagentconfig.json` block of the file\.

   1. Add the highlighted lines, specifying the Region that you want:

      ```
      cwagentconfig.json: |
          {
            "agent": {
              "region": "us-east-2"
            },
            "logs": { ...
      ```

   1. Save the file and deploy the agent using your updated file\.

      ```
      kubectl apply -f prometheus-eks.yaml
      ```

### Installing the CloudWatch Agent on a Kubernetes Cluster<a name="ContainerInsights-Prometheus-Setup-install-agent-Kubernetes"></a>

To install the CloudWatch agent with Prometheus support on a cluster running Kubernetes, enter the following command:

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-k8s.yaml | 
sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" | 
kubectl apply -f -
```

Replace *MyCluster* with the name of the cluster\. This name is used in the log group name that stores the log events collected by the agent, and is also used as a dimension for the metrics collected by the agent\.

Replace *region* with the name of the AWS Region where you want the metrics to be sent\. For example, **us\-west\-1**\.

### Verify that the Agent is Running<a name="ContainerInsights-Prometheus-Setup-install-agent-verify"></a>

On both Amazon EKS and Kubernetes clusters, you can enter the following command to confirm that the agent is running\.

```
kubectl get pod -l "app=cwagent-prometheus" -n amazon-cloudwatch
```

If the results include a single CloudWatch agent pod in the `Running` state, the agent is running and collecting Prometheus metrics\. By default the CloudWatch agent collects metrics for App Mesh, NGINX, Memcached, Java/JMX, and HAProxy every minute\. For more information about those metrics, see [Prometheus Metrics Collected by the CloudWatch Agent](ContainerInsights-Prometheus-metrics.md)\. For instructions on how to see your Prometheus metrics in CloudWatch, see [Viewing Your Prometheus Metrics ](ContainerInsights-Prometheus-viewmetrics.md)

You can also configure the CloudWatch agent to collect metrics from other Prometheus exporters\. For more information, see [Configuring the CloudWatch Agent for Prometheus Monitoring](ContainerInsights-Prometheus-Setup-configure.md)\.
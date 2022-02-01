# Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters<a name="ContainerInsights-Prometheus-Setup"></a>

This section explains how to set up the CloudWatch agent with Prometheus monitoring in a cluster running Amazon EKS or Kubernetes\. After you do this, the agent automatically scrapes and imports metrics for the following workloads running in that cluster\.
+ AWS App Mesh
+ NGINX
+ Memcached
+ Java/JMX
+ HAProxy
+ Fluent Bit

You can also configure the agent to scrape and import additional Prometheus workloads and sources\.

Before following these steps to install the CloudWatch agent for Prometheus metric collection, you must have a cluster running on Amazon EKS or a Kubernetes cluster running on an Amazon EC2 instance\.

**VPC security group requirements**

The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.

The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

**Topics**
+ [Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters](#ContainerInsights-Prometheus-Setup-roles)
+ [Scraping additional Prometheus sources and importing those metrics](ContainerInsights-Prometheus-Setup-configure.md)
+ [\(Optional\) Set up sample containerized Amazon EKS workloads for Prometheus metric testing](ContainerInsights-Prometheus-Sample-Workloads.md)

## Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters<a name="ContainerInsights-Prometheus-Setup-roles"></a>

This section explains how to set up the CloudWatch agent with Prometheus monitoring in a cluster running Amazon EKS or Kubernetes\. After you do this, the agent automatically scrapes and imports metrics for the following workloads running in that cluster\.
+ AWS App Mesh
+ NGINX
+ Memcached
+ Java/JMX
+ HAProxy
+ Fluent Bit

You can also configure the agent to scrape and import additional Prometheus workloads and sources\.

Before following these steps to install the CloudWatch agent for Prometheus metric collection, you must have a cluster running on Amazon EKS or a Kubernetes cluster running on an Amazon EC2 instance\.

**VPC security group requirements**

The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.

The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

**Topics**
+ [Setting up IAM roles](#ContainerInsights-Prometheus-Setup-roles)
+ [Installing the CloudWatch agent to collect Prometheus metrics](#ContainerInsights-Prometheus-Setup-install-agent)

### Setting up IAM roles<a name="ContainerInsights-Prometheus-Setup-roles"></a>

The first step is to set up the necessary IAM role in the cluster\. There are two methods:
+ Set up an IAM role for a service account, also known as a *service role*\. This method works for both the EC2 launch type and the Fargate launch type\.
+ Add an IAM policy to the IAM role used for the cluster\. This works only for the EC2 launch type\.

**Set up a service role \(EC2 launch type and Fargate launch type\)**

To set up a service role, enter the following command\. Replace *MyCluster* with the name of the cluster\.

```
eksctl create iamserviceaccount \
 --name cwagent-prometheus \
--namespace amazon-cloudwatch \
 --cluster MyCluster \
--attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
--approve \
--override-existing-serviceaccounts
```

**Add a policy to the cluster's IAM role \(EC2 launch type only\)**

**To set up the IAM policy in a cluster for Prometheus support**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**\.

1. You need to find the prefix of the IAM role name for the cluster\. To do this, select the check box next to the name of an instance that is in the cluster, and choose **Actions**, **Instance Settings**, **Attach/Replace IAM Role**\. Then copy the prefix of the IAM role, such as `eksctl-dev303-workshop-nodegroup`\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Use the search box to find the prefix that you copied earlier in this procedure, and choose that role\.

1. Choose **Attach policies**\.

1. Use the search box to find **CloudWatchAgentServerPolicy**\. Select the check box next to **CloudWatchAgentServerPolicy**, and choose **Attach policy**\.

### Installing the CloudWatch agent to collect Prometheus metrics<a name="ContainerInsights-Prometheus-Setup-install-agent"></a>

You must install the CloudWatch agent in the cluster to collect the metrics\. How to install the agent differs for Amazon EKS clusters and Kubernetes clusters\.

**Delete previous versions of the CloudWatch agent with Prometheus support**

If you have already installed a version of the CloudWatch agent with Prometheus support in your cluster, you must delete that version by entering the following command\. This is necessary only for previous versions of the agent with Prometheus support\. You do not need to delete the CloudWatch agent that enables Container Insights without Prometheus support\.

```
kubectl delete deployment cwagent-prometheus -n amazon-cloudwatch
```

#### Installing the CloudWatch agent on Amazon EKS clusters with the EC2 launch type<a name="ContainerInsights-Prometheus-Setup-install-agent-EKS"></a>

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
**Note**  
The following setup step pulls the container image from Docker Hub as an anonymous user by default\. This pull may be subject to a rate limit\. For more information, see [CloudWatch agent container image](ContainerInsights.md#container-insights-download-limit)\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
   ```

   To have the agent send data to a different Region instead, follow these steps:

   1. Download the YAML file for the agent by entering the following command:

      ```
      curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
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

#### Installing the CloudWatch agent on Amazon EKS clusters with the Fargate launch type<a name="ContainerInsights-Prometheus-Setup-install-agent-EKS-fargate"></a>

To install the CloudWatch agent with Prometheus support on an Amazon EKS cluster with the Fargate launch type, follow these steps\.

**To install the CloudWatch agent with Prometheus support on an Amazon EKS cluster with the Fargate launch type**

1. Enter the following command to create a Fargate profile for the CloudWatch agent so that it can run inside the cluster\. Replace *MyCluster* with the name of the cluster\.

   ```
   eksctl create fargateprofile --cluster MyCluster \
   --name amazon-cloudwatch \
   --namespace amazon-cloudwatch
   ```

1. To install the CloudWatch agent, enter the following command\. Replace *MyCluster* with the name of the cluster\. This name is used in the log group name that stores the log events collected by the agent, and is also used as a dimension for the metrics collected by the agent\.

   Replace *region* with the name of the Region where you want the metrics to be sent\. For example, `us-west-1`\. 

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks-fargate.yaml | 
   sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" | 
   kubectl apply -f -
   ```

#### Installing the CloudWatch agent on a Kubernetes cluster<a name="ContainerInsights-Prometheus-Setup-install-agent-Kubernetes"></a>

To install the CloudWatch agent with Prometheus support on a cluster running Kubernetes, enter the following command:

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-k8s.yaml | 
sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" | 
kubectl apply -f -
```

Replace *MyCluster* with the name of the cluster\. This name is used in the log group name that stores the log events collected by the agent, and is also used as a dimension for the metrics collected by the agent\.

Replace *region* with the name of the AWS Region where you want the metrics to be sent\. For example, **us\-west\-1**\.

#### Verify that the agent is running<a name="ContainerInsights-Prometheus-Setup-install-agent-verify"></a>

On both Amazon EKS and Kubernetes clusters, you can enter the following command to confirm that the agent is running\.

```
kubectl get pod -l "app=cwagent-prometheus" -n amazon-cloudwatch
```

If the results include a single CloudWatch agent pod in the `Running` state, the agent is running and collecting Prometheus metrics\. By default the CloudWatch agent collects metrics for App Mesh, NGINX, Memcached, Java/JMX, and HAProxy every minute\. For more information about those metrics, see [Prometheus metrics collected by the CloudWatch agent](ContainerInsights-Prometheus-metrics.md)\. For instructions on how to see your Prometheus metrics in CloudWatch, see [Viewing your Prometheus metrics ](ContainerInsights-Prometheus-viewmetrics.md)

You can also configure the CloudWatch agent to collect metrics from other Prometheus exporters\. For more information, see [Scraping additional Prometheus sources and importing those metrics](ContainerInsights-Prometheus-Setup-configure.md)\.
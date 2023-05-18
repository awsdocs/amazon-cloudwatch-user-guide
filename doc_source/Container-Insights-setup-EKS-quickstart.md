# Quick Start setup for Container Insights on Amazon EKS and Kubernetes<a name="Container-Insights-setup-EKS-quickstart"></a>

To complete the setup of Container Insights, you can follow the quick start instructions in this section\.

**Important**  
Before completing the steps in this section, you must have verified the prerequisites including IAM permissions\. For more information, see [Verify prerequisites](Container-Insights-prerequisites.md)\. 

Alternatively, you can instead follow the instructions in the following two sections, [Set up the CloudWatch agent to collect cluster metrics](Container-Insights-setup-metrics.md) and [Send logs to CloudWatch Logs](Container-Insights-EKS-logs.md)\. Those sections provide more configuration details on how the CloudWatch agent works with Amazon EKS and Kubernetes, but require you to perform more installation steps\.

**Note**  
Amazon has now launched Fluent Bit as the default log solution for Container Insights with significant performance gains\. We recommend that you use Fluent Bit instead of Fluentd\.

## Quick Start with the CloudWatch agent and Fluent Bit<a name="Container-Insights-setup-EKS-quickstart-FluentBit"></a>

There are two configurations for Fluent Bit: an optimized version and a version that provides an experience more similar to Fluentd\. The Quick Start configuration uses the optimized version\. For more details about the Fluentd\-compatible configuration, see [Set up Fluent Bit as a DaemonSet to send logs to CloudWatch Logs](Container-Insights-setup-logs-FluentBit.md)\.

To deploy Container Insights using the quick start, enter the following command\.

**Note**  
The following setup step pulls the container image from Docker Hub as an anonymous user by default\. This pull may be subject to a rate limit\. For more information, see [CloudWatch agent container image](ContainerInsights.md#container-insights-download-limit)\.

```
ClusterName=<my-cluster-name>
RegionName=<my-cluster-region>
FluentBitHttpPort='2020'
FluentBitReadFromHead='Off'
[[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
[[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${RegionName}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f -
```

In this command, *my\-cluster\-name* is the name of your Amazon EKS or Kubernetes cluster, and *my\-cluster\-region* is the name of the Region where the logs are published\. We recommend that you use the same Region where your cluster is deployed to reduce the AWS outbound data transfer costs\.

For example, to deploy Container Insights on the cluster named `MyCluster` and publish the logs and metrics to US West \(Oregon\), enter the following command\.

```
ClusterName='MyCluster'
LogRegion='us-west-2'
FluentBitHttpPort='2020'
FluentBitReadFromHead='Off'
[[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
[[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${LogRegion}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f -
```

**Migrating from Fluentd**

If you already have Fluentd configured and want to move to Fluent Bit, you must delete the Fluentd pods after you install Fluent Bit\. You can use the following command to delete Fluentd\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/fluentd/fluentd.yaml | kubectl delete -f -
kubectl delete configmap cluster-info -n amazon-cloudwatch
```

**Deleting Container Insights**

If you want to remove Container Insights after using the quick start setup, enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${LogRegion}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl delete -f -
```

## Quick Start with the CloudWatch agent and Fluentd<a name="Container-Insights-setup-EKS-quickstart-Fluentd"></a>

If you are already using Fluentd in your Kubernetes cluster and want to extend it to be the log solution for Container Insights, we provide a Fluentd configuration for you to do so\.

**Warning**  
Container Insights support for Fluentd is now in maintenance mode, which means that AWS will not provide any further updates for Fluentd and that we are planning to deprecate it in near future\. Additionally, the current Fluentd configuration for Container Insights is using an old version of the Fluentd Image `fluent/fluentd-kubernetes-daemonset:v1.7.3-debian-cloudwatch-1.0` which does not have the latest improvement and security patches\. For the latest Fluentd image supported by the open source community, see [fluentd\-kubernetes\-daemonset](https://github.com/fluent/fluentd-kubernetes-daemonset)\.   
We strongly recommend that you migrate to use FluentBit with Container Insights whenever possible\. Using FluentBit as the log forwarder for Container Insights provides significant performance gains\.   
For more information, see [Set up Fluent Bit as a DaemonSet to send logs to CloudWatch Logs](Container-Insights-setup-logs-FluentBit.md) and [ Differences if you're already using Fluentd](Container-Insights-setup-logs-FluentBit.md#Container-Insights-setup-logs-FluentBit-migrate)\.

To deploy the CloudWatch agent and Fluentd using the quick start, use the following command\. The following setup contains a community supported Fluentd container image\. You can replace the image with your own Fluentd image as long as it meets the Fluentd image requirements\. For more information, see [\(Optional\) Set up Fluentd as a DaemonSet to send logs to CloudWatch Logs](Container-Insights-setup-logs.md)\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/cluster-name/;s/{{region_name}}/cluster-region/" | kubectl apply -f -
```

In this command, *cluster\-name* is the name of your Amazon EKS or Kubernetes cluster, and *cluster\-region* is the name of the Region where the logs are published\. We recommend that you use the same Region where your cluster is deployed to reduce the AWS outbound data transfer costs\.

For example, to deploy Container Insights on the cluster named `MyCluster` and publish the logs and metrics to US West \(Oregon\), enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/us-west-2/" | kubectl apply -f -
```

**Deleting Container Insights**

If you want to remove Container Insights after using the quick start setup, enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/cluster-name/;s/{{region_name}}/cluster-region/" | kubectl delete -f -
```
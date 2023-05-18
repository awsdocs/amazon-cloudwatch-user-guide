# Deleting the CloudWatch agent and Fluentd for Container Insights<a name="ContainerInsights-delete-agent"></a>

To delete all resources related to the CloudWatch agent and Fluentd, enter the following command\. In this command, *Cluster* is the name of your Amazon EKS or Kubernetes cluster, and *Region* is the name of the Region where the logs are published\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/Cluster_Name/;s/{{region_name}}/Region/" | kubectl delete -f -
```
# Using AWS Distro for OpenTelemetry<a name="Container-Insights-EKS-otel"></a>

Use the instructions in this section to set up Container Insights on an Amazon EKS cluster by using the AWS Distro for OpenTelemetry collector\. For more information about the AWS Distro for OpenTelemetry, see [AWS Distro for OpenTelemetry](https://aws.amazon.com/otel/)\. 

If you have not already done so, make sure that you have fulfilled the prerequisites including the necessary IAM roles\. For more information, see [Verify prerequisites](Container-Insights-prerequisites.md)\.

First, deploy the AWS Distro for OpenTelemetry collector as a DaemonSet by entering the following command\. 

```
curl https://raw.githubusercontent.com/aws-observability/aws-otel-collector/main/deployment-template/eks/otel-container-insights-infra.yaml |
kubectl apply -f -
```

To confirm that the collector is running, enter the following command\.

```
kubectl get pods -l name=aws-otel-eks-ci -n aws-otel-eks
```

If the output of this command includes multiple pods in the `Running` state, the collector is running and collecting metrics from the cluster\. The collector creates a log group named `aws/containerinsights/cluster-name/performance` and sends the performance log events to it\.

For information about how to see your Container Insights metrics in CloudWatch, see [Viewing Container Insights metrics](Container-Insights-view-metrics.md)\.

AWS has also provided documentation on GitHub for this scenario\. If you want to customize the metrics and logs published by Container Insights, see [https://aws\-otel\.github\.io/docs/getting\-started/container\-insights/eks\-infra](https://aws-otel.github.io/docs/getting-started/container-insights/eks-infra)\.
# Set Up HAProxy with a Metric Exporter on Amazon EKS and Kubernetes<a name="ContainerInsights-Prometheus-Sample-Workloads-haproxy"></a>

HAProxy is an open\-source proxy application\. For more information, see [HAProxy](https://www.haproxy.org)\.

If you are running HAProxy on a cluster with the Fargate launch type, you need to set up a Fargate profile before doing the steps in this procedure\. To set up the profile, enter the following command\. Replace *MyCluster* with the name of your cluster\.

```
eksctl create fargateprofile --cluster MyCluster \
--namespace haproxy-ingress-sample --name haproxy-ingress-sample
```

**To install HAProxy with a metric exporter to test Container Insights Prometheus support**

1. Enter the following command to add the Helm incubator repo:

   ```
   helm repo add haproxy-ingress https://haproxy-ingress.github.io/charts
   ```

1. Enter the following command to create a new namespace:

   ```
   kubectl create namespace haproxy-ingress-sample
   ```

1. Enter the following commands to install HAProxy:

   ```
   helm install haproxy haproxy-ingress/haproxy-ingress \
   --namespace haproxy-ingress-sample \
   --set defaultBackend.enabled=true \
   --set controller.stats.enabled=true \
   --set controller.metrics.enabled=true \
   --set-string controller.metrics.service.annotations."prometheus\.io/port"="9101" \
   --set-string controller.metrics.service.annotations."prometheus\.io/scrape"="true"
   ```

1. Enter the following command to confirm the annotation of the service:

   ```
   kubectl describe service haproxy-haproxy-ingress-metrics -n haproxy-ingress-sample
   ```

   You should see the following annotations\.

   ```
   Annotations:   prometheus.io/port: 9101
                  prometheus.io/scrape: true
   ```

**To uninstall HAProxy**
+ Enter the following commands:

  ```
  helm uninstall haproxy --namespace haproxy-ingress-sample
  kubectl delete namespace haproxy-ingress-sample
  ```
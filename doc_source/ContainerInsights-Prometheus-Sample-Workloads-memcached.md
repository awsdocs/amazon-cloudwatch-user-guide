# Set up memcached with a metric exporter on Amazon EKS and Kubernetes<a name="ContainerInsights-Prometheus-Sample-Workloads-memcached"></a>

memcached is an open\-source memory object caching system\. For more information, see [What is Memcached?](https://www.memcached.org)\.

If you are running memcached on a cluster with the Fargate launch type, you need to set up a Fargate profile before doing the steps in this procedure\. To set up the profile, enter the following command\. Replace *MyCluster* with the name of your cluster\.

```
eksctl create fargateprofile --cluster MyCluster \
--namespace memcached-sample --name memcached-sample
```

**To install memcached with a metric exporter to test Container Insights Prometheus support**

1. Enter the following command to add the repo:

   ```
   helm repo add bitnami https://charts.bitnami.com/bitnami
   ```

1. Enter the following command to create a new namespace:

   ```
   kubectl create namespace memcached-sample
   ```

1. Enter the following command to install Memcached

   ```
   helm install my-memcached bitnami/memcached --namespace memcached-sample \
   --set metrics.enabled=true \
   --set-string serviceAnnotations.prometheus\\.io/port="9150" \
   --set-string serviceAnnotations.prometheus\\.io/scrape="true"
   ```

1. Enter the following command to confirm the annotation of the running service:

   ```
   kubectl describe service my-memcached-metrics -n memcached-sample
   ```

   You should see the following two annotations:

   ```
   Annotations:   prometheus.io/port: 9150
                  prometheus.io/scrape: true
   ```

**To uninstall memcached**
+ Enter the following commands:

  ```
  helm uninstall my-memcached --namespace memcached-sample
  kubectl delete namespace memcached-sample
  ```
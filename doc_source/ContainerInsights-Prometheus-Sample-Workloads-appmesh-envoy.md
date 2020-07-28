# \(Optional\) Enable App Mesh Envoy Access Logs<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy"></a>

You can set up Container Insights FluentD to send App Mesh Envoy access logs to CloudWatch Logs\. For more information, see [Access logs](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html#envoy-logs)\.

**To have Envoy access logs sent to CloudWatch Logs**

1. Set up FluentD in the cluster\. For more information, see [Set Up FluentD as a DaemonSet to Send Logs to CloudWatch Logs](Container-Insights-setup-logs.md)\.

1. Configure Envoy access logs for your virtual nodes\. Be sure to configure the log path to be **/dev/stdout** in each virtual node\. There are three methods for enabling and configuring the logs:

   1. Use the App Mesh console\. For instructions on how to use the App Mesh console to configure the logs, see [Access logs](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy.html#envoy-logs)\.

   1. To use the App Mesh CLI to enable or update the Envoy access logs, use the `update-virtual-node` command\. For more information, see [update\-virtual\-node](https://docs.aws.amazon.com/cli/latest/reference/appmesh/update-virtual-node.html)\.

   1. If you created Virtual Nodes using App Mesh Kubernetes Controller, use this option. To use the YAML file, add the bold `logging` section of the YAML file shown below when you create or update your virtual node. The following sample YAML is for [howto-k8s-http-headers walkthrough](https://github.com/aws/aws-app-mesh-examples/tree/master/walkthroughs/howto-k8s-http-headers) used earlier\.

      ```
      ---
      apiVersion: appmesh.k8s.aws/v1beta2
      kind: VirtualNode
      metadata:
        name: blue
        namespace: howto-k8s-http-headers
      spec:
        podSelector:
          matchLabels:
            app: color
            version: blue
        listeners:
          - portMapping:
              port: 8080
              protocol: http
            healthCheck:
              protocol: http
              path: '/ping'
              healthyThreshold: 2
              unhealthyThreshold: 2
              timeoutMillis: 2000
              intervalMillis: 5000
        serviceDiscovery:
          dns:
            hostname: color-blue.howto-k8s-http-headers.svc.cluster.local
        logging:
          accessLog:
            file:
              path: "/dev/stdout"
      ```

When you have finished, the envoy access logs are sent to the `/aws/containerinsights/Cluster_Name/application` log group\.

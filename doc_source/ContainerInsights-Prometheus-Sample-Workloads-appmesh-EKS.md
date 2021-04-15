# Set Up AWS App Mesh sample workload on an Amazon EKS cluster with the EC2 launch type or a Kubernetes cluster<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-EKS"></a>

Use these instructions if you are setting up App Mesh on a cluster running Amazon EKS with the EC2 launch type, or a Kubernetes cluster\.

## Configure IAM Permissions<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-iam"></a>

You must add the **AWSAppMeshFullAccess** policy to the IAM role for your Amazon EKS or Kubernetes node group\. On Amazon EKS, this node group name looks similar to eksctl\-integ\-test\-eks\-prometheus\-NodeInstanceRole\-ABCDEFHIJKL\. On Kubernetes, it might look similar to nodes\.integ\-test\-kops\-prometheus\.k8s\.local\.

## Install App Mesh<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-install"></a>

To install the App Mesh Kubernetes controller, follow the instructions in [App Mesh Controller](https://github.com/aws/eks-charts/tree/master/stable/appmesh-controller#app-mesh-controller)\.

## Install a Sample Application<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-application"></a>

[aws\-app\-mesh\-examples](https://github.com/aws/aws-app-mesh-examples) contains several Kubernetes App Mesh walkthroughs\. For this tutorial, you install a sample color application that shows how http routes can use headers for matching incoming requests\.

**To use a sample App Mesh application to test Container Insights**

1. Install the application using these instructions: [https://github.com/aws/aws-app-mesh-examples/tree/master/walkthroughs/howto-k8s-http-headers](https://github.com/aws/aws-app-mesh-examples/tree/master/walkthroughs/howto-k8s-http-headers)\. 

1. Launch a curler pod to generate traffic:

   ```
   kubectl -n default run -it curler --image=tutum/curl /bin/bash
   ```

1. Curl different endpoints by changing HTTP headers\. Run the curl command multiple times, as shown:

   ```
   curl -H "color_header: blue" front.howto-k8s-http-headers.svc.cluster.local:8080/; echo;
   
   curl -H "color_header: red" front.howto-k8s-http-headers.svc.cluster.local:8080/; echo;
   
   curl -H "color_header: yellow" front.howto-k8s-http-headers.svc.cluster.local:8080/; echo;
   ```

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the AWS Region where your cluster is running, choose **Metrics** in the navigation pane\. The metric are in the **ContainerInsights/Prometheus** namespace\.

1. To see the CloudWatch Logs events, choose **Log groups** in the navigation pane\. The events are in the log group ` /aws/containerinsights/your_cluster_name/prometheus ` in the log stream `kubernetes-pod-appmesh-envoy`\.

## Deleting the App Mesh Test Environment<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-delete"></a>

When you have finished using App Mesh and the sample application, use the following commands to delete the unnecessary resources\. Delete the sample application by entering the following command:

```
cd aws-app-mesh-examples/walkthroughs/howto-k8s-http-headers/
kubectl delete -f _output/manifest.yaml
```

Delete the App Mesh controller by entering the following command:

```
helm delete appmesh-controller -n appmesh-system
```
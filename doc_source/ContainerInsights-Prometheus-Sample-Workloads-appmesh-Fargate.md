# Set Up AWS App Mesh sample workload on an Amazon EKS cluster with the Fargate launch type<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-Fargate"></a>

Use these instructions if you are setting up App Mesh on a cluster running Amazon EKS with the Fargate launch type\.

## Configure IAM Permissions<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh--fargate-iam"></a>

To set up IAM permissions, enter the following command\. Replace *MyCluster* with the name of your cluster\.

```
eksctl create iamserviceaccount --cluster MyCluster \
 --namespace howto-k8s-fargate \
 --name appmesh-pod \
 --attach-policy-arn arn:aws:iam::aws:policy/AWSAppMeshEnvoyAccess \
 --attach-policy-arn arn:aws:iam::aws:policy/AWSCloudMapDiscoverInstanceAccess \
 --attach-policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess \
 --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess \
 --attach-policy-arn arn:aws:iam::aws:policy/AWSAppMeshFullAccess \
 --attach-policy-arn arn:aws:iam::aws:policy/AWSCloudMapFullAccess \
 --override-existing-serviceaccounts \
 --approve
```

## Install App Mesh<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-fargate-install"></a>

To install the App Mesh Kubernetes controller, follow the instructions in [App Mesh Controller](https://github.com/aws/eks-charts/tree/master/stable/appmesh-controller#app-mesh-controller)\. Be sure to follow the instructions for Amazon EKS with the Fargate launch type\.

## Install a Sample Application<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-fargate-application"></a>

[aws\-app\-mesh\-examples](https://github.com/aws/aws-app-mesh-examples) contains several Kubernetes App Mesh walkthroughs\. For this tutorial, you install a sample color application that works for Amazon EKS clusters with the Fargate launch type\.

**To use a sample App Mesh application to test Container Insights**

1. Install the application using these instructions: [https://github.com/aws/aws-app-mesh-examples/tree/master/walkthroughs/howto-k8s-fargate](https://github.com/aws/aws-app-mesh-examples/tree/master/walkthroughs/howto-k8s-fargate)\. 

   Those instructions assume that you are creating a new cluster wtih the correct Fargate profile\. If you want to use an Amazon EKS cluster that you've already set up, you can use the following commands to set up that cluster for this demonstration\. Replace *MyCluster* with the name of your cluster\.

   ```
   eksctl create iamserviceaccount --cluster MyCluster \
    --namespace howto-k8s-fargate \
    --name appmesh-pod \
    --attach-policy-arn arn:aws:iam::aws:policy/AWSAppMeshEnvoyAccess \
    --attach-policy-arn arn:aws:iam::aws:policy/AWSCloudMapDiscoverInstanceAccess \
    --attach-policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess \
    --attach-policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess \
    --attach-policy-arn arn:aws:iam::aws:policy/AWSAppMeshFullAccess \
    --attach-policy-arn arn:aws:iam::aws:policy/AWSCloudMapFullAccess \
    --override-existing-serviceaccounts \
    --approve
   ```

   ```
   eksctl create fargateprofile --cluster MyCluster \
   --namespace howto-k8s-fargate --name howto-k8s-fargate
   ```

1. Port forward the front application deployment:

   ```
   kubectl -n howto-k8s-fargate port-forward deployment/front 8080:8080
   ```

1. Curl the front app:

   ```
   while true; do  curl -s http://localhost:8080/color; sleep 0.1; echo ; done
   ```

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the AWS Region where your cluster is running, choose **Metrics** in the navigation pane\. The metric are in the **ContainerInsights/Prometheus** namespace\.

1. To see the CloudWatch Logs events, choose **Log groups** in the navigation pane\. The events are in the log group ` /aws/containerinsights/your_cluster_name/prometheus ` in the log stream `kubernetes-pod-appmesh-envoy`\.

## Deleting the App Mesh Test Environment<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-fargate-delete"></a>

When you have finished using App Mesh and the sample application, use the following commands to delete the unnecessary resources\. Delete the sample application by entering the following command:

```
cd aws-app-mesh-examples/walkthroughs/howto-k8s-fargate/
kubectl delete -f _output/manifest.yaml
```

Delete the App Mesh controller by entering the following command:

```
helm delete appmesh-controller -n appmesh-system
```
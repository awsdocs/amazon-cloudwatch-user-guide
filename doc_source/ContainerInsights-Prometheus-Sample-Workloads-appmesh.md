# \(Optional\) Set Up AWS App Mesh<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh"></a>

The beta of Prometheus support in CloudWatch Container Insights supports AWS App Mesh\. This section explains how to set up App Mesh\.

CloudWatch Container Insights can also collect App Mesh Envoy Access Logs\. For more information, see [\(Optional\) Enable App Mesh Envoy Access Logs](ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy.md)\. 

## Configure IAM Permissions<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-iam"></a>

You must add the **AWSAppMeshFullAccess** policy to the IAM role for your Amazon EKS or Kubernetes node group\. On Amazon EKS, this node group name looks similar to eksctl\-integ\-test\-eks\-prometheus\-NodeInstanceRole\-ABCDEFHIJKL\. On Kubernetes, it might look similar to nodes\.integ\-test\-kops\-prometheus\.k8s\.local\.

## Install App Mesh<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-install"></a>

Follow these steps to install App Mesh\.

**To install App Mesh for testing with CloudWatch Container Insights Prometheus support**

1. Enter the following command to add the Helm repo\. This repo works on both Amazon EKS and Kubernetes clusters\.

   ```
   helm repo add eks https://aws.github.io/eks-charts
   ```

1. Enter the following command to apply the App Mesh custom resource definition:

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws/eks-charts/master/stable/appmesh-controller/crds/crds.yaml
   ```

1. Set the environment variables for the App Mesh workload\. The following lines are an example, you can choose your own environment variables:

   ```
   APPMESH_NAME=appmesh-sample
   APPMESH_SYSTEM_NAMESPACE=appmesh-system-sample
   APPMESH_SAMPLE_TRAFFIC_NAMESPACE=appmesh-sample-traffic-test
   ```

1. Install the App Mesh custom resource definition controller by entering the following commands:

   ```
   kubectl create namespace $APPMESH_SYSTEM_NAMESPACE
   helm upgrade -i appmesh-controller eks/appmesh-controller \
   --namespace $APPMESH_SYSTEM_NAMESPACE
   ```

1. Install the App Mesh injector by entering the following command:

   ```
   helm upgrade -i appmesh-inject eks/appmesh-inject \
   --namespace $APPMESH_SYSTEM_NAMESPACE \
   --set mesh.create=true \
   --set mesh.name=$APPMESH_NAME \
   --set stats.tagsEnabled=true
   ```

1. \(Optional\) To confirm that the sample service mesh has been created, 0pen the App Mesh console at [https://console\.aws\.amazon\.com/app\-mesh/](https://console.aws.amazon.com/app-mesh/)\. Set the console to the AWS Region where your cluster is running, and choose **Meshes**\. You should see a new mesh named appmesh\-sample\.

1. Create App Mesh testing traffic by entering the following command:

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/appmesh/appmesh_traffic_sample.yaml | 
   sed "s/{{appmeshname}}/$APPMESH_NAME/g" | 
   sed "s/{{namespace}}/$APPMESH_SAMPLE_TRAFFIC_NAMESPACE/g" | 
   kubectl apply -f -
   ```

1. \(Optional\) Install a second App Mesh workload service to generate 404 and 503 response codes by entering the following command:

   ```
   APPMESH_SAMPLE_ERROR_TRAFFIC_NAMESPACE=appmesh-sample-errortraffic-test
   ```

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/appmesh/appmesh_traffic_sample_4xx5xx.yaml | 
   sed "s/{{appmeshname}}/$APPMESH_NAME/g" | 
   sed "s/{{namespace}}/$APPMESH_SAMPLE_ERROR_TRAFFIC_NAMESPACE/g" | 
   kubectl apply -f -
   ```

1. Find the name of the traffic generator by entering the following command:

   ```
   kubectl get pod -n $APPMESH_SAMPLE_TRAFFIC_NAMESPACE 
   ```

   The traffic generator is in the NAME column and starts with `traffic-generator`

1. Find the name of the virtual service by entering the following command:

   ```
   kubectl get virtualservice.appmesh.k8s.aws -n $APPMESH_SAMPLE_TRAFFIC_NAMESPACE
   ```

1. Remote exec into the traffic generator pod by entering the following comman\. Substitute *traffic\-generator\-name* with the name of the traffic generator that you found in step 9\.

   ```
   kubectl exec -ti traffic-generator-name bash -n $APPMESH_SAMPLE_TRAFFIC_NAMESPACE
   ```

1. Run the curl command multiple times, using the name of the virtual service that you found in step 10\.

   ```
   curl virtual_servie_name:9080;echo
   ```

1. Exit the pod by entering the following command:

   ```
   Exit
   ```

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the AWS Region where your cluster is running, choose **Metrics**\. The App Mesh meetrics are in the **ContainerInsights/Prometheus** namespace\.

1. To see the CloudWatch Logs events, choose **Log Groups** in the navigation pane\. The events are in the log group **/aws/containerinsights/*your\_cluster\_name*/prometheus**, in the log stream **kubernetes\-pod\-appmesh\-envoyappmesh**\.

## Troubleshooting App Mesh<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-troubleshooting"></a>

The following can help you troubleshoot your App Mesh testing setup\.

**Pod Initialized in Pending Status**

This example setup creates 20 pods\. You can check whether any pods are still pending by entering the following commands:

```
kubectl get pod -n $APPMESH_SYSTEM_NAMESPACE
kubectl get pod -n $APPMESH_SAMPLE_TRAFFIC_NAMESPACE
kubectl get pod -n $APPMESH_SAMPLE_ERROR_TRAFFIC_NAMESPACE
```

If any pods are pending, your cluster might not have enough CPU or memory capacity to provision the required pods\. To solve this, provision a new EC2 instance to your cluster\.

**Error Running Load Generator**

You might see the following message when you run the load generator:

```
curl: (7) Failed to connect to jazz.appmesh-sample-traffic-test.svc.cluster.local port 9080: Connection refused
```

This could that the App Mesh IAM policy is not set up on the node group\. To solve this, add the **AWSAppMeshFullAccess** policy to the IAM role for your Amazon EKS or Kubernetes node group\. On Amazon EKS, this node group name looks similar to **eksctl\-integ\-test\-eks\-prometheus\-NodeInstanceRole\-ABCDEFHIJKL**\. On Kubernetes, it might look similar to **nodes\.integ\-test\-kops\-prometheus\.k8s\.local**\.

Once you have done this, confirm that the sample service mesh has been created\. 0pen the App Mesh console at [https://console\.aws\.amazon\.com/app\-mesh/](https://console.aws.amazon.com/app-mesh/)\. Set the console to the AWS Region where your cluster is running, and choose **Meshes**\. You should see a new mesh named appmesh\-sample\.

## Deleting The App Mesh Test Environment<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-cleanup"></a>

When you have finished using App Mesh and the traffic generator for testing, use the following commands to delete the unnecessary resources\.

Delete the sample traffic by entering the following command:

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/appmesh/appmesh_traffic_sample_4xx5xx.yaml | 
sed "s/{{appmeshname}}/$APPMESH_NAME/g" | 
sed "s/{{namespace}}/$APPMESH_SAMPLE_ERROR_TRAFFIC_NAMESPACE/g" | 
kubectl delete -f -curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/appmesh/appmesh_traffic_sample.yaml | 
sed "s/{{appmeshname}}/$APPMESH_NAME/g" | 
sed "s/{{namespace}}/$APPMESH_SAMPLE_TRAFFIC_NAMESPACE/g" | 
kubectl delete -f -
```

Delete the App Mesh controller and injector by entering the following command:

```
kubectl delete namespace appmesh-system-sample
```

Delete the App Mesh controller cluster\-level objects:

```
kubectl delete clusterrole appmesh-controller
kubectl delete clusterrolebinding appmesh-controller
```

Delete the App Mesh injector cluster level objects:

```
kubectl delete clusterrole appmesh-inject
kubectl delete clusterrolebinding appmesh-inject
kubectl delete MutatingWebhookConfiguration appmesh-inject
```
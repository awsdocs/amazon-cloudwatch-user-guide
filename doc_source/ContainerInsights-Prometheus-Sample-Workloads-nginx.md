# Set up NGINX with sample traffic on Amazon EKS and Kubernetes<a name="ContainerInsights-Prometheus-Sample-Workloads-nginx"></a>

NGINX is a web server that can also be used as a load balancer and reverse proxy\. For more information, see [NGINX](https://www.nginx.com/)\.

**To install NGINX with a sample traffic service to test Container Insights Prometheus support**

1. Enter the following command to add the Helm ingress\-nginx repo:

   ```
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   ```

1. Enter the following commands:

   ```
   kubectl create namespace nginx-ingress-sample
   
   helm install my-nginx ingress-nginx/ingress-nginx \
   --namespace nginx-ingress-sample \
   --set controller.metrics.enabled=true \
   --set-string controller.metrics.service.annotations."prometheus\.io/port"="10254" \
   --set-string controller.metrics.service.annotations."prometheus\.io/scrape"="true"
   ```

1. Check whether the services started correctly by entering the following command:

   ```
   kubectl get service -n nginx-ingress-sample
   ```

   The output of this command should display several columns, including an EXTERNAL\-IP column\.

1. Set an `EXTERNAL-IP` variable to the value of the EXTERNAL\-IP column in the row of the NGINX ingress controller\.

   ```
   EXTERNAL_IP=your-nginx-controller-external-ip
   ```

1. Start some sample NGINX traffic by entering the following command\. 

   ```
   SAMPLE_TRAFFIC_NAMESPACE=nginx-sample-traffic
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_traffic/nginx-traffic/nginx-traffic-sample.yaml | 
   sed "s/{{external_ip}}/$EXTERNAL_IP/g" | 
   sed "s/{{namespace}}/$SAMPLE_TRAFFIC_NAMESPACE/g" | 
   kubectl apply -f -
   ```

1. Enter the following command to confirm that all three pods are in the Running status\.

   ```
   kubectl get pod -n $SAMPLE_TRAFFIC_NAMESPACE
   ```

   If they are running, you should soon see metrics in the **ContainerInsights/Prometheus** namespace\.

**To uninstall NGINX and the sample traffic application**

1. Delete the sample traffic service by entering the following command:

   ```
   kubectl delete namespace $SAMPLE_TRAFFIC_NAMESPACE
   ```

1. Delete the NGINX egress by the Helm release name\. 

   ```
   helm uninstall my-nginx --namespace nginx-ingress-sample
   kubectl delete namespace nginx-ingress-sample
   ```
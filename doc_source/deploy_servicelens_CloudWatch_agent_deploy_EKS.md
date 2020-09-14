# Deploying the CloudWatch Agent and the X\-Ray Daemon on Amazon EKS or Kubernetes<a name="deploy_servicelens_CloudWatch_agent_deploy_EKS"></a>

These topics explain how to install the X\-Ray daemon and the CloudWatch agent on Amazon EKS or Kubernetes\.

## Deploying the X\-Ray Daemon on Amazon EKS or Kubernetes<a name="deploy_servicelens_CloudWatch_agent_deploy_Xray_daemon"></a>

To install the CloudWatch agent and the X\-Ray daemon on Amazon EKS or Kubernetes, you can use a quick setup\.

**To install the CloudWatch agent and the X\-Ray daemon on Amazon EKS or Kubernetes**

1. Ensure that the IAM role that is attached to the EC2 instance, or the Kubernetes worker node, has the **CloudWatchAgentServerPolicy** and **AWSXRayDaemonWriteAccess** policies attached\.

1. Enter the following command:

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/daemonset/cwagent-fluentd-xray/cwagent-fluentd-xray-quickstart.yaml | sed "s/{{cluster_name}}/cluster-name/;s/{{region_name}}/region/" | kubectl apply -f -
   ```

**What the Quick Start Does**

This section describes the quick setup of the CloudWatch agent and the X\-Ray daemon\.
+ The quick setup specifies inbound ports and protocols\. Outbound connections do not have to be explicitly opened\.
+ The quick setup installs the X\-Ray daemon via the `kubectl apply -f` command with the following file content\.

  ```
  apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    name: xray-daemon
    namespace: amazon-cloudwatch
  spec:
    selector:
      matchLabels:
        name: xray-daemon
    template:
      metadata:
        labels:
          name: xray-daemon
      spec:
        containers:
          - name: xray-daemon
            image: amazon/aws-xray-daemon:latest
            imagePullPolicy: Always
            ports:
              - containerPort: 2000
                hostPort: 2000
                protocol: UDP
            resources:
              limits:
                cpu:  100m
                memory: 256Mi
              requests:
                cpu: 50m
                memory: 50Mi
        terminationGracePeriodSeconds: 60
  ```
+ The quick setup updates the CloudWatch agent using the Docker image version/label 1\.231221\.0 or later, or the latest version\. You can find the image at [https://hub\.docker\.com/r/amazon/cloudwatch\-agent](https://hub.docker.com/r/amazon/cloudwatch-agent)\.
+ To enable the X\-Ray SDK to read cluster name and Region information, the quick setup updates the CloudWatch agent using the Docker image version/label 1\.231221\.0, or later, or the latest\. You can find the image at [https://hub\.docker\.com/r/amazon/cloudwatch\-agent](https://hub.docker.com/r/amazon/cloudwatch-agent)\.
+ To enable the X\-Ray SDK to read cluster name and Region information, the quick setup created a file with the following content, and then applied it with the **kubectl apply \-f** command\.

  ```
  ---
  # create role binding for XRay SDK to read config map
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
  name: container-insights-discovery-role
  namespace: amazon-cloudwatch
  rules:
  - apiGroups:
  - ""
  resourceNames:
  - cluster-info
  resources:
  - configmaps
  verbs:
  - get
   
  ---
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
  name: service-users-cloudwatch-discovery-role-binding
  namespace: amazon-cloudwatch
  roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: container-insights-discovery-role
  subjects:
  - apiGroup: rbac.authorization.k8s.io
  kind: Group
                        
  name: system:serviceaccounts
  ```
+ The quick setup exposes the CloudWatch agent port that receives X\-Ray SDK metrics\. The default port is UDP 25888\.

  ```
  ports:
    - containerPort: 25888
      hostPort: 25888
      protocol: UDP
  ```
+ The quick setup merges the agent configuration JSON with the X\-Ray SDK metrics configuration with the following JSON\.

  ```
  {
    "logs": {
      "metrics_collected": {
        "emf": {}
      }
    }
  }
  ```
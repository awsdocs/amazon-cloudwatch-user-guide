# Troubleshooting Container Insights<a name="ContainerInsights-troubleshooting"></a>

The following sections can help if you're having trouble issues with Container Insights\.

## Failed Deployment on Amazon EKS or Kubernetes<a name="ContainerInsights-setup-EKS-troubleshooting-general"></a>

If the agent doesn't deploy correctly on a Kubernetes cluster, try the following:
+ Run the following command to get the list of pods\.

  ```
  kubectl get pods -n amazon-cloudwatch
  ```
+ Run the following command and check the events at the bottom of the output\.

  ```
  kubectl describe pod pod-name -n amazon-cloudwatch
  ```
+ Run the following command to check the logs\.

  ```
  kubectl logs pod-name -n amazon-cloudwatch
  ```

## Unauthorized panic: Cannot retrieve cadvisor data from kubelet<a name="ContainerInsights-setup-EKS-troubleshooting-permissions"></a>

If your deployment fails with the error `Unauthorized panic: Cannot retrieve cadvisor data from kubelet`, your kubelet might not have Webhook authorization mode enabled\. This mode is required for Container Insights\. For more information, see [Verify Prerequisites](Container-Insights-prerequisites.md)\.

## Deploying Container Insights on a Deleted and Re\-Created Cluster<a name="ContainerInsights-troubleshooting-recreate"></a>

If you delete an existing cluster that does not have Container Insights enabled, and you re\-create it with the same name, you can't enable Container Insights on this new cluster at the time you re\-create it\. You can enable it by re\-creating it, and then entering the following command:

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=enabled
```

## Metrics Don't Appear in the Console<a name="ContainerInsights-setup-EKS-troubleshooting-nometrics"></a>

If you don't see any Container Insights metrics in the AWS Management Console, be sure that you have completed the setup of Container Insights\. Metrics don't appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.

## CrashLoopBackoff Error on the CloudWatch Agent<a name="ContainerInsights-troubleshooting-crashloopbackoff"></a>

If you see a `CrashLoopBackOff` error for the CloudWatch agent, make sure that your IAM permissions are set correctly\. For more information, see [Verify Prerequisites](Container-Insights-prerequisites.md)\.

## CloudWatch Agent or FluentD Pod Stuck in Pending<a name="ContainerInsights-troubleshooting-pending"></a>

If you have a CloudWatch agent or FluentD pod stuck in `Pending` or with a `FailedScheduling` error, determine if your nodes have enough compute resources based on the number of cores and amount of RAM required by the agents\. Enter the following command to describe the pod:

```
kubectl describe pod cloudwatch-agent-85ppg -n amazon-cloudwatch
```
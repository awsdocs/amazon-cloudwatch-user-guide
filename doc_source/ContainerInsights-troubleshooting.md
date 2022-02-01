# Troubleshooting Container Insights<a name="ContainerInsights-troubleshooting"></a>

The following sections can help if you're having trouble issues with Container Insights\.

## Failed deployment on Amazon EKS or Kubernetes<a name="ContainerInsights-setup-EKS-troubleshooting-general"></a>

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

If your deployment fails with the error `Unauthorized panic: Cannot retrieve cadvisor data from kubelet`, your kubelet might not have Webhook authorization mode enabled\. This mode is required for Container Insights\. For more information, see [Verify prerequisites](Container-Insights-prerequisites.md)\.

## Deploying Container Insights on a deleted and re\-created cluster on Amazon ECS<a name="ContainerInsights-troubleshooting-recreate"></a>

If you delete an existing Amazon ECS cluster that does not have Container Insights enabled, and you re\-create it with the same name, you can't enable Container Insights on this new cluster at the time you re\-create it\. You can enable it by re\-creating it, and then entering the following command:

```
aws ecs update-cluster-settings --cluster myCICluster --settings name=containerInsights,value=enabled
```

## Invalid endpoint error<a name="ContainerInsights-setup-invalid-endpoint"></a>

If you see an error message similar to the following, check to make sure that you replaced all the placeholders such as *cluster\-name* and *region\-name* in the commands that you are using with the correct information for your deployment\.

```
"log": "2020-04-02T08:36:16Z E! cloudwatchlogs: code: InvalidEndpointURL, message: invalid endpoint uri, original error: &url.Error{Op:\"parse\", URL:\"https://logs.{{region_name}}.amazonaws.com/\", Err:\"{\"}, &awserr.baseError{code:\"InvalidEndpointURL\", message:\"invalid endpoint uri\", errs:[]error{(*url.Error)(0xc0008723c0)}}\n",
```

## Metrics don't appear in the console<a name="ContainerInsights-setup-EKS-troubleshooting-nometrics"></a>

If you don't see any Container Insights metrics in the AWS Management Console, be sure that you have completed the setup of Container Insights\. Metrics don't appear before Container Insights has been set up completely\. For more information, see [Setting up Container Insights](deploy-container-insights.md)\.

## Pod metrics missing on Amazon EKS or Kubernetes after upgrading cluster<a name="ContainerInsights-troubleshooting-podmetrics-missing"></a>

This section might be useful if you all or some pod metrics are missing after you deploy the CloudWatch agent as a daemonset on a new or upgraded cluster, or you see an error log with the message W\! No pod metric collected\.

These errors can be caused by changes in the container runtime, such as containerd or the docker systemd cgroup driver\. You can usually solve this by updating your deployment manifest so that the containerd socket from the host is mounted into the container\. See the following example:

```
# For full example see https://github.com/aws-samples/amazon-cloudwatch-container-insights/blob/master/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/cwagent/cwagent-daemonset.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: cloudwatch-agent
  namespace: amazon-cloudwatch
spec:
  template:
    spec:
      containers:
        - name: cloudwatch-agent
# ...
          # Don't change the mountPath
          volumeMounts:
# ...
            - name: dockersock
              mountPath: /var/run/docker.sock
              readOnly: true
            - name: varlibdocker
              mountPath: /var/lib/docker
              readOnly: true
            - name: containerdsock # NEW mount
              mountPath: /run/containerd/containerd.sock
              readOnly: true
# ...
      volumes:
# ...
        - name: dockersock
          hostPath:
            path: /var/run/docker.sock
        - name: varlibdocker
          hostPath:
            path: /var/lib/docker
        - name: containerdsock # NEW volume
          hostPath:
            path: /run/containerd/containerd.sock
```

## No pod metrics when using Bottlerocket for Amazon EKS<a name="ContainerInsights-troubleshooting-bottlerocket"></a>

Bottlerocket is a Linux\-based open source operating system that is purpose\-built by AWS for running containers\. 

Bottlerocket uses a different `containerd` path on the host, so you need to change the volumes to its location\. If you don't, you see an error in the logs that includes W\! No pod metric collected\. See the following example\.

```
volumes:
  # ... 
    - name: containerdsock
      hostPath:
        # path: /run/containerd/containerd.sock
        # bottlerocket does not mount containerd sock at normal place
        # https://github.com/bottlerocket-os/bottlerocket/commit/91810c85b83ff4c3660b496e243ef8b55df0973b
        path: /run/dockershim.sock
```

## No container filesystem metrics when using the containerd runtime for Amazon EKS or Kubernetes<a name="ContainerInsights-troubleshooting-containerd"></a>

This is a known issue and is being worked on by community contributors\. For more information, see [Disk usage metric for containerd](https://github.com/google/cadvisor/issues/2785) and [container file system metrics is not supported by cadvisor for containerd](https://github.com/aws/amazon-cloudwatch-agent/issues/192) on GitHub\.

## Unexpected log volume increase from CloudWatch agent when collecting Prometheus metrics<a name="ContainerInsights-troubleshooting-log-volume-increase"></a>

This was a regression introduced in version 1\.247347\.6b250880 of the CloudWatch agent\. This regression has already been fixed in more recent versions of the agent\. It's impact was limited to scenarios where customers collected the logs of the CloudWatch agent itself and were also using Prometheus\. For more information, see [\[prometheus\] agent is printing all the scraped metrics in log](https://github.com/aws/amazon-cloudwatch-agent/issues/209) on GitHub\.

## Latest docker image mentioned in release notes not found from Dockerhub<a name="ContainerInsights-troubleshooting-docker-image"></a>

We update the release note and tag on Github before we start the actual release internally\. It usually takes 1\-2 weeks to see the latest docker image on registries after we bump the version number on Github\. There is no nightly release for the CloudWatch agent container image\. You can build the image directly from source at the following location: [https://github\.com/aws/amazon\-cloudwatch\-agent/tree/master/amazon\-cloudwatch\-container\-insights/cloudwatch\-agent\-dockerfile](https://github.com/aws/amazon-cloudwatch-agent/tree/master/amazon-cloudwatch-container-insights/cloudwatch-agent-dockerfile)

## CrashLoopBackoff error on the CloudWatch agent<a name="ContainerInsights-troubleshooting-crashloopbackoff"></a>

If you see a `CrashLoopBackOff` error for the CloudWatch agent, make sure that your IAM permissions are set correctly\. For more information, see [Verify prerequisites](Container-Insights-prerequisites.md)\.

## CloudWatch agent or FluentD pod stuck in pending<a name="ContainerInsights-troubleshooting-pending"></a>

If you have a CloudWatch agent or FluentD pod stuck in `Pending` or with a `FailedScheduling` error, determine if your nodes have enough compute resources based on the number of cores and amount of RAM required by the agents\. Enter the following command to describe the pod:

```
kubectl describe pod cloudwatch-agent-85ppg -n amazon-cloudwatch
```
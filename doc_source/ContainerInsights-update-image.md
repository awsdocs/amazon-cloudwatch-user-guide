# Updating the CloudWatch Agent Container Image<a name="ContainerInsights-update-image"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights isn't released yet\. It's in preview and is subject to change\. The preview is open to all AWS accounts\. You do not need to request access\. | 

If you need to update the version of the container image, run the following command\.

```
kubectl set image ds/cloudwatch-agent cloudwatch-agent=new-image-container  -n amazon-cloudwatch
```

To achieve rolling updates, you must make sure the `.spec.template` section in the `cwagent-daemonset.yaml` file has changes\. Otherwise, Kubernetes treats the DaemonSet as unchanged\. A common practice is to add the hash value of the ConfigMap into `.spec.template.metadata.annotations.configHash`, as in the following example\.

```
yq w -i cwagent-daemonset.yaml spec.template.metadata.annotations.configHash $(kubectl get cm/cwagentconfig -n amazon-cloudwatch -o yaml | sha256sum)
```

This adds a hash value into the `cwagent-daemonset.yaml` file, as in the following example\.

```
spec:
  selector:
    matchLabels:
      name: cloudwatch-agent
  template:
    metadata:
      labels:
        name: cloudwatch-agent
      annotations:
        configHash: 88915de4cf9c3551a8dc74c0137a3e83569d28c71044b0359c2578d2e0461825
```

Then if you run the following command, the new configuration is picked up\.

```
kubectl apply -f cwagent-daemonset.yaml 
```

For more information about yq, see [yq](https://mikefarah.github.io/yq/)\.
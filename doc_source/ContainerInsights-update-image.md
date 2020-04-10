# Updating the CloudWatch Agent Container Image<a name="ContainerInsights-update-image"></a>

If you need to update your container image to the latest version, use the steps in this section\.

**To update your container image**

1. Apply the latest `cwagent-serviceaccount.yaml` file by entering the following command\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/cwagent/cwagent-serviceaccount.yaml
   ```

1. This step is necessary only for customers who upgraded their containerized CloudWatch agent from a version earlier than 1\.226589\.0, which was released on August 20, 2019\.

   In the Configmap file `cwagentconfig`, change the keyword `structuredlogs` to `logs`

   1. First, open the existing `cwagentconfig` in edit mode by entering the following command\.

      ```
      kubectl edit cm cwagentconfig -n amazon-cloudwatch
      ```

      In the file, if you see the keyword `structuredlogs`, change it to `logs`

   1. Enter **wq** to save the file and exit edit mode\.

1. Apply the latest `cwagent-daemonset.yaml` file by entering the following command\.

   ```
   kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/cwagent/cwagent-daemonset.yaml
   ```

You can achieve rolling updates of the CloudWatch agent DaemonSet anytime that you change your configuration in `cwagent-configmap.yaml`\. To do so, you must make sure the `.spec.template` section in the `cwagent-daemonset.yaml` file has changes\. Otherwise, Kubernetes treats the DaemonSet as unchanged\. A common practice is to add the hash value of the ConfigMap into `.spec.template.metadata.annotations.configHash`, as in the following example\.

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
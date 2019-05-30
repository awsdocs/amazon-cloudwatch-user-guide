# Deleting the CloudWatch Agent<a name="ContainerInsights-delete-agent"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights isn't released yet\. It's in preview and is subject to change\. The preview is open to all AWS accounts\. You do not need to request access\. | 

To delete all resources related to the CloudWatch agent, run the following commands:

```
kubectl delete -f cwagent-daemonset.yaml
```

```
kubectl delete -f cwagent-configmap.yaml
```

```
kubectl delete -f cwagent-serviceaccount.yaml
```

```
kubectl delete cm cwagent-clusterleader -n amazon-cloudwatch
```

```
kubectl delete -f fluentd.yml && \
kubectl delete cm cluster-info -n amazon-cloudwatch
```
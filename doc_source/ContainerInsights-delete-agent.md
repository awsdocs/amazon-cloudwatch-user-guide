# Deleting the CloudWatch Agent<a name="ContainerInsights-delete-agent"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

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
# Enabling segment metrics from X\-Ray<a name="deploy_servicelens_CloudWatch_agent_segments"></a>

The AWS X\-Ray SDK for Java can emit several metrics about segments into CloudWatch to give an unsampled view of latency, throttle, error, and fault rates\. It uses the CloudWatch agent to emit these metrics to minimize the impact on application performance\. For more information about segments, see [Segments](https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html#xray-concepts-segments)\.

If you enable segment metrics, a log group called **XRayApplicationMetrics** is created, and the metrics **ErrorRate**, **FaultRate**, **ThrottleRate**, and **Latency**, are published into a custom CloudWatch metric namespace called **Observability**\.

Segment metrics are not currently supported in Lambda\.

To enable the AWS X\-Ray SDK for Java to publish segment metrics, use the following example\.

```
AWSXRayRecorderBuilder builder = AWSXRayRecorderBuilder.standard().withSegmentListener(new MetricsSegmentListener());
```

If you are using ServiceLens with Amazon EKS and Container Insights, add the `AWS_XRAY_METRICS_DAEMON_ADDRESS` environment variable to the `HOST_IP` as shown in the following example\.

```
env:
- name: HOST_IP
  valueFrom: 
    fieldRef: 
      apiVersion: v1 
      fieldPath: status.hostIP
- name: AWS_XRAY_METRICS_DAEMON_ADDRESS 
  value: $(HOST_IP):25888
```

For more information, see [Enable X\-Ray CloudWatch Metrics](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-java-monitoring.html#xray-sdk-java-monitoring-enable)\.
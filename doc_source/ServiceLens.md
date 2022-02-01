# Using ServiceLens to monitor the health of your applications<a name="ServiceLens"></a>

CloudWatch ServiceLens enhances the observability of your services and applications by enabling you to integrate traces, metrics, logs, alarms, and other resource health information into one place\. ServiceLens integrates CloudWatch with AWS X\-Ray to provide an end\-to\-end view of your application to help you more efficiently pinpoint performance bottlenecks and identify impacted users\. A service map displays your service endpoints and resources as “nodes” and highlights the traffic, latency, and errors for each node and its connections\. You can choose a node to see detailed insights about the correlated metrics, logs, and traces associated with that part of the service\. This enables you to investigate problems and their effect on the application\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/ServiceMap.png)

To fully take advantage of ServiceLens and correlated metrics, logs, and traces, you must update the X\-Ray SDK and the instrumentation of your application\. ServiceLens supports logs correlation for Lambda functions, API Gateway, Java\-based applications running on Amazon EC2, and Java\-based applications running on Amazon EKS or Kubernetes with Container Insights deployed\.

ServiceLens integrates with Amazon CloudWatch Synthetics, a fully\-managed service that enables you to create canaries to monitor your endpoints and APIs from the outside\-in\. Canaries that you create appear on the ServiceLens service map\. For more information, see [Using synthetic monitoring](CloudWatch_Synthetics_Canaries.md)\.

ServiceLens is available in every Region where X\-Ray is available\.

**Topics**
+ [Deploying ServiceLens](deploy_servicelens.md)
+ [Using the service map in ServiceLens](servicelens_service_map.md)
+ [Using the traces view in ServiceLens](servicelens_service_map_traces.md)
+ [Using the resource health view in ServiceLens](servicelens_resource_health.md)
+ [ServiceLens troubleshooting](servicelens_troubleshooting.md)
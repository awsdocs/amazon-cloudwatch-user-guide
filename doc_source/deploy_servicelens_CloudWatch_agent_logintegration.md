# Integrating with CloudWatch Logs<a name="deploy_servicelens_CloudWatch_agent_logintegration"></a>

To enable integration with CloudWatch Logs, there are two steps:
+ Enable trace to logs correlation\. This is supported only using the SDK for Java\.
+ Configure trace ID injection\.

## Enabling trace to logs correlation<a name="deploy_servicelens_CloudWatch_agent_logintegration_correlation"></a>

The SDK for Java supports both a set of standard application logging frameworks and CloudWatch Logs native support\. Before completing the following steps, you must have completed a standard setup of the AWS X\-Ray SDK for Java\. 

The supported runtimes are Amazon EC2, Amazon ECS with CloudWatch Container Insights enabled, and Lambda\. 
+ To enable trace to logs correlation on Amazon EC2, enable the X\-Ray EC2 Plugin\. For more information, see [Service Plugins](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-java-configuration.html#xray-sdk-java-configuration-plugins)
+ To enable trace to logs correlation on Amazon EKS, first enable Container Insights if you have not already done so\. For more information, see [Using Container Insights](ContainerInsights.md)\.

  Then, enable the X\-Ray SDK EKS Plugin\. For more information, see [Service Plugins](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-java-configuration.html#xray-sdk-java-configuration-plugins)\.
+ To enable trace to logs correlation on Lambda, you must enable X\-Ray on Lambda\. For more information, see [AWS Lambda and AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-services-lambda.html)\.

## Enabling trace ID injection<a name="deploy_servicelens_CloudWatch_agent_logintegration_injection"></a>

For information about how to enable trace ID injection, see [Logging](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-java-configuration.html#xray-sdk-java-configuration-logging)\.
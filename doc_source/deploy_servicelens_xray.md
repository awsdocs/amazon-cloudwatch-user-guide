# Deploying AWS X\-Ray<a name="deploy_servicelens_xray"></a>

You can use any AWS X\-Ray SDK to enable X\-Ray\. However, the correlation of logs and metrics with your traces is supported only if you use the Java SDK\.

To deploy X\-Ray, follow the standard X\-Ray setup\. For more information, see the following:
+ [AWS X\-Ray SDK for Java](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-java.html) \(supports logs correlations\)
+ [The X\-Ray SDK for Node\.js](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-nodejs.html)
+ [AWS X\-Ray SDK for \.NET](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-dotnet.html)
+ [AWS X\-Ray SDK for Go](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-go.html)
+ [AWS X\-Ray SDK for Python](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-python.html)
+ [AWS X\-Ray SDK for Ruby](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-ruby.html)

After completing the X\-Ray setup, follow the steps in the following sections to integrate X\-Ray with CloudWatch Logs and enable segment metrics\.

**Topics**
+ [Integrating with CloudWatch Logs](deploy_servicelens_CloudWatch_agent_logintegration.md)
+ [Enabling segment metrics from X\-Ray](deploy_servicelens_CloudWatch_agent_segments.md)
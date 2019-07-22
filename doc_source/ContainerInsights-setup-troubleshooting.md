# Troubleshooting Container Insights Setup<a name="ContainerInsights-setup-troubleshooting"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

The following sections can help if you're having trouble setting up Container Insights\.

## Unauthorized panic: Cannot retrieve cadvisor data from kubelet<a name="ContainerInsights-setup-EKS-troubleshooting-permissions"></a>

If your deployment fails with the error `Unauthorized panic: Cannot retrieve cadvisor data from kubelet`, your kubelet might not have Webhook authorization mode enabled\. This mode is required for Container Insights\. For more information, see [Verify Prerequisites](Container-Insights-prerequisites.md)\.

## Metrics Don't Appear in the Console<a name="ContainerInsights-setup-EKS-troubleshooting-nometrics"></a>

If you don't see any Container Insights metrics in the console, be sure that you have completed the setup of Container Insights\. Metrics don't appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.
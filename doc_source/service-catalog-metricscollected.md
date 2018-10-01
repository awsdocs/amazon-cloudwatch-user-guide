# AWS Service Catalog Metrics and Dimensions<a name="service-catalog-metricscollected"></a>

AWS Service Catalog sends the following metrics to CloudWatch\.

## AWS Service Catalog Metrics<a name="service-catalog-metrics"></a>

The `AWS/ServiceCatalog` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `ProvisionedProductLaunch`  |  The number of provisioned products launched for a given product and provisioning artifact in a specified time period\. Units: Count Valid statistics: Minimum, Maximum, Sum, Average  | 

## Dimensions for AWS Service Catalog Metrics<a name="service-catalog-metrics-dimensions"></a>

AWS Service Catalog sends the following dimensions to CloudWatch\.


| Dimension | Description | 
| --- | --- | 
| `State` |  This dimension filters the data you request for all provisioned products launched with this specified state\. This helps you categorize your data by the state of launch\. Valid State: SUCCEEDED, FAILED  | 
| `ProductId` |  This dimension filters the data you request for the identified product id only\. This helps you to pinpoint an exact product from which to be launched\.  | 
| `ProvisioningArtifactId` |  This dimension filters the data you request for the identified provisioning artifact id only\. This helps you to pinpoint an exact version of products from which to be launched\.  | 
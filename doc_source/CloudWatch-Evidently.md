# Perform launches and A/B experiments with CloudWatch Evidently<a name="CloudWatch-Evidently"></a>

You can use Amazon CloudWatch Evidently to safely validate new features by serving them to a specified percentage of your users while you roll out the feature\. You can monitor the performance of the new feature to help you decide when to ramp up traffic to your users\. This helps you reduce risk and identify unintended consequences before you fully launch the feature\.

You can also conduct A/B experiments to make feature design decisions based on evidence and data\. An experiment can test as many as five variations at once\. Evidently collects experiment data and analyzes it using statistical methods\. It also provides clear recommendations about which variations perform better\. You can test both user\-facing features and backend features\.

**Evidently pricing**

Evidently charges your account based on Evidently events and Evidently analysis units\. Evidently events include both data events such as clicks and page views, and assignment events that determine the feature variation to serve to a user\.

Evidently analysis units are generated from Evidently events, based on rules that you have created in Evidently\. Analysis units are the number of rule matches on events\. For example, a user click event might produce a single Evidently analysis unit, a click count\. Another example is a user checkout event that might produce two Evidently analysis units, checkout value and the number of items in cart\. For more information about pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. 

CloudWatch Evidently is currently available in the following Regions:
+ US East \(Ohio\)
+ US East \(N\. Virginia\)
+ US West \(Oregon\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(Stockholm\)

**Topics**
+ [IAM policies to use Evidently](CloudWatch-Evidently-permissions.md)
+ [Create projects, features, launches, and experiments](CloudWatch-Evidently-projectsfeatures.md)
+ [Manage features, launches, and experiments](CloudWatch-Evidently-manage.md)
+ [Adding code to your application](CloudWatch-Evidently-code-application.md)
+ [Project data storage](CloudWatch-Evidently-datastorage.md)
+ [How Evidently calculates results](CloudWatch-Evidently-calculate-results.md)
+ [View launch results in the dashboard](CloudWatch-Evidently-launch-dashboard.md)
+ [View experiment results in the dashboard](CloudWatch-Evidently-experiment-dashboard.md)
+ [How CloudWatch Evidently collects and stores data](CloudWatch-Evidently-datacollection.md)
+ [Using service\-linked roles for Evidently](Evidently-using-service-linked-roles.md)
+ [CloudWatch Evidently quotas](CloudWatch-Evidently-quotas.md)
+ [Tutorial: A/B testing with the Evidently sample application](CloudWatch-Evidently-sample-application.md)
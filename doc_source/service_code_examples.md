# Code examples for CloudWatch using AWS SDKs<a name="service_code_examples"></a>

The following code examples show how to use CloudWatch with an AWS software development kit \(SDK\)\. 

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

For a complete list of AWS SDK developer guides and code examples, see [Using CloudWatch with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.

**Get started**

## Hello CloudWatch<a name="example_cloudwatch_Hello_section"></a>

The following code examples show how to get started using CloudWatch\.

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/CloudWatch#code-examples)\. 
  

```
using Microsoft.Extensions.Hosting;
using Amazon.CloudWatch;
using Amazon.CloudWatch.Model;
using Microsoft.Extensions.DependencyInjection;

namespace CloudWatchActions;

public static class HelloCloudWatch
{
    static async Task Main(string[] args)
    {
        // Use the AWS .NET Core Setup package to set up dependency injection for the Amazon CloudWatch service.
        // Use your AWS profile name, or leave it blank to use the default profile.
        using var host = Host.CreateDefaultBuilder(args)
            .ConfigureServices((_, services) =>
                services.AddAWSService<IAmazonCloudWatch>()
            ).Build();

        // Now the client is available for injection.
        var cloudWatchClient = host.Services.GetRequiredService<IAmazonCloudWatch>();

        // You can use await and any of the async methods to get a response.
        var metricNamespace = "AWS/Billing";
        var response = await cloudWatchClient.ListMetricsAsync(new ListMetricsRequest
        {
            Namespace = metricNamespace
        });
        Console.WriteLine($"Hello Amazon CloudWatch! Following are some metrics available in the {metricNamespace} namespace:");
        Console.WriteLine();
        foreach (var metric in response.Metrics.Take(5))
        {
            Console.WriteLine($"\tMetric: {metric.MetricName}");
            Console.WriteLine($"\tNamespace: {metric.Namespace}");
            Console.WriteLine($"\tDimensions: {string.Join(", ", metric.Dimensions.Select(m => $"{m.Name}:{m.Value}"))}");
            Console.WriteLine();
        }
    }
}
```
+  For API details, see [ListMetrics](https://docs.aws.amazon.com/goto/DotNetSDKV3/monitoring-2010-08-01/ListMetrics) in *AWS SDK for \.NET API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/cloudwatch#readme)\. 
  

```
/**
 * Before running this Java V2 code example, set up your development environment, including your credentials.
 *
 * For more information, see the following documentation topic:
 *
 * https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/get-started.html
 */
public class HelloService {

    public static void main(String[] args) {

        final String usage = "\n" +
            "Usage:\n" +
            "  <namespace> \n\n" +
            "Where:\n" +
            "  namespace - The namespace to filter against (for example, AWS/EC2). \n" ;

        if (args.length != 1) {
            System.out.println(usage);
            System.exit(1);
        }

        String namespace = args[0];
        Region region = Region.US_EAST_1;
        CloudWatchClient cw = CloudWatchClient.builder()
            .region(region)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();

        listMets(cw, namespace);
        cw.close();
    }


    public static void listMets( CloudWatchClient cw, String namespace) {
        try {
            ListMetricsRequest request = ListMetricsRequest.builder()
                .namespace(namespace)
                .build();

            ListMetricsIterable listRes = cw.listMetricsPaginator(request);
            listRes.stream()
                .flatMap(r -> r.metrics().stream())
                .forEach(metrics -> System.out.println(" Retrieved metric is: " + metrics.metricName()));

        } catch (CloudWatchException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```
+  For API details, see [ListMetrics](https://docs.aws.amazon.com/goto/SdkForJavaV2/monitoring-2010-08-01/ListMetrics) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/cloudwatch#code-examples)\. 
  

```
/**
Before running this Kotlin code example, set up your development environment,
including your credentials.

For more information, see the following documentation topic:
https://docs.aws.amazon.com/sdk-for-kotlin/latest/developer-guide/setup.html
 */
suspend fun main(args: Array<String>) {
    val usage = """
        Usage:
           <namespace> 
        Where:
           namespace - The namespace to filter against (for example, AWS/EC2). 
    """

    if (args.size != 1) {
        println(usage)
        exitProcess(0)
    }

    val namespace = args[0]
    listAllMets(namespace)
}

suspend fun listAllMets(namespaceVal: String?) {
    val request = ListMetricsRequest {
        namespace = namespaceVal
    }

    CloudWatchClient { region = "us-east-1" }.use { cwClient ->
        cwClient.listMetricsPaginated(request)
            .transform { it.metrics?.forEach { obj -> emit(obj) } }
            .collect { obj ->
                println("Name is ${obj.metricName}")
                println("Namespace is ${obj.namespace}")
            }
    }
}
```
+  For API details, see [ListMetrics](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------

**Contents**
+ [Actions](service_code_examples_actions.md)
  + [Create a dashboard](example_cloudwatch_PutDashboard_section.md)
  + [Create a metric alarm](example_cloudwatch_PutMetricAlarm_section.md)
  + [Create an anomaly detector](example_cloudwatch_PutAnomalyDetector_section.md)
  + [Delete alarms](example_cloudwatch_DeleteAlarms_section.md)
  + [Delete an anomaly detector](example_cloudwatch_DeleteAnomalyDetector_section.md)
  + [Delete dashboards](example_cloudwatch_DeleteDashboards_section.md)
  + [Describe alarm history](example_cloudwatch_DescribeAlarmHistory_section.md)
  + [Describe alarms](example_cloudwatch_DescribeAlarms_section.md)
  + [Describe alarms for a metric](example_cloudwatch_DescribeAlarmsForMetric_section.md)
  + [Describe anomaly detectors](example_cloudwatch_DescribeAnomalyDetectors_section.md)
  + [Disable alarm actions](example_cloudwatch_DisableAlarmActions_section.md)
  + [Enable alarm actions](example_cloudwatch_EnableAlarmActions_section.md)
  + [Get a metric data image](example_cloudwatch_GetMetricImage_section.md)
  + [Get dashboard details](example_cloudwatch_GetDashboard_section.md)
  + [Get metric data](example_cloudwatch_GetMetricData_section.md)
  + [Get metric statistics](example_cloudwatch_GetMetricStatistics_section.md)
  + [List dashboards](example_cloudwatch_ListDashboards_section.md)
  + [List metrics](example_cloudwatch_ListMetrics_section.md)
  + [Put a set of data into a metric](example_cloudwatch_PutMetricData_DataSet_section.md)
  + [Put data into a metric](example_cloudwatch_PutMetricData_section.md)
+ [Scenarios](service_code_examples_scenarios.md)
  + [Get started with alarms](example_cloudwatch_Scenario_GettingStarted_section.md)
  + [Get started with metrics, dashboards, and alarms](example_cloudwatch_GetStartedMetricsDashboardsAlarms_section.md)
  + [Manage metrics and alarms](example_cloudwatch_Usage_MetricsAlarms_section.md)
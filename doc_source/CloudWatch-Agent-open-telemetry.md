# Use the AWS OpenTelemetry Collector with the CloudWatch agent<a name="CloudWatch-Agent-open-telemetry"></a>

The AWS OpenTelemetry Collector complements the CloudWatch agent when you install them side\-by\-side, and then use the OpenTelemetry SDKs to collect application traces in addition to metrics and logs from your workloads running on Amazon EC2, Amazon ECS, and Amazon EKS\. For general information about AWS OpenTelemetry, see [AWS Distro for OpenTelemetry](https://aws.amazon.com/otel/)\.

For information about installing the AWS OpenTelemetry Collector, see [AWS Distro for OpenTelemetry](https://aws-otel.github.io/) on GitHub\.

Beginning with CloudWatch agent version 1\.247356, the OpenTelemetry Collector is not embedded in the CloudWatch agent\.

## Generating OpenTelemetry traces<a name="CloudWatch-Agent-open-telemetry-generate"></a>

AWS provides OpenTelemetry SDKs and a Java auto instrumentation agent for your applications to use to generate OpenTelemetry traces and feed them to an OpenTelemetry Collector\. For more information, see the following:
+ [ Getting Started with the Java SDK on Traces and Metrics Instrumentation](https://aws-otel.github.io/docs/getting-started/java-sdk)
+ [ Tracing with the AWS Distro for OpenTelemetry Go SDK](https://aws-otel.github.io/docs/getting-started/go-sdk)
+ [ Getting Started with the Python SDK](https://aws-otel.github.io/docs/getting-started/python-sdk)
+ [ Getting Started with the JavaScript SDK on Traces and Metrics Instrumentation](https://aws-otel.github.io/docs/getting-started/javascript-sdk)
# OpenTelemetry 0\.7\.0 format<a name="CloudWatch-metric-streams-formats-opentelemetry"></a>

OpenTelemetry is a collection of tools, APIs, and SDKs\. You can use it to instrument, generate, collect, and export telemetry data \(metrics, logs, and traces\) for analysis\. OpenTelemetry is part of the Cloud Native Computing Foundation\. For more information, see [OpenTelemetry](https://opentelemetry.io/)\.

For information about the full OpenTelemetry 0\.7\.0 specification, see [v0\.7\.0 release](https://github.com/open-telemetry/opentelemetry-proto/releases/tag/v0.7.0)\.

**Note**  
While metric streams is in general availability, the OpenTelemetry format 0\.7\.0 is not yet generally available\. Metric streams will continue to offer OpenTelemetry format 0\.7\.0 even when the OpenTelemetry format version 1\.0 becomes available\.

A Kinesis record can contain one or more `ExportMetricsServiceRequest` OpenTelemetry data structures\. Each data structure starts with a header with an `UnsignedVarInt32` indicating the record length in bytes\. Each `ExportMetricsServiceRequest` may contain data from multiple metrics at once\.

The following is a string representation of the message of the `ExportMetricsServiceRequest` OpenTelemetry data structure\. OpenTelemetry serializes the use of the Google Protocol Buffers binary protocol, which is not human\-readable\.

```
resource_metrics {
  resource {
    attributes {
      key: "cloud.provider"
      value {
        string_value: "aws"
      }
    }
    attributes {
      key: "cloud.account.id"
      value {
        string_value: "2345678901"
      }
    }
    attributes {
      key: "cloud.region"
      value {
        string_value: "us-east-1"
      }
    }
    attributes {
      key: "aws.exporter.arn"
      value {
        string_value: "arn:aws:cloudwatch:us-east-1:123456789012:metric-stream/MyMetricStream"
      }
    }
  }
  instrumentation_library_metrics {
    metrics {
      name: "amazonaws.com/AWS/DynamoDB/ConsumedReadCapacityUnits"
      unit: "1"
      double_summary {
        data_points {
          labels {
            key: "Namespace"
            value: "AWS/DynamoDB"
          }
          labels {
            key: "MetricName"
            value: "ConsumedReadCapacityUnits"
          }
          labels {
            key: "TableName"
            value: "MyTable"
          }
          start_time_unix_nano: 1604948400000000000
          time_unix_nano: 1604948460000000000
          count: 1
          sum: 1.0
          quantile_values {
            quantile: 0.0
            value: 1.0
          }
          quantile_values {
            quantile: 1.0
            value: 1.0
          }
        }
        data_points {
          labels {
            key: "Namespace"
            value: "AWS/DynamoDB"
          }
          labels {
            key: "MetricName"
            value: "ConsumedReadCapacityUnits"
          }
          labels {
            key: "TableName"
            value: "MyTable"
          }
          start_time_unix_nano: 1604948460000000000
          time_unix_nano: 1604948520000000000
          count: 2
          sum: 5.0
          quantile_values {
            quantile: 0.0
            value: 2.0
          }
          quantile_values {
            quantile: 1.0
            value: 3.0
          }
        }
      }
    }
  }
}
```

**Top\-level object to serialize OpenTelemetry metric data**

`ExportMetricsServiceRequest` is the top\-level wrapper to serialize an OpenTelemetry exporter payload\. It contains one or more `ResourceMetrics`\.

```
message ExportMetricsServiceRequest {
  // An array of ResourceMetrics.
  // For data coming from a single resource this array will typically contain one
  // element. Intermediary nodes (such as OpenTelemetry Collector) that receive
  // data from multiple origins typically batch the data before forwarding further and
  // in that case this array will contain multiple elements.
  repeated opentelemetry.proto.metrics.v1.ResourceMetrics resource_metrics = 1;
}
```

`ResourceMetrics` is the top\-level object to represent MetricData objects\. 

```
// A collection of InstrumentationLibraryMetrics from a Resource.
message ResourceMetrics {
  // The resource for the metrics in this message.
  // If this field is not set then no resource info is known.
  opentelemetry.proto.resource.v1.Resource resource = 1;
  
  // A list of metrics that originate from a resource.
  repeated InstrumentationLibraryMetrics instrumentation_library_metrics = 2;
}
```

**The Resource object**

A `Resource` object is a value\-pair object that contains some information about the resource that generated the metrics\. For metrics created by AWS, the data structure contains the Amazon Resource Name \(ARN\) of the resource related to the metric, such as an EC2 instance or an S3 bucket\.

The `Resource` object contains an attribute called `attributes`, which store a list of key\-value pairs\.
+ `cloud.account.id` contains the account ID
+ `cloud.region` contains the Region
+ `aws.exporter.arn` contains the metric stream ARN
+ `cloud.provider` is always `aws`\.

```
// Resource information.
message Resource {
  // Set of labels that describe the resource.
  repeated opentelemetry.proto.common.v1.KeyValue attributes = 1;
  
  // dropped_attributes_count is the number of dropped attributes. If the value is 0,
  // no attributes were dropped.
  uint32 dropped_attributes_count = 2;
}
```

**The InstrumentationLibraryMetrics object**

The instrumentation\_library field will not be filled\. We will fill only the metrics field that we are exporting\.

```
// A collection of Metrics produced by an InstrumentationLibrary.
message InstrumentationLibraryMetrics {
  // The instrumentation library information for the metrics in this message.
  // If this field is not set then no library info is known.
  opentelemetry.proto.common.v1.InstrumentationLibrary instrumentation_library = 1;
  // A list of metrics that originate from an instrumentation library.
  repeated Metric metrics = 2;
}
```

**The Metric object**

The metric object contains a `DoubleSummary` data field that contains a list of `DoubleSummaryDataPoint`\.

```
message Metric {
  // name of the metric, including its DNS name prefix. It must be unique.
  string name = 1;

  // description of the metric, which can be used in documentation.
  string description = 2;

  // unit in which the metric value is reported. Follows the format
  // described by http://unitsofmeasure.org/ucum.html.
  string unit = 3;

  oneof data {
    IntGauge int_gauge = 4;
    DoubleGauge double_gauge = 5;
    IntSum int_sum = 6;
    DoubleSum double_sum = 7;
    IntHistogram int_histogram = 8;
    DoubleHistogram double_histogram = 9;
    DoubleSummary double_summary = 11;
  }
}

message DoubleSummary {
  repeated DoubleSummaryDataPoint data_points = 1;
}
```

**The MetricDescriptor object**

The MetricDescriptor object contains metadata\. For more information, see [metrics\.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/main/opentelemetry/proto/metrics/v1/metrics.proto#L110) on GitHub\.

For metric streams, the MetricDescriptor has the following contents:
+ `name` will be `amazonaws.com/metric_namespace/metric_name`
+ `description` will be blank\.
+ `unit` will be filled by mapping the metric datum's unit to the case\-sensitive variant of the Unified code for Units of Measure\. For more information, see [ Translations with OpenTelemetry 0\.7\.0 format](CloudWatch-metric-streams-formats-opentelemetry-translation.md) and [The Unified Code For Units of Measure](https://ucum.org/ucum.html)\.
+ `type` will be `SUMMARY`\.

**The DoubleSummaryDataPoint object**

The DoubleSummaryDataPoint object contains the value of a single data point in a time series in a DoubleSummary metric\.

```
// DoubleSummaryDataPoint is a single data point in a timeseries that describes the
// time-varying values of a Summary metric.
message DoubleSummaryDataPoint {
  // The set of labels that uniquely identify this timeseries.
  repeated opentelemetry.proto.common.v1.StringKeyValue labels = 1;

  // start_time_unix_nano is the last time when the aggregation value was reset
  // to "zero". For some metric types this is ignored, see data types for more
  // details.
  //
  // The aggregation value is over the time interval (start_time_unix_nano,
  // time_unix_nano].
  //
  // Value is UNIX Epoch time in nanoseconds since 00:00:00 UTC on 1 January
  // 1970.
  //
  // Value of 0 indicates that the timestamp is unspecified. In that case the
  // timestamp may be decided by the backend.
  fixed64 start_time_unix_nano = 2;

  // time_unix_nano is the moment when this aggregation value was reported.
  //
  // Value is UNIX Epoch time in nanoseconds since 00:00:00 UTC on 1 January
  // 1970.
  fixed64 time_unix_nano = 3;

  // count is the number of values in the population. Must be non-negative.
  fixed64 count = 4;

  // sum of the values in the population. If count is zero then this field
  // must be zero.
  double sum = 5;

  // Represents the value at a given quantile of a distribution.
  //
  // To record Min and Max values following conventions are used:
  // - The 1.0 quantile is equivalent to the maximum value observed.
  // - The 0.0 quantile is equivalent to the minimum value observed.
  message ValueAtQuantile {
    // The quantile of a distribution. Must be in the interval
    // [0.0, 1.0].
    double quantile = 1;

    // The value at the given quantile of a distribution.
    double value = 2;
  }

  // (Optional) list of values at different quantiles of the distribution calculated
  // from the current snapshot. The quantiles must be strictly increasing.
  repeated ValueAtQuantile quantile_values = 6;
}
```

For more information, see [ Translations with OpenTelemetry 0\.7\.0 format](CloudWatch-metric-streams-formats-opentelemetry-translation.md)\.
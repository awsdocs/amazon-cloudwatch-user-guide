# Specification: Embedded metric format<a name="CloudWatch_Embedded_Metric_Format_Specification"></a>

The CloudWatch embedded metric format is a JSON specification used to instruct CloudWatch Logs to automatically extract metric values embedded in structured log events\. You can use CloudWatch to graph and create alarms on the extracted metric values\.

## Embedded metric format specification conventions<a name="CloudWatch_Embedded_Metric_Format_Specification_Conventions"></a>

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this format specification are to be interpreted as described in [Key Words RFC2119](http://tools.ietf.org/html/rfc2119)\.

The terms "JSON", "JSON text", "JSON value", "member", "element", "object", "array", "number", "string", "boolean", "true", "false", and "null" in this format specification are to be interpreted as defined in [JavaScript Object Notation RFC8259](https://tools.ietf.org/html/rfc8259)\.

## Embedded metric format specification PutLogEvents request format<a name="CloudWatch_Embedded_Metric_Format_Specification_PutLogEvents"></a>

Clients DO NOT NEED to use the following log format header anymore when sending an embedded metric format document using the CloudWatch Logs [PutLogEvents](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutLogEvents.html) API:

```
x-amzn-logs-format: json/emf
```

On Lambda, you do not need to set this header yourself\. Writing JSON to standard out in the embedded metric format is sufficient\. While Lambda may prepend log events with metadata such as timestamp and request id, a valid embedded metric format document after this metadata is considered valid\.

**Note**  
If you plan to create alarms on metrics created using embedded metric format, see [Setting alarms on metrics created with embedded metric format](CloudWatch_Embedded_Metric_Format_Alarms.md) for recommendations\.

## Embedded metric format document structure<a name="CloudWatch_Embedded_Metric_Format_Specification_structure"></a>

This section describes the structure of an embedded metric format document\. Embedded metric format documents are defined in [JavaScript Object Notation RFC8259](https://tools.ietf.org/html/rfc8259)\.

Unless otherwise noted, objects defined by this specification MUST NOT contain any additional members\. Members not recognized by this specification MUST be ignored\. Members defined in this specification are case\-sensitive\.

The embedded metric format is subject to the same limits as standard CloudWatch Logs events and are limited to a maximum size of 256 KB\.

 With the embedded metric format, you can track the processing of your EMF logs by metrics that are published in the `AWS/Logs` namespace of your account\. These can be used to track failed metric generation from EMF, as well as whether failures happen due to parsing or validation\. For more details see [Monitoring with CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatch-Logs-Monitoring-CloudWatch-Metrics.html)\. 

### Root node<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_root"></a>

The LogEvent message MUST be a valid JSON object with no additional data at the beginning or end of the LogEvent message string\. For more information about the LogEvent structure, see [InputLogEvent](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_InputLogEvent.html)\. 

Embedded metric format documents MUST contain the following top\-level member on the root node\. This is a [Metadata object](#CloudWatch_Embedded_Metric_Format_Specification_structure_metadata) object\. 

```
{
 "_aws": {
    "CloudWatchMetrics": [ ... ]
  }
}
```

The root node MUST contain all [Target members](#CloudWatch_Embedded_Metric_Format_Specification_structure_target) members defined by the references in the [MetricDirective object](#CloudWatch_Embedded_Metric_Format_Specification_structure_metricdirective)\.

The root node MAY contain any other members that are not included in the above requirements\. The values of these members MUST be valid JSON types\.

### Metadata object<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_metadata"></a>

The `_aws` member can be used to represent metadata about the payload that informs downstream services how they should process the LogEvent\. The value MUST be an object and MUST contain the following members: 
+ **CloudWatchMetrics**— An array of [MetricDirective object](#CloudWatch_Embedded_Metric_Format_Specification_structure_metricdirective) used to instruct CloudWatch to extract metrics from the root node of the LogEvent\.

  ```
  {
    "_aws": {
      "CloudWatchMetrics": [ ... ]
    }
  }
  ```
+ **Timestamp**— A number representing the time stamp used for metrics extracted from the event\. Values MUST be expressed as the number of milliseconds after Jan 1, 1970 00:00:00 UTC\.

  ```
  {
    "_aws": {
      "Timestamp": 1559748430481
    }
  }
  ```

### MetricDirective object<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_metricdirective"></a>

The MetricDirective object instructs downstream services that the LogEvent contains metrics that will be extracted and published to CloudWatch\. MetricDirectives MUST contain the following members:
+ **Namespace**— A string representing the CloudWatch namespace for the metric\.
+ **Dimensions**— A [DimensionSet array](#CloudWatch_Embedded_Metric_Format_Specification_structure_dimensionset)\.
+ **Metrics**— An array of [MetricDefinition object](#CloudWatch_Embedded_Metric_Format_Specification_structure_metricdefinition) objects\. This array MUST NOT contain more than 100 MetricDefinition objects\.

### DimensionSet array<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_dimensionset"></a>

A DimensionSet is an array of strings containing the dimension keys that will be applied to all metrics in the document\. The values within this array MUST also be members on the root\-node—referred to as the [Target members](#CloudWatch_Embedded_Metric_Format_Specification_structure_target)

A DimensionSet MUST NOT contain more than 30 dimension keys\. A DimensionSet MAY be empty\.

The target member MUST have a string value\. This value MUST NOT contain more than 1024 characters\. The target member defines a dimension that will be published as part of the metric identity\. Every DimensionSet used creates a new metric in CloudWatch\. For more information about dimensions, see [Dimension](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_Dimension.html) and [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension)\.

```
{
 "_aws": {
   "CloudWatchMetrics": [
     {
       "Dimensions": [ [ "functionVersion" ] ],
       ...
     }
   ]
 },
 "functionVersion": "$LATEST"
}
```

**Note**  
Be careful when configuring your metric extraction as it impacts your custom metric usage and corresponding bill\. If you unintentionally create metrics based on high\-cardinality dimensions \(such as `requestId`\), the embedded metric format will by design create a custom metric corresponding to each unique dimension combination\. For more information, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension)\.

### MetricDefinition object<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_metricdefinition"></a>

A MetricDefinition is an object that MUST contain the following member:
+ **Name**— A string [Reference values](#CloudWatch_Embedded_Metric_Format_Specification_structure_referencevalues) to a metric [Target members](#CloudWatch_Embedded_Metric_Format_Specification_structure_target)\. Metric targets MUST be either a numeric value or an array of numeric values\.

A MetricDefinition object MAY contain the following members:
+ **Unit**— An OPTIONAL string value representing the unit of measure for the corresponding metric\. Values SHOULD be valid CloudWatch metric units\. For information about valid units, see [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\. If a value is not provided, then a default value of NONE is assumed\.
+ **StorageResolution**— An OPTIONAL integer value representing the storage resolution for the corresponding metric\. Setting this to 1 specifies this metric as a high\-resolution metric, so that CloudWatch stores the metric with sub\-minute resolution down to one second\. Setting this to 60 specifies this metric as standard\-resolution, which CloudWatch stores at 1\-minute resolution\. Values SHOULD be valid CloudWatch supported resolutions, 1 or 60\. If a value is not provided, then a default value of 60 is assumed\.

  For more information about high\-resolution metrics, see [High\-resolution metrics](publishingMetrics.md#high-resolution-metrics)\.

**Note**  
If you plan to create alarms on metrics created using embedded metric format, see [Setting alarms on metrics created with embedded metric format](CloudWatch_Embedded_Metric_Format_Alarms.md) for recommendations\.

```
{
  "_aws": {
    "CloudWatchMetrics": [
      {
        "Metrics": [
          {
            "Name": "Time",
            "Unit": "Milliseconds",
            "StorageResolution": 60
          }
        ],
        ...
      }
    ]
  },
  "Time": 1
}
```

### Reference values<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_referencevalues"></a>

Reference values are string values that reference [Target members](#CloudWatch_Embedded_Metric_Format_Specification_structure_target) members on the root node\. These references should NOT be confused with the JSON Pointers described in [RFC6901](https://tools.ietf.org/html/rfc6901)\. Target values cannot be nested\.

### Target members<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_target"></a>

Valid targets MUST be members on the root node and cannot be nested objects\. For example, a \_reference\_ value of `"A.a"` MUST match the following member:

```
{ "A.a" }
```

It MUST NOT match the nested member:

```
{ "A": { "a" } }
```

Valid values of target members depend on what is referencing them\. A metric target MUST be a numeric value or an array of numeric values\. Numeric array metric targets MUST NOT have more than 100 members\. A dimension target MUST have a string value\.

### Embedded metric format example and JSON schema<a name="CloudWatch_Embedded_Metric_Format_Specification_structure_example"></a>

The following is a valid example of embedded metric format\.

```
{
  "_aws": {
    "Timestamp": 1574109732004,
    "CloudWatchMetrics": [
      {
        "Namespace": "lambda-function-metrics",
        "Dimensions": [["functionVersion"]],
        "Metrics": [
          {
            "Name": "time",
            "Unit": "Milliseconds",
            "StorageResolution": 60
          }
        ]
      }
    ]
  },
  "functionVersion": "$LATEST",
  "time": 100,
  "requestId": "989ffbf8-9ace-4817-a57c-e4dd734019ee"
}
```

You can use the following schema to validate embedded metric format documents\.

```
{
    "type": "object",
    "title": "Root Node",
    "required": [
        "_aws"
    ],
    "properties": {
        "_aws": {
            "$id": "#/properties/_aws",
            "type": "object",
            "title": "Metadata",
            "required": [
                "Timestamp",
                "CloudWatchMetrics"
            ],
            "properties": {
                "Timestamp": {
                    "$id": "#/properties/_aws/properties/Timestamp",
                    "type": "integer",
                    "title": "The Timestamp Schema",
                    "examples": [
                        1565375354953
                    ]
                },
                "CloudWatchMetrics": {
                    "$id": "#/properties/_aws/properties/CloudWatchMetrics",
                    "type": "array",
                    "title": "MetricDirectives",
                    "items": {
                        "$id": "#/properties/_aws/properties/CloudWatchMetrics/items",
                        "type": "object",
                        "title": "MetricDirective",
                        "required": [
                            "Namespace",
                            "Dimensions",
                            "Metrics"
                        ],
                        "properties": {
                            "Namespace": {
                                "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Namespace",
                                "type": "string",
                                "title": "CloudWatch Metrics Namespace",
                                "examples": [
                                    "MyApp"
                                ],
                                "pattern": "^(.*)$",
                                "minLength": 1,
                                "maxLength": 1024
                            },
                            "Dimensions": {
                                "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Dimensions",
                                "type": "array",
                                "title": "The Dimensions Schema",
                                "minItems": 1,
                                "items": {
                                    "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Dimensions/items",
                                    "type": "array",
                                    "title": "DimensionSet",
                                    "minItems": 0,
                                    "maxItems": 30,
                                    "items": {
                                        "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Dimensions/items/items",
                                        "type": "string",
                                        "title": "DimensionReference",
                                        "examples": [
                                            "Operation"
                                        ],
                                        "pattern": "^(.*)$",
                                        "minLength": 1,
                                        "maxLength": 250
}
                                }
                            },
                            "Metrics": {
                                "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Metrics",
                                "type": "array",
                                "title": "MetricDefinitions",
                                "items": {
                                    "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Metrics/items",
                                    "type": "object",
                                    "title": "MetricDefinition",
                                    "required": [
                                        "Name"
                                    ],
                                    "properties": {
                                        "Name": {
                                            "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Metrics/items/properties/Name",
                                            "type": "string",
                                            "title": "MetricName",
                                            "examples": [
                                                "ProcessingLatency"
                                            ],
                                            "pattern": "^(.*)$",
                                            "minLength": 1,
                                            "maxLength": 1024
                                        },
                                        "Unit": {
                                            "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Metrics/items/properties/Unit",
                                            "type": "string",
                                            "title": "MetricUnit",
                                            "examples": [
                                                "Milliseconds"
                                            ],
                                            "pattern": "^(Seconds|Microseconds|Milliseconds|Bytes|Kilobytes|Megabytes|Gigabytes|Terabytes|Bits|Kilobits|Megabits|Gigabits|Terabits|Percent|Count|Bytes\\/Second|Kilobytes\\/Second|Megabytes\\/Second|Gigabytes\\/Second|Terabytes\\/Second|Bits\\/Second|Kilobits\\/Second|Megabits\\/Second|Gigabits\\/Second|Terabits\\/Second|Count\\/Second|None)$"
                                         },
                                         "StorageResolution": {
                                            "$id": "#/properties/_aws/properties/CloudWatchMetrics/items/properties/Metrics/items/properties/StorageResolution",
                                            "type": "integer",
                                            "title": "StorageResolution",
                                            "examples": [
                                                60
                                            ]
                                         }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
```
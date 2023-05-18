# JSON format<a name="CloudWatch-metric-streams-formats-json"></a>

In a CloudWatch metric stream that uses the JSON format, each Kinesis Data Firehose record contains multiple JSON objects separated by a newline character \(\\n\)\. Each object includes a single data point of a single metric\.

The JSON format that is used is fully compatible with AWS Glue and with Amazon Athena\. If you have a Kinesis Data Firehose delivery stream and an AWS Glue table formatted correctly, the format can be automatically transformed into Parquet format or Optimized Row Columnar \(ORC\) format before being stored in S3\. For more information about transforming the format, see [ Converting Your Input Record Format in Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)\. For more information about the correct format for AWS Glue, see [Which AWS Glue schema should I use for JSON output format?](#CloudWatch-metric-streams-format-glue)\.

In the JSON format, the valid values for `unit` are the same as for the value of `unit` in the `MetricDatum` API structure\. For more information, see [ MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\. The value for the `timestamp` field is in epoch milliseconds, such as `1616004674229`\.

The following is an example of the format\. In this example, the JSON is formatted for easy reading, but in practice the whole format is on a single line\.

```
{
    "metric_stream_name": "MyMetricStream",
    "account_id": "1234567890",
    "region": "us-east-1",
    "namespace": "AWS/EC2",
    "metric_name": "DiskWriteOps",
    "dimensions": {
        "InstanceId": "i-123456789012"
    },
    "timestamp": 1611929698000,
    "value": {
        "count": 3.0,
        "sum": 20.0,
        "max": 18.0,
        "min": 0.0,
        "p99": 17.56,
        "p99.9": 17.8764,
        "TM(25%:75%)": 16.43
    },
    "unit": "Seconds"
}
```

## Which AWS Glue schema should I use for JSON output format?<a name="CloudWatch-metric-streams-format-glue"></a>

The following is an example of a JSON representation of the `StorageDescriptor` for an AWS Glue table, which would then be used by Kinesis Data Firehose\. For more information about `StorageDescriptor`, see [ StorageDescriptor](https://docs.aws.amazon.com/glue/latest/webapi/API_StorageDescriptor.html)\.

```
{
  "Columns": [
    {
      "Name": "metric_stream_name",
      "Type": "string"
    },
    {
      "Name": "account_id",
      "Type": "string"
    },
    {
      "Name": "region",
      "Type": "string"
    },
    {
      "Name": "namespace",
      "Type": "string"
    },
    {
      "Name": "metric_name",
      "Type": "string"
    },
    {
      "Name": "timestamp",
      "Type": "timestamp"
    },
    {
      "Name": "dimensions",
      "Type": "map<string,string>"
    },
    {
      "Name": "value",
      "Type": "struct<min:double,max:double,count:double,sum:double,p99:double,p99.9:double>"
    },
    {
      "Name": "unit",
      "Type": "string"
    }
  ],
  "Location": "s3://my-s3-bucket/",
  "InputFormat": "org.apache.hadoop.mapred.TextInputFormat",
  "OutputFormat": "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat",
  "SerdeInfo": {
    "SerializationLibrary": "org.apache.hive.hcatalog.data.JsonSerDe"
  },
  "Parameters": {
    "classification": "json"
  }
}
```

The preceding example is for data written on Amazon S3 in JSON format\. Replace the values in the following fields with the indicated values to store the data in Parquet format or Optimized Row Columnar \(ORC\) format\.
+ **Parquet:**
  + inputFormat: org\.apache\.hadoop\.hive\.ql\.io\.parquet\.MapredParquetInputFormat
  + outputFormat: org\.apache\.hadoop\.hive\.ql\.io\.parquet\.MapredParquetOutputFormat
  + SerDeInfo\.serializationLib: org\.apache\.hadoop\.hive\.ql\.io\.parquet\.serde\.ParquetHiveSerDe
  + parameters\.classification: parquet
+ **ORC:**
  + inputFormat: org\.apache\.hadoop\.hive\.ql\.io\.orc\.OrcInputFormat
  + outputFormat: org\.apache\.hadoop\.hive\.ql\.io\.orc\.OrcOutputFormat
  + SerDeInfo\.serializationLib: org\.apache\.hadoop\.hive\.ql\.io\.orc\.OrcSerde
  + parameters\.classification: orc
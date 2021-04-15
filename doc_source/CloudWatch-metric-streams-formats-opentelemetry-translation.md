# Translations with OpenTelemetry 0\.7\.0 format<a name="CloudWatch-metric-streams-formats-opentelemetry-translation"></a>

CloudWatch performs some transformations to put CloudWatch data into OpenTelemetry format\.

**Translating namespace, metric name, and dimensions**

These attributes are key\-value pairs encoded in the mapping\.
+ One pair contains the namespace of the metric
+ One pair contains the name of the metric
+ For each dimension, CloudWatch stores the following pair: `metricDatum.Dimensions[i].Name, metricDatum.Dimensions[i].Value`

**Translating Average, Sum, SampleCount, Min and Max**

The Summary datapoint enables CloudWatch to export all of these statistics using one datapoint\.
+ `startTimeUnixNano` contains the CloudWatch `startTime`
+ `timeUnixNano` contains the CloudWatch `endTime`
+ `sum` contains the Sum statistic\.
+ `count` contains the SampleCount statistic\.
+ `quantile_values` contains two `valueAtQuantile.value` objects:
  + `valueAtQuantile.quantile = 0.0` with `valueAtQuantile.value = Min value`
  + `valueAtQuantile.quantile = 1.0` with `valueAtQuantile.value = Max value`

Resources that consume the metric stream can calculate the Average statistic as **Sum/SampleCount**\.

**Translating units**

CloudWatch units are mapped to the case\-sensitive variant of the Unified code for Units of Measure, as shown in the following table\. For more information, see [The Unified Code For Units of Measure](https://ucum.org/ucum.html)\.


| CloudWatch | OpenTelemetry | 
| --- | --- | 
|  Second |  s | 
|  Second or Seconds |  s | 
|  Microsecond |  us | 
|  Milliseconds |  ms | 
|  Bytes |  By | 
|  Kilobytes |  kBy | 
|  Megabytes |  MBy | 
|  Gigabytes |  GBy | 
|  Terabytes |  TBy | 
|  Bits |  bit | 
|  Kilobits |  kbit | 
|  Megabits |  MBit | 
|  Gigabits |  GBit | 
|  Terabits |  Tbit | 
|  Percent |  % | 
|  Count |  \{Count\} | 
|  None |  1 | 

Units that are combined with a slash are mapped by applying the OpenTelemetry conversion of both the units\. For example, Bytes/Second is mapped to By/s\.
# How to parse OpenTelemetry 0\.7\.0 messages<a name="CloudWatch-metric-streams-formats-opentelemetry-parse"></a>

This section provides information to help you get started with parsing OpenTelemetry 0\.7\.0\.

First, you should get language\-specific bindings, which enable you to parse OpenTelemetry 0\.7\.0 messages in your preferred language\.

**To get language\-specific bindings**
+ The steps depend on your preferred language\.
  + To use Java, add the following Maven dependency to your Java project: [OpenTelemetry Java >> 0\.14\.1](https://mvnrepository.com/artifact/io.opentelemetry/opentelemetry-proto/0.14.1)\.
  + To use any other language, follow these steps:

    1. Make sure that your language is supported by checking the list at [Generating Your Classes](https://developers.google.com/protocol-buffers/docs/proto3#generating)\.

    1. Install the Protobuf compiler by following the steps at [Download Protocol Buffers](https://developers.google.com/protocol-buffers/docs/downloads)\.

    1. Download the OpenTelemetry 0\.7\.0 ProtoBuf definitions at [v0\.7\.0 release](https://github.com/open-telemetry/opentelemetry-proto/releases/tag/v0.7.0)\. 

    1. Confirm that you are in the root folder of the downloaded OpenTelemetry 0\.7\.0 ProtoBuf definitions\. Then create a `src` folder and then run the command to generate language\-specific bindings\. For more information, see [Generating Your Classes](https://developers.google.com/protocol-buffers/docs/proto3#generating)\. 

       The following is an example for how to generate Javascript bindings\.

       ```
       protoc --proto_path=./ --js_out=import_style=commonjs,binary:src \
       opentelemetry/proto/common/v1/common.proto \
       opentelemetry/proto/resource/v1/resource.proto \
       opentelemetry/proto/metrics/v1/metrics.proto \
       opentelemetry/proto/collector/metrics/v1/metrics_service.proto
       ```

The following section includes examples of using the language\-specific bindings that you can build using the previous instructions\.

**Java**

```
package com.example;

import io.opentelemetry.proto.collector.metrics.v1.ExportMetricsServiceRequest;

import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

public class MyOpenTelemetryParser {

    public List<ExportMetricsServiceRequest> parse(InputStream inputStream) throws IOException {
        List<ExportMetricsServiceRequest> result = new ArrayList<>();

        ExportMetricsServiceRequest request;
        /* A Kinesis record can contain multiple `ExportMetricsServiceRequest`
           records, each of them starting with a header with an
           UnsignedVarInt32 indicating the record length in bytes:
            ------ --------------------------- ------ -----------------------
           |UINT32|ExportMetricsServiceRequest|UINT32|ExportMetricsService...
            ------ --------------------------- ------ -----------------------
         */
        while ((request = ExportMetricsServiceRequest.parseDelimitedFrom(inputStream)) != null) {
            // Do whatever we want with the parsed message
            result.add(request);
        }

        return result;
    }
}
```

**Javascript**

This example assumes that the root folder with the bindings generated is `./`

The data argument of the function `parseRecord` can be one of the following types:
+ `Uint8Array` this is optimal
+ `Buffer` optimal under node
+ `Array.number` 8\-bit integers

```
const pb = require('google-protobuf')
const pbMetrics =
    require('./opentelemetry/proto/collector/metrics/v1/metrics_service_pb')

function parseRecord(data) {
    const result = []

    // Loop until we've read all the data from the buffer
    while (data.length) {
        /* A Kinesis record can contain multiple `ExportMetricsServiceRequest`
           records, each of them starting with a header with an
           UnsignedVarInt32 indicating the record length in bytes:
            ------ --------------------------- ------ -----------------------
           |UINT32|ExportMetricsServiceRequest|UINT32|ExportMetricsService...
            ------ --------------------------- ------ -----------------------
         */
        const reader = new pb.BinaryReader(data)
        const messageLength = reader.decoder_.readUnsignedVarint32()
        const messageFrom = reader.decoder_.cursor_
        const messageTo = messageFrom + messageLength

        // Extract the current `ExportMetricsServiceRequest` message to parse
        const message = data.subarray(messageFrom, messageTo)

        // Parse the current message using the ProtoBuf library
        const parsed =
            pbMetrics.ExportMetricsServiceRequest.deserializeBinary(message)

        // Do whatever we want with the parsed message
        result.push(parsed.toObject())

        // Shrink the remaining buffer, removing the already parsed data
        data = data.subarray(messageTo)
    }

    return result
}
```

**Python**

You must read the `var-int` delimiters yourself or use the internal methods `_VarintBytes(size)` and `_DecodeVarint32(buffer, position)`\. These return the position in the buffer just after the size bytes\. The read\-side constructs a new buffer that is limited to reading only the bytes of the message\. 

```
size = my_metric.ByteSize()
f.write(_VarintBytes(size))
f.write(my_metric.SerializeToString())
msg_len, new_pos = _DecodeVarint32(buf, 0)
msg_buf = buf[new_pos:new_pos+msg_len]
request = metrics_service_pb.ExportMetricsServiceRequest()
request.ParseFromString(msg_buf)
```

**Go**

Use `Buffer.DecodeMessage()`\.

**C\#**

Use `CodedInputStream`\. This class can read size\-delimited messages\.

**C\+\+**

The functions described in `google/protobuf/util/delimited_message_util.h` can read size\-delimited messages\.

**Other languages**

For other languages, see [Download Protocol Buffers](https://developers.google.com/protocol-buffers/docs/downloads)\.

When implementing the parser, consider that a Kinesis record can contain multiple `ExportMetricsServiceRequest` Protocol Buffers messages, each of them starting with a header with an `UnsignedVarInt32` that indicates the record length in bytes\.
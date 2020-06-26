# Using the PutLogEvents API to Send Manually\-Created Embedded Metric Format Logs<a name="CloudWatch_Embedded_Metric_Format_Generation_PutLogEvents"></a>

You can send embedded metric format logs to CloudWatch Logs using the CloudWatch Logs PutLogEvents API\. When calling PutLogEvents, you need to include the following HTTP header to instruct CloudWatch Logs that the metrics should be extracted\.

```
x-amzn-logs-format: json/emf
```

The following is a full example using the AWS SDK for Java 2\.x:

```
package org.example.basicapp;

import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.cloudwatchlogs.CloudWatchLogsClient;
import software.amazon.awssdk.services.cloudwatchlogs.model.DescribeLogStreamsRequest;
import software.amazon.awssdk.services.cloudwatchlogs.model.DescribeLogStreamsResponse;
import software.amazon.awssdk.services.cloudwatchlogs.model.InputLogEvent;
import software.amazon.awssdk.services.cloudwatchlogs.model.PutLogEventsRequest;

import java.util.Collections;

public class EmbeddedMetricsExample {
        public static void main(String[] args) {

                final String usage = "To run this example, supply a region id (eg. us-east-1), log group, and stream name as command line arguments"
                                + "Ex: PutLogEvents <region-id> <log-group-name> <stream-name>";

                if (args.length != 3) {
                        System.out.println(usage);
                        System.exit(1);
                }

                String regionId = args[0];
                String logGroupName = args[1];
                String logStreamName = args[2];

                CloudWatchLogsClient logsClient = CloudWatchLogsClient.builder().region(Region.of(regionId)).build();

                // A sequence token is required to put a log event in an existing stream.
                // Look up the stream to find its sequence token.
                String sequenceToken = getNextSequenceToken(logsClient, logGroupName, logStreamName);

                // Build a JSON log using the EmbeddedMetricFormat.
                long timestamp = System.currentTimeMillis();
                String message = "{" +
                                "  \"_aws\": {" +
                                "    \"Timestamp\": " + timestamp  + "," +
                                "    \"CloudWatchMetrics\": [" +
                                "      {" +
                                "        \"Namespace\": \"MyApp\"," +
                                "        \"Dimensions\": [[\"Operation\"], [\"Operation\", \"Cell\"]]," +
                                "        \"Metrics\": [{ \"Name\": \"ProcessingLatency\", \"Unit\": \"Milliseconds\" }]" +
                                "      }" +
                                "    ]" +
                                "  }," +
                                "  \"Operation\": \"Aggregator\"," +
                                "  \"Cell\": \"001\"," +
                                "  \"ProcessingLatency\": 100" +
                                "}";
                InputLogEvent inputLogEvent = InputLogEvent.builder()
                        .message(message)
                        .timestamp(timestamp)
                        .build();

                // Specify the request parameters.
                PutLogEventsRequest putLogEventsRequest = PutLogEventsRequest.builder()
                        .overrideConfiguration(builder ->
                                // provide the log-format header of json/emf
                                builder.headers(Collections.singletonMap("x-amzn-logs-format",  Collections.singletonList("json/emf"))))
                        .logEvents(Collections.singletonList(inputLogEvent))
                        .logGroupName(logGroupName)
                        .logStreamName(logStreamName)
                        .sequenceToken(sequenceToken)
                        .build();

                logsClient.putLogEvents(putLogEventsRequest);

                System.out.println("Successfully put CloudWatch log event");
        }

        private static String getNextSequenceToken(CloudWatchLogsClient logsClient, String logGroupName, String logStreamName) {
                DescribeLogStreamsRequest logStreamRequest = DescribeLogStreamsRequest.builder()
                        .logGroupName(logGroupName)
                        .logStreamNamePrefix(logStreamName)
                        .build();

                DescribeLogStreamsResponse describeLogStreamsResponse = logsClient.describeLogStreams(logStreamRequest);

                // Assume that a single stream is returned since a specific stream name was
                // specified in the previous request.
                return describeLogStreamsResponse.logStreams().get(0).uploadSequenceToken();
        }
}
```
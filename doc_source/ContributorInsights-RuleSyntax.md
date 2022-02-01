# Contributor Insights rule syntax<a name="ContributorInsights-RuleSyntax"></a>

This section explains the syntax for Contributor Insights rules\. Use this syntax only when you are creating a rule by entering a JSON block\. If you use the wizard to create a rule, you don't need to know the syntax\. For more information about creating rules using the wizard, see [Creating a Contributor Insights rule](ContributorInsights-CreateRule.md)\.

All matching of rules to log event field names and values is case sensitive\.

The following example illustrates the syntax for JSON logs\.

```
{
    "Schema": {
        "Name": "CloudWatchLogRule",
        "Version": 1
    },
    "LogGroupNames": [
        "API-Gateway-Access-Logs*",
        "Log-group-name2"
    ],
    "LogFormat": "JSON",
    "Contribution": {
        "Keys": [
            "$.ip"
        ],
        "ValueOf": "$.requestBytes",
        "Filters": [
            {
                "Match": "$.httpMethod",
                "In": [
                    "PUT"
                ]
            }
        ]
    },
    "AggregateOn": "Sum"
}
```Fields in Contributor Insights rules

Schema  
 The value of `Schema` for a rule that analyzes CloudWatch Logs data must always be `{"Name": "CloudWatchLogRule", "Version": 1}` 

LogGroupNames  
 An array of strings\. For each element in the array, you can optionally use `*` at the end of a string to include all log groups with names that start with that prefix\.   
Be careful about using wildcards with log group names\. You incur charges or each log event that matches a rule\. If you accidentally search more log groups than you intend, you might incur unexpected charges\. For more information, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

LogFormat  
 Valid values are `JSON` and `CLF`\. 

Contribution  
 This object includes a `Keys` array with as many as four members, optionally a single `ValueOf`, and optionally an array of as many as four `Filters`\. 

Keys  
 An array of up to four log fields that are used as dimensions to classify contributors\. If you enter more than one key, each unique combination of values for the keys is counted as a unique contributor\. The fields must be specified using JSON property format notation\. 

ValueOf  
 \(Optional\) Specify this only when you are specifying `Sum` as the value of `AggregateOn`\. `ValueOf` specifies a log field with numerical values\. In this type of rule, the contributors are ranked by their sum of the value of this field, instead of their number of occurrences in the log entries\. For example, if you want to sort contributors by their total `BytesSent` over a period, you would set `ValueOf` to `BytesSent` and specify `Sum` for `AggregateOn`\. 

Filters  
 \(Optional\) Specifies an array of as many as four filters to narrow the log events that are included in the report\. If you specify multiple filters, Contributor Insights evaluates them with a logical AND operator\. You can use this to filter out irrelevant log events in your search or you can use it to select a single contributor to analyze their behavior\.  
Each member in the array must include a `Match` field and a field indicating the type of matching operator to use\.  
The `Match` field specifies a log field to evaluate in the filter\. The log field is specified using JSON property format notation\.  
The matching operator field must be one of the following: `In`, `NotIn`, `StartsWith`, `GreaterThan`, `LessThan`, `EqualTo`, `NotEqualTo`, or `IsPresent`\. If the operator field is `In`, `NotIn`, or `StartsWith`, it is followed by an array of string values to check for\. Contributor Insights evaluates the array of string values with an OR operator\. The array can include as many as 10 string values\.  
If the operator field is `GreaterThan`, `LessThan`, `EqualTo`, or `NotEqualTo`, it is followed by a single numerical value to compare with\.  
If the operator field is `IsPresent`, it is followed by either `true` or `false`\. This operator matches log events based on whether the specified log field is present in the log event\. The `isPresent` works only with values in the leaf node of JSON properties\. For example, a filter that looks for matches to `c-count` does not evaluate a log event with a value of `details.c-count.c1`\.  
See the following for filter examples:  

```
{"Match": "$.httpMethod", "In": [ "PUT", ] }
{"Match": "$.StatusCode", "EqualTo": 200 }
{"Match": "$.BytesReceived", "GreaterThan": 10000}
{"Match": "$.eventSource", "StartsWith": [ "ec2", "ecs" ] }
```

AggregateOn  
 Valid values are `Count` and `Sum`\. Specifies whether to aggregate the report based on a count of occurrences or a sum of the values of the field that is specified in the `ValueOf` field\. 

**JSON property format notation**

The `Keys`, `ValueOf`, and `Match` fields follow JSON property format with dot notation, where `$` represents the root of the JSON object\. This is followed by a period and then an alphanumeric string with the name of the subproperty\. Multiple property levels are supported\.

The following list illustrates valid examples of JSON property format:

```
$.userAgent
$.endpoints[0]
$.users[1].name
$.requestParameters.instanceId
```

**Additional field in rules for CLF logs**

Common Log Format \(CLF\) log events do not have names for the fields like JSON does\. To provide the fields to use for Contributor Insights rules, a CLF log event can be treated as array with an index starting from 1\. You can specify the first field as **"1"**, the second field as **"2"**, and so on\.

To make a rule for a CLF log easier to read, you can use `Fields`\. This enables you to provide a naming alias for CLF field locations\. For example, you can specify that the location "4" is an IP address\. Once specified, `IpAddress` can be used as property in the `Keys`, `ValueOf`, and `Filters` in the rule\.

The following is an example of a rule for a CLF log that uses the `Fields` field\.

```
{
    "Schema": {
        "Name": "CloudWatchLogRule",
        "Version": 1
    },
    "LogGroupNames": [
        "API-Gateway-Access-Logs*"
    ],
    "LogFormat": "CLF",
    "Fields": {
        "4": "IpAddress",
        "7": "StatusCode"
    },
    "Contribution": {
        "Keys": [
            "IpAddress"
        ],
        "Filters": [
            {
                "Match": "StatusCode",
                "EqualTo": 200
            }
        ]
    },
    "AggregateOn": "Count"
}
```
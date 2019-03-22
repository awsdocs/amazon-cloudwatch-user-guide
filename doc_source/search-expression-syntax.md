# CloudWatch Search Expression Syntax<a name="search-expression-syntax"></a>

A valid search expression has the following format\.

```
SEARCH(' {Namespace, DimensionName1, DimensionName2, ...}, SearchTerm', 'Statistic', Period)
```

For example:

```
SEARCH(' {AWS/EC2,InstanceId} MetricName="CPUUtilization" ', 'Average', 300)
```
+ The first part of the query after the word `SEARCH`, enclosed in curly braces, is the *metric schema* to be searched\. The metric schema contains a metric namespace and one or more dimension names\. Including a metric schema in a search query is optional\. If specified, the metric schema must contain a namespace and can optionally contain one or more dimension names that are valid in that namespace\.

  You don't need to use quote marks inside the metric schema unless a namespace or dimension name includes spaces or non\-alphanumeric characters\. In that case, you must enclose the name that contains those characters with double quotes\.
+ The `SearchTerm` is also optional, but a valid search must contain either the metric schema, the `SearchTerm`, or both\. The `SearchTerm` usually contains one or more metric names or dimension values\. The `SearchTerm` can include multiple terms to search for, by both partial match and exact match\. It can also contain Boolean operators\.

  The `SearchTerm` can include one or more designators, such as `MetricName=` as in this example, but using designators isn't required\.

  The metric schema and `SearchTerm` must be enclosed together in a pair of single quote marks\.
+ The `Statistic` is the name of any valid CloudWatch statistic\. It must be enclosed by single quotes\. For more information, see [Statistics](cloudwatch_concepts.md#Statistic)\.
+ The `Period` is the aggregation time period in seconds\.

The preceding example searches the `AWS/EC2` namespace for any metrics that have `InstanceId` as a dimension name\. It returns all `CPUUtilization` metrics that it finds, with the graph showing the `Average` statistic with an aggregation period of 5 minutes\. 

**Search Expression Limits**

The maximum search expression query size is 1024 characters\. You can have as many as five search expressions on one graph\. A graph can display as many as 100 time series\.

## CloudWatch Search Expressions: Tokenization<a name="search-expression-syntax-tokenization"></a>

When you specify a `SearchTerm`, the search function searches for *tokens*, which are substrings that CloudWatch automatically generates from full metric names, dimension names, dimension values, and namespaces\. CloudWatch generates tokens distinguished by the camel\-case capitalization in the original string\. Numeric characters also serve as the start of new tokens, and non\-alphanumeric characters serve as delimiters, creating tokens before and after the non\-alphanumeric characters\.

A continuous string of the same type of token delimiter character results in one token\. 

All generated tokens are in lowercase\. The following table shows some examples of tokens generated\.


| Original String | Tokens Generated | 
| --- | --- | 
|  CustomCount1  |  `customcount1`, `custom`, `count`, `1`  | 
|  SDBFailure  |  `sdbfailure`, `sdb`, `failure`  | 
|  Project2\-trial333  |  `project2trial333`, `project`, `2`, `trial`, `333`  | 

## CloudWatch Search Expressions: Partial Matches<a name="search-expression-partial-match"></a>

When you specify a `SearchTerm`, the search term is also tokenized\. CloudWatch finds metrics based on partial matches, which are matches of a single token generated from the search term to a single token generated from a metric name, namespace, dimension name, or dimension value\.

Partial match searches to match a single token are case insensitive\. For example, using any of the following search terms can return the `CustomCount1` metric:
+ `count`
+ `Count`
+ `COUNT`

However, using `couNT` as a search term doesn't find `CustomCount1` because the capitalization in the search term `couNT` is tokenized into `cou` and `NT`\.

Searches can also match composite tokens, which are multiple tokens that appear consecutively in the original name\. To match a composite token, the search is case sensitive\. For example, if the original term is `CustomCount1`, searches for `CustomCount` or `Count1` are successful, but searches for `customcount` or `count1` aren't\.

## CloudWatch Search Expressions: Exact Matches<a name="search-expression-exact-match"></a>

You can define a search to find only exact matches of your search term by using double quotes around the part of the search term that requires an exact match\. These double\-quotes are enclosed in the single\-quotes used around the entire search term\. For example, **SEARCH\(' \{MyNamespace\}, "CustomCount1" ', 'Maximum', 120\)** finds the exact string `CustomCount1` if it exists as a metric name, dimension name, or dimension value in the namespace named `MyNamespace`\. However, the searches **SEARCH\(' \{MyNamespace\}, "customcount1" ', 'Maximum', 120\)** or **SEARCH\(' \{MyNamespace\}, "Custom" ', 'Maximum', 120\)** do not find this string\.

You can combine partial match terms and exact match terms in a single search expression\. For example, **SEARCH\(' \{AWS/NetworkELB, LoadBalancer\}, "ConsumedLCUs" OR flow ', 'Maximum', 120\)** returns the Elastic Load Balancing metric named `ConsumedLCUs` as well as all Elastic Load Balancing metrics or dimensions that contain the token `flow`\. 

Using exact match is also a good way to find names with special characters, such as non\-alphanumeric characters or spaces, as in the following example\.

```
SEARCH(' {"My Namespace", "Dimension@Name"}, "Custom:Name[Special_Characters" ', 'Maximum', 120)
```

## CloudWatch Search Expressions: Excluding a Metric Schema<a name="search-expression-no-schema"></a>

All examples shown so far include a metric schema, in curly braces\. Searches that omit a metric schema are also valid\.

For example, **SEARCH\(' "CPUUtilization" ', 'Average', 300\)** returns all metric names, dimension names, dimension values, and namespaces that are an exact match for the string `CPUUtilization`\. In the AWS metric namespaces, this can include metrics from several services including Amazon EC2, Amazon ECS, Amazon SageMaker, and others\.

To narrow this search to only one AWS service, the best practice is to specify the namespace and any necessary dimensions in the metric schema, as in the following example\. Although this narrows the search to the `AWS/EC2` namespace, it would still return results of other metrics if you have defined `CPUUtilization` as a dimension value for those metrics\. 

```
SEARCH(' {AWS/EC2, InstanceType} "CPUUtilization" ', 'Average', 300)
```

Alternatively you could add the namespace in the `SearchTerm` as in the following example\. But in this example, the search would match any `AWS/EC2` string, even if it was a custom dimension name or value\.

```
SEARCH(' "AWS/EC2" MetricName="CPUUtilization" ', 'Average', 300)
```

## CloudWatch Search Expressions: Specifying Property Names in the Search<a name="search-expression-type-of-search-term"></a>

The following exact match search for `"CustomCount1"` returns all metrics with exactly that name\.

```
SEARCH(' "CustomCount1" ', 'Maximum', 120)
```

But it also returns metrics with dimension names, dimension values, or namespaces of `CustomCount1`\. To structure your search further, you can specify the property name of the type of object that you want to find in your searches\. The following example searches all namespaces and returns metrics named `CustomCount1`\.

```
SEARCH(' MetricName="CustomCount1" ', 'Maximum', 120)
```

You can also use namespaces and dimension name/value pairs as property names, as in the following examples\. The first of these examples also illustrates that you can use property names with partial match searches as well\.

```
SEARCH(' InstanceType=micro ', 'Average', 300)
```

```
SEARCH(' InstanceType="t2.micro" Namespace="AWS/EC2" ', 'Average', 300)
```

## CloudWatch Search Expressions: Non\-Alphanumeric Characters<a name="search-expression-syntax-characters"></a>

Non\-alphanumeric characters serve as delimiters, and mark where the names of metrics, dimensions, namespaces, and search terms are to be separated into tokens\. When terms are tokenized, non\-alphanumeric characters are stripped out and don't appear in the tokens\. For example, `Network-Errors_2` generates the tokens `network`, `errors`, and `2`\. 

Your search term can include any non\-alphanumeric characters\. If these characters appear in your search term, they can specify composite tokens in a partial match\. For example, all of the following searches would find metrics named either `Network-Errors-2` or `NetworkErrors2`\. 

```
network/errors
network+errors
network-errors
Network_Errors
```

When you're doing an exact value search, any non\-alphanumeric characters used in the exact search must be the correct characters that appear in the string being searched for\. For example, if you want to find `Network-Errors-2`, searching for `"Network-Errors-2"` is successful, but a search for `"Network_Errors_2"` isn't\.

When you perform an exact match search, the following characters must be escaped with a backslash\.

```
" \ ( )
```

For example, to find the metric name `Europe\France Traffic(Network)` by exact match, use the search term **"Europe\\\\France Traffic\\\(Network\\\)"**

## CloudWatch Search Expressions: Boolean Operators<a name="search-expression-boolean-operators"></a>

Search supports the use of the Boolean operators `AND`, `OR`, and `NOT` within the `SearchTerm`\. Boolean operators are enclosed in the single quote marks that you use to enclose the entire search term\. Boolean operators are case sensitive, so `and`, `or`, and `not` aren't valid as Boolean operators\.

You can use `AND` explicitly in your search, such as **SEARCH\('\{AWS/EC2,InstanceId\} network AND packets', 'Average', 300\)**\. Not using any Boolean operator between search terms implicitly searches them as if there were an `AND` operator, so **SEARCH\(' \{AWS/EC2,InstanceId\} network packets ', 'Average', 300\)** yields the same search results\.

Use `NOT` to exclude subsets of data from the results\. For example, **SEARCH\(' \{AWS/EC2,InstanceId\} MetricName="CPUUtilization" NOT i\-1234567890123456 ', 'Average', 300\)** returns the `CPUUtilization` for all your instances, except for the instance `i-1234567890123456`\. You can also use a `NOT` clause as the only search term\. For example, **SEARCH\( 'NOT Namespace=AWS ', 'Maximum', 120\)** yields all your custom metrics \(metrics with namespaces that don't include `AWS`\)\.

You can use multiple `NOT` phrases in a query\. For example, **SEARCH\(' \{AWS/EC2,InstanceId\} MetricName="CPUUtilization" NOT "ProjectA" NOT "ProjectB" ', 'Average', 300\)** returns the `CPUUtilization` of all instances in the Region, except for those with dimension values of `ProjectA` or `ProjectB`\.

You can combine Boolean operators for more powerful and detailed searches, as in the following examples\. Use parentheses to group the operators\.

Both of the next two examples return all metric names containing `ReadOps` from both the EC2 and EBS namespaces\.

```
SEARCH(' (EC2 OR EBS) AND MetricName=ReadOps ', 'Maximum', 120)
```

```
SEARCH(' (EC2 OR EBS) MetricName=ReadOps ', 'Maximum', 120)
```

The following example narrows the previous search to only results that include `ProjectA`, which could be the value of a dimension\. 

```
SEARCH(' (EC2 OR EBS) AND ReadOps AND ProjectA ', 'Maximum', 120)
```

The following example uses nested grouping\. It returns Lambda metrics for `Errors` from all functions, and `Invocations` of functions with names that include the strings `ProjectA` or `ProjectB`\.

```
SEARCH(' {AWS/Lambda,FunctionName} MetricName="Errors" OR (MetricName="Invocations" AND (ProjectA OR ProjectB)) ', 'Average', 600)
```

## CloudWatch Search Expressions: Using Math Expressions<a name="search-expression-math-expressions"></a>

You can use a search expression within a math expressions in a graph\. 

For example, **SUM\(SEARCH\(' \{AWS/Lambda, FunctionName\}, MetricName="Errors" ', 'Sum', 300\)\)** returns the sum of the `Errors` metric of all your Lambda functions\.

Using separate lines for your search expression and math expression might yield more useful results\. For example, suppose that you use the following two expressions in a graph\. The first line displays separate `Errors` lines for each of your Lambda functions\. The ID of this expression is `e1`\. The second line adds another line showing the sum of the errors from all of the functions\.

```
SEARCH(' {AWS/Lambda, FunctionName}, MetricName="Errors" ', 'Sum', 300)
SUM(e1)
```
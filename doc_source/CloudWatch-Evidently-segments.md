# Use segments to focus your audience<a name="CloudWatch-Evidently-segments"></a>

You can define audience *segments* and use them in your launches and experiments\. A segment is a portion of your audience that shares one or more characteristics\. Examples could be Chrome browser users, users in Europe, or Firefox browser users in Europe who also fit other criteria that your application collects, such as age\.

Using a segment in an experiment limits that experiment to evaluating only the users who match the segment criteria\. When you use one or more segments in a launch, you can define different traffic splits for the different audience segments\.

## Segment rule pattern syntax<a name="CloudWatch-Evidently-segments-syntax"></a>

To create a segment, define a segment rule *pattern*\. Specify the attributes that you want to use to evaluate whether a user session will be in the segment\. The pattern that you create is compared to the value of `evaluationContext` that Evidently finds in a user session\. For more information, see [Using EvaluateFeature](CloudWatch-Evidently-code-application.md#CloudWatch-Evidently-code-EvaluateFeature)\.

To create an segment rule pattern, specify the fields that you want the pattern to match\. You can also use logic in your pattern, such as `And`, `Or`, `Not` and `Exists`\. 



For an `evaluationContext` to match a pattern, the `evaluationContext` must match all parts of the rule pattern\. Evidently ignores the fields in the `evaluationContext` that aren't included in the rule pattern\. 

The values that rule patterns match follow JSON rules\. You can include strings enclosed in quotation marks \("\), numbers, and the keywords `true`, `false`, and `null`\.

For strings, Evidently uses exact character\-by\-character matching without case\-folding or any other string normalization\. Therefore, rule matches are case\-sensitive\. For example, if your `evaluationContext` includes a `browser` attribute but your rule pattern checks for `Browser`, it will not match\.

For numbers, Evidently uses string representation\. For example, 300, 300\.0, and 3\.0e2 are not considered equal\.

When you write rule patterns to match `evaluationContext`, you can use the `TestSegmentPattern` API or the `test-segment-pattern` CLI command to test that your pattern matches the correct JSON\. For more information, see [ TestSegmentPattern](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_TestSegmentPattern.html)\. 

The following summary shows all the comparison operators that are available in Evidently segment patterns\.


| **Comparison** | **Example** | **Rule syntax** | 
| --- | --- | --- | 
|  Null  |  UserID is null  |  <pre>{<br />    "UserID": [ null ]<br />}</pre>  | 
|  Empty  |  LastName is empty  |  <pre>{<br />    "LastName": [""]<br />}</pre>  | 
|  Equals  |  Browser is "Chrome"  |  <pre>{<br />    "Browser": [ "Chrome" ]<br />}</pre>  | 
|  And  |  Country is "France" and Device is "Mobile"  |  <pre>{<br />    "Country": [ "France" ], "Device": ["Mobile"]<br />}</pre>  | 
|  Or \(multiple values of a single attribute\)  |  Browser is "Chrome" or "Firefox"  |  <pre>{<br />    "Browser": ["Chrome", "Firefox"]<br />}</pre>  | 
|  Or \(different attributes\)  |  Browser is "Safari" or Device is "Tablet"  |  <pre>{<br /> "$or": [<br />   {"Browser": ["Safari"]},<br />   {"Device": ["Tablet"}]<br /> ]<br />}</pre>  | 
|  Not  |  Browser is anything but "Safari"  |  <pre>{<br />    "Browser": [ { "anything-but": [ "Safari" ] } ]<br />}</pre>  | 
|  Numeric \(equals\)  |  Price is 100  |  <pre>{<br />    "Price": [ { "numeric": [ "=", 100 ] } ]<br />}</pre>  | 
|  Numeric \(range\)  |  Price is more than 10, and less than or equal to 20  |  <pre>{<br />    "Price": [ { "numeric": [ ">", 10, "<=", 20 ] } ]<br />}</pre>  | 
|  Exists  |  Age field exists  |  <pre>{<br />    "Age": [ { "exists": true } ]<br />} </pre>  | 
|  Does not exist  |  Age field does not exist  |  <pre>{<br />    "Age": [ { "exists": false } ]<br />}</pre>  | 
|  Begins with a prefix  |  Region is in the United States  |  <pre>{<br />    "Region": [ {"prefix": "us-" } ]<br />}</pre>  | 
|  Ends with a suffix  |  Location has a suffix "West"  |  <pre>{<br />    "Region": [ {"suffix": "West" } ]<br />}</pre>  | 

## Segment rule examples<a name="CloudWatch-Evidently-segments-examples"></a>

All of the following examples assume that you are passing values for `evaluationContext` with the same field labels and values that you are using in your rule patterns\. 

The following example matches if `Browser` is Chrome or Firefox and `Location` is US\-West\.

```
{
  "Browser": ["Chrome", "Firefox"],
  "Location": ["US-West"]
}
```

The following example matches if `Browser` is any browser except Chrome, the `Location` starts with `US`, and an `Age` field exists\.

```
{
  "Browser": [ {"anything-but": ["Chrome"]}],
  "Location": [{"prefix": "US"}],
  "Age": [{"exists": true}]
}
```

The following example matches if the `Location` is Japan and either `Browser` is Safari or `Device` is Tablet\.

```
{
 "Location": ["Japan"],
 "$or": [
   {"Browser": ["Safari"]},
   {"Device": ["Tablet"]}
 ]
}
```

## Create a segment<a name="CloudWatch-Evidently-create-segment"></a>

After you create a segment, you can use it in any launch or experiment in any project\.

**To create a segment**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the **Segments** tab\.

1. Choose **Create segment**\.

1. For **Segment name**, enter a name to use to identify this segment\.

   Optionally, add a description\.

1. For **Segment pattern**, enter a JSON block that defines the rule pattern\. For more information about rule pattern syntax, see [Segment rule pattern syntax](#CloudWatch-Evidently-segments-syntax)\.
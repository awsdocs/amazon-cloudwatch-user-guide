# Sample custom widgets<a name="add_custom_widget_samples"></a>

AWS provides sample custom widgets in both JavaScript and Python\. You can create these sample widgets by using the link for each widget in this list\. Alternatively, you can create and customize a widget by using the CloudWatch console\. The links in this list open an AWS CloudFormation console and use an AWS CloudFormation quick\-create link to create the custom widget\.

You can also access the custom widget samples on [GitHub](https://github.com/aws-samples/cloudwatch-custom-widgets-samples)\.

Following this list, complete examples of the Echo widget are shown for each language\.

------
#### [ JavaScript ]

**Sample custom widgets in JavaScript**
+ [ Echo](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetEcho-js&template=customWidgets/customWidgetEcho-js.yaml&param_DoCreateExampleDashboard=Yes) – A basic echoer that you can use to test how HTML appears in a custom widget, without having to write a new widget\.
+ [ Hello world](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetHelloWorld-js&template=customWidgets/customWidgetHelloWorld-js.yaml&param_DoCreateExampleDashboard=Yes) – A very basic starter widget\.
+ [ Custom widget debugger](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetDebugger-js&template=customWidgets/customWidgetDebugger-js.yaml&param_DoCreateExampleDashboard=Yes) – A debugger widget that displays useful information about the Lambda runtime environment\.
+ [ Query CloudWatch Logs Insights](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetLogsInsightsQuery-js&template=customWidgets/customWidgetLogsInsightsQuery-js.yaml&param_DoCreateExampleDashboard=Yes) – Run and edit CloudWatch Logs Insights queries\.
+ [ Run Amazon Athena queries](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetAthenaQuery-js&template=customWidgets/customWidgetAthenaQuery-js.yaml&param_DoCreateExampleDashboard=Yes) – Run and edit Athena queries\.
+ [ Call AWS API](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetAwsCall-js&template=customWidgets/customWidgetAwsCall-js.yaml&param_DoCreateExampleDashboard=Yes) – Call any read\-only AWS API and display the results in JSON format\.
+ [ Fast CloudWatch bitmap graph](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetCloudWatchBitmapGraph-js&template=customWidgets/customWidgetCloudWatchBitmapGraph-js.yaml&param_DoCreateExampleDashboard=Yes) – Render CloudWatch graphs using on the server side, for fast display\.
+ [ Text widget from CloudWatch dashboard](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetIncludeTextWidget-js&template=customWidgets/customWidgetIncludeTextWidget-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays the first text widget from the specified CloudWatch dashboard\.
+ [ CloudWatch metric data as a table](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetCloudWatchMetricDataTable-js&template=customWidgets/customWidgetCloudWatchMetricDataTable-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays raw CloudWatch metric data in a table\.
+ [ Amazon EC2 table](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetEc2Table-js&template=customWidgets/customWidgetEc2Table-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays the top EC2 instances by CPU utilization\. This widget also includes a Reboot button, which is disabled by default\.
+ [AWS CodeDeploy deployments](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetCodeDeploy-js&template=customWidgets/customWidgetCodeDeploy-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays CodeDeploy deployments\.
+ [AWS Cost Explorer report](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetCostExplorerReport-js&template=customWidgets/customWidgetCostExplorerReport-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays a report on the cost of each AWS service for a selected time range\.
+ [ Display content of external URL](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetFetchURL-js&template=customWidgets/customWidgetFetchURL-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays the content of an externally accessible URL\.
+ [ Display an Amazon S3 object](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetS3GetObject-js&template=customWidgets/customWidgetS3GetObject-js.yaml&param_DoCreateExampleDashboard=Yes) – Displays an object in an Amazon S3 bucket in your account\.
+ [ Simple SVG pie chart](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetSimplePie-js&template=customWidgets/customWidgetSimplePie-js.yaml&param_DoCreateExampleDashboard=Yes) – Example of a graphical SVG\-based widget\.

------
#### [ Python ]

**Sample custom widgets in Python**
+ [ Echo](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetEcho-py&template=customWidgets/customWidgetEcho-py.yaml&param_DoCreateExampleDashboard=Yes) – A basic echoer which you can use to test how HTML appears in a custom widget, without having to write a new widget\.
+ [ Hello world](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetHelloWorld-py&template=customWidgets/customWidgetHelloWorld-py.yaml&param_DoCreateExampleDashboard=Yes) – A very basic starter widget\.
+ [ Custom widget debugger](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetDebugger-py&template=customWidgets/customWidgetDebugger-py.yaml&param_DoCreateExampleDashboard=Yes) – A debugger widget that displays useful information about the Lambda runtime environment\.
+ [ Call AWS API](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetAwsCall-py&template=customWidgets/customWidgetAwsCall-py.yaml&param_DoCreateExampleDashboard=Yes) – Call any read\-only AWS API and display the results in JSON format\.
+  [ Fast CloudWatch bitmap graph](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetCloudWatchBitmapGraph-py&template=customWidgets/customWidgetCloudWatchBitmapGraph-py.yaml&param_DoCreateExampleDashboard=Yes) – Render CloudWatch graphs using on the server side, for fast display\.
+  [ Send dashboard snapshot by email](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetEmailDashboardSnapshot-py&template=customWidgets/customWidgetEmailDashboardSnapshot-py.yaml&param_DoCreateExampleDashboard=Yes) – Take a snapshot of the current dashboard and send it to email recipients\.
+  [ Send dashboard snapshot to Amazon S3](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetSnapshotDashboardToS3-py&template=customWidgets/customWidgetSnapshotDashboardToS3-py.yaml&param_DoCreateExampleDashboard=Yes) – Take a snapshot of the current dashboard and store it in Amazon S3\.
+ [ Text widget from CloudWatch dashboard](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetIncludeTextWidget-py&template=customWidgets/customWidgetIncludeTextWidget-py.yaml&param_DoCreateExampleDashboard=Yes) – Displays the first text widget from the specified CloudWatch dashboard\.
+ [ Display content of external URL](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetFetchURL-py&template=customWidgets/customWidgetFetchURL-py.yaml&param_DoCreateExampleDashboard=Yes) – Displays the content of an externally accessible URL\.
+ [ RSS reader](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetRssReader-py&template=customWidgets/customWidgetRssReader-py.yaml&param_DoCreateExampleDashboard=Yes) – Displays RSS feeds\.
+ [ Display an Amazon S3 object](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetS3GetObject-py&template=customWidgets/customWidgetS3GetObject-py.yaml&param_DoCreateExampleDashboard=Yes) – Displays an object in an Amazon S3 bucket in your account\.
+ [ Simple SVG pie chart](https://console.aws.amazon.com/cloudwatch/cfn.js?region=us-east-1&action=create&stackName=customWidgetSimplePie-py&template=customWidgets/customWidgetSimplePie-py.yaml&param_DoCreateExampleDashboard=Yes) – Example of a graphical SVG\-based widget\.

------

**Echo widget in JavaScript**

The following is the Echo sample widget in JavaScript\.

```
const DOCS = `
## Echo
A basic echo script. Anything passed in the \`\`\`echo\`\`\` parameter is returned as the content of the custom widget.
### Widget parameters
Param | Description
---|---
**echo** | The content to echo back
 
### Example parameters
\`\`\` yaml
echo: <h1>Hello world</h1>
\`\`\`
`;
 
exports.handler = async (event) => {
    if (event.describe) {
        return DOCS;   
    }
    
    let widgetContext = JSON.stringify(event.widgetContext, null, 4);
    widgetContext = widgetContext.replace(/</g, '&lt;');
    widgetContext = widgetContext.replace(/>/g, '&gt;');
    
    return `${event.echo || ''}<pre>${widgetContext}</pre>`;
};
```

**Echo widget in Python**

The following is the Echo sample widget in Python\.

```
import json
     
DOCS = """
## Echo
A basic echo script. Anything passed in the ```echo``` parameter is returned as the content of the custom widget.
### Widget parameters
Param | Description
---|---
**echo** | The content to echo back
     
### Example parameters
``` yaml
echo: <h1>Hello world</h1>
```"""
 
def lambda_handler(event, context):
    if 'describe' in event:
        return DOCS
        
    echo = event.get('echo', '')
    widgetContext = event.get('widgetContext')
    widgetContext = json.dumps(widgetContext, indent=4)
    widgetContext = widgetContext.replace('<', '&lt;')
    widgetContext = widgetContext.replace('>', '&gt;')
        
    return f'{echo}<pre>{widgetContext}</pre>'
```

**Echo widget in Java**

The following is the Echo sample widget in Java\.

```
package example;
 
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
 
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
 
public class Handler implements RequestHandler<Event, String>{
 
  static String DOCS = ""
    + "## Echo\n"
    + "A basic echo script. Anything passed in the ```echo``` parameter is returned as the content of the custom widget.\n"
    + "### Widget parameters\n"
    + "Param | Description\n"
    + "---|---\n"
    + "**echo** | The content to echo back\n\n"
    + "### Example parameters\n"
    + "```yaml\n"
    + "echo: <h1>Hello world</h1>\n"
    + "```\n";
 
  Gson gson = new GsonBuilder().setPrettyPrinting().create();
 
  @Override
  public String handleRequest(Event event, Context context) {
 
    if (event.describe) {
      return DOCS;
    }
     
    return (event.echo != null ? event.echo : "") + "<pre>" + gson.toJson(event.widgetContext) + "</pre>";
  }
}
     
class Event {
 
    public boolean describe;
    public String echo;
    public Object widgetContext;
 
    public Event() {}
 
    public Event(String echo, boolean describe, Object widgetContext) {
        this.describe = describe;
        this.echo = echo;
        this.widgetContext = widgetContext;
    }
}
```
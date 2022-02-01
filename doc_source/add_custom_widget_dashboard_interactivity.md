# Interactivity in the custom widget<a name="add_custom_widget_dashboard_interactivity"></a>

Even though JavaScript is not allowed, there are other ways to allow interactivity with the returned HTML\.
+ Any element in the returned HTML can be tagged with special configuration in a `<cwdb-action>` tag, which can display information in pop\-ups, ask for confirmation on clicks, and call any Lambda function when that element is chosen\. For example, you can define buttons that call any AWS API using a Lambda function\. The returned HTML can be set to either replace the existing Lambda widget's content, or display inside a modal\.
+ The returned HTML can include links that open new consoles, open other customer pages, or load other dashboards\.
+ The HTML can include the `title` attribute for an element, which gives additional information if the user pauses on that element\.
+ The element can include CSS selectors, such as `:hover`, which can invoke animations or other CSS effects\. You can also show or hide elements in the page\.

## <cwdb\-action> definition and usage<a name="add_custom_widget_dashboard_cwdb"></a>

The `<cwdb-action>` element defines a behavior on the immediately previous element\. The content of the `<cwdb-action>` is either HTML to display or a JSON block of parameters to pass to a Lambda function\.

The following is an example of a `<cwdb-action>` element\.

```
<cwdb-action 
     action="call|html" 
     confirmation="message" 
     display="popup|widget" 
     endpoint="<lambda ARN>" 
     event="click|dblclick|mouseenter">  
 
     html | params in JSON
</cwdb-action>
```
+ **action**— Valid values are `call`, which calls a Lambda function, and `html`, which displays any HTML contained within `<cwdb-action>`\. The default is `html`\.
+ **confirmation**— Displays a confirmation message that must be acknowledged before the action is taken, allowing the customer to cancel\.
+ **display**— Valid values are `popup` and `widget`, which replaces the content of the widget itself\. The default is `widget`\.
+ **endpoint**— The Amazon Resource Name \(ARN\) of the Lambda function to call\. This is required if `action` is `call`\.
+ **event**— Defines the event on the previous element that invokes the action\. Valid values are `click`, `dblclick`, and `mouseenter`\. The `mouseenter` event can be used only in combination with the `html` action\. The default is `click`\. 

**Examples**

The following is an example of how to use the `<cwdb-action>` tag to create a button that reboots an Amazon EC2 instance using a Lambda function call\. It displays the success or failure of the call in a pop\-up\.

```
<a class="btn">Reboot Instance</a>
<cwdb-action action="call" endpoint="arn:aws:lambda:us-east-1:123456:function:rebootInstance" display="popup">  
       { "instanceId": "i-342389adbfef" }
</cwdb-action>
```

The next example displays more information in a pop\-up\.

```
<a>Click me for more info in popup</a>
<cwdb-action display="popup"> 
   <h1>Big title</h1>
   More info about <b>something important</b>.
</cwdb-action>
```

This example is a **Next** button that replaces the content of a widget with a call to a Lambda function\.

```
<a class="btn btn-primary">Next</a>
<cwdb-action action="call" endpoint="arn:aws:lambda:us-east-1:123456:function:nextPage"> 
   { "pageNum": 2 }
</cwdb-action>
```
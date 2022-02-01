# Details about custom widgets<a name="add_custom_widget_dashboard_about"></a>

Custom widgets work as follows:

1. The CloudWatch dashboard calls the Lambda function containing the widget code\. It passes in any custom parameters that are defined in the widget\.

1. The Lambda function returns a string of HTML, JSON, or Markdown\. Markdown is returned as JSON in the following format:

   ```
   {"markdown":"markdown content"}
   ```

1. The dashboard displays the returned HTML or JSON\.

If the function returns HTML, most HTML tags are supported\. You can use Cascading Style Sheets \(CSS\) styles and Scalable Vector Graphics \(SVG\) to build sophisticated views\.

The default style of HTML elements such as links and tables follow the styling of CloudWatch dashboards\. You can customize this style by using inline styles, using the `<style>` tag\. You can also deactivate the default styles by including a single HTML element with the class of `cwdb-no-default-styles`\. The following example deactivates default styles: `<div class="cwdb-no-default-styles"></div>`\.

Every call by a custom widget to Lambda includes a `widgetContext` element with the following contents, to provide the Lambda function developer with useful context information\.

```
{
    "widgetContext": {
        "dashboardName": "Name-of-current-dashboard",
        "widgetId": "widget-16",
        "accountId": "012345678901",
        "locale": "en",
        "timezone": {
            "label": "UTC",
            "offsetISO": "+00:00",
            "offsetInMinutes": 0
        },
        "period": 300,
        "isAutoPeriod": true,
        "timeRange": {
            "mode": "relative",
            "start": 1627236199729,
            "end": 1627322599729,
            "relativeStart": 86400012,
            "zoom": {
                "start": 1627276030434,
                "end": 1627282956521
             }
        },
        "theme": "light",
        "linkCharts": true,
        "title": "Tweets for Amazon website problem",
        "forms": {
            "all": {}
        },
        "params": {
            "original": "param-to-widget"
        },
        "width": 588,
        "height": 369
    }
}
```

## Default CSS styling<a name="add_custom_widget_styling"></a>

Custom widgets provide the following default CSS styling elements:
+ You can use the CSS class **btn** to add a button\. It turns an anchor \(`<a>`\) into a button as in the following example:

  ```
  <a class="btn" href=https://amazon.com”>Open Amazon</a>
  ```
+ You can use the CSS class **btn btn\-primary** to add a primary button\.
+ The following elements are styled by default: **table**, **select**, **headers \(h1, h2, and h3\)**, **preformatted text \(pre\)**, **input**, and **text area**\.

## Using the describe parameter<a name="add_custom_widget_describe"></a>

We strongly recommend that you support the **describe** parameter in your functions, even if it just returns an empty string\. If you don't support it, and it is called in the custom widget, it displays widget content as if it was documentation\.

If you include the **describe** parameter, the Lambda function returns the documentation in Markdown format and does nothing else\.

When you create a custom widget in the console, after you select the Lambda function a **Get documentation** button appears\. If you choose this button, the function is invoked with the **describe** parameter and the function's documentation is returned\. If the documentation is well\-formatted in markdown, CloudWatch parses the first entry in the documentation that is surrounded by three single backtick characters \(```\) in YAML\. Then, it automatically populates the documentation in the parameters\. The following is an example of such well\-formatted documentation\. 

```
``` yaml
echo: <h1>Hello world</h1>
```
```
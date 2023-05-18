# Send custom events<a name="CloudWatch-RUM-custom-events"></a>

CloudWatch RUM records and ingests the events listed in [Information collected by the CloudWatch RUM web client](CloudWatch-RUM-datacollected.md)\. If you use version 1\.12\.0 or later of the CloudWatch RUM web client, you can define, record, and send additional custom events\. You define the event type name and the data to send for each event type that you define\. Each custom event payload can be up to 6 KB\.

Custom events are ingested only if the app monitor has custom events enabled\. To update the configuration settings of your app monitor, use the CloudWatch RUM console or the [UpdateAppMonitor](https://docs.aws.amazon.com/cloudwatchrum/latest/APIReference/API_UpdateAppMonitor.html) API\. 

After you enable custom events, and then define and send custom events, you can search for them\. To search for them, use the **Events** tab in the CloudWatch RUM console\. Search by using the event type\.

## Requirements and syntax<a name="CloudWatch-RUM-custom-event-syntax"></a>

Custom events consist of an event type and event details\. The requirements for these are as follows:
+ **Event type**
  + This can be either the **type** or **name** of your event\. For example, the CloudWatch RUM built\-in event type called **JsError** has an event type of `com.amazon.rum.js_error_event`\.
  + Must be between 1 and 256 characters\.
  + Can be a combination of alphanumeric characters, underscores, hyphens, and periods\.
+ **Event details**
  + Contains the actual data that you want to record in CloudWatch RUM\.
  + Must be an object that consists of fields and values\.

## Examples of recording custom events<a name="CloudWatch-RUM-custom-event-examples"></a>

There are two ways to record custom events in the CloudWatch RUM web client\. 
+ Use the CloudWatch RUM web client's `recordEvent` API\.
+ Use a customized plugin\.

**Send a custom event using the `recordEvent` API, NPM example**

```
awsRum.recordEvent('my_custom_event', {
        location: 'IAD', 
        current_url: 'amazonaws.com', 
        user_interaction: {
            interaction_1 : "click",
            interaction_2 : "scroll"
        }, 
        visit_count:10
    }
)
```

**Send a custom event using the `recordEvent` API, embedded script example**

```
cwr('recordEvent', {
    type: 'my_custom_event', 
    data: {
        location: 'IAD', 
        current_url: 'amazonaws.com', 
        user_interaction: {
            interaction_1 : "click",
            interaction_2 : "scroll"
        }, 
        visit_count:10
    }
})
```

**Example of sending a custom event using a customized plugin**

```
// Example of a plugin that listens to a scroll event, and
// records a 'custom_scroll_event' that contains the timestamp of the event.
class MyCustomPlugin implements Plugin {
    // Initialize MyCustomPlugin.
    constructor() {
        this.enabled;
        this.context;
        this.id = 'custom_event_plugin';
    }
    // Load MyCustomPlugin.
    load(context) {
        this.context = context;
        this.enable();
    }
    // Turn on MyCustomPlugin.
    enable() {
        this.enabled = true;
        this.addEventHandler();
    }
    // Turn off MyCustomPlugin.
    disable() {
        this.enabled = false;
        this.removeEventHandler();
    }
    // Return MyCustomPlugin Id.
    getPluginId() {
        return this.id;
    }
    // Record custom event.
    record(data) {
        this.context.record('custom_scroll_event', data);
    }
    // EventHandler.
    private eventHandler = (scrollEvent: Event) => {
        this.record({timestamp: Date.now()})
    }
    // Attach an eventHandler to scroll event.
    private addEventHandler(): void {
        window.addEventListener('scroll', this.eventHandler);
    }
    // Detach eventHandler from scroll event.
    private removeEventHandler(): void {
        window.removeEventListender('scroll', this.eventHandler);
    }
}
```
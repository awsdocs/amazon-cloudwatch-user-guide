# Use page groups<a name="CloudWatch-RUM-page-groups"></a>

Use page groups to associate different pages in your application with each other so that you can see aggregated analytics for groups of pages\. For example, you might want to see the aggregated page load times of all of your landing pages\. 

You put pages into page groups by adding one or more tags to page view events in the CloudWatch RUM web client\. The following examples put the `/home` page into the page group named `en` and the page group named `landing`\.

**Embedded script example**

```
cwr('recordPageView', { pageId: '/home', pageTags: ['en', 'landing']});
```

**JavaScript module example**

```
awsRum.recordPageView({ pageId: '/home', pageTags: ['en', 'landing']});
```

**Note**  
Page groups are intended to facilitate aggregating analytics across different pages\. For information about how to define and manipulate `pageIds` for your application, see the **Manually recording page views** section in [\(Optional\) Step 3: Manually modify the code snippet to configure the CloudWatch RUM web client](CloudWatch-RUM-modify-snippet.md)\.
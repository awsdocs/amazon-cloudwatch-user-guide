# \(Optional\) Step 3: Manually modify the code snippet to configure the CloudWatch RUM web client<a name="CloudWatch-RUM-modify-snippet"></a>

You can modify the code snippet before inserting it into your application, to activate or deactivate several options\. For more information, see the [ CloudWatch RUM web client documentation](https://github.com/aws-observability/aws-rum-web/blob/main/docs/cdn_installation.md)\.

There are three configuration options that you should definitely be aware of, as discussed in these sections\.

## Preventing the collection of resource URLs that might contain personal information<a name="CloudWatch-RUM-resourceURL"></a>

By default, the CloudWatch RUM web client is configured to record the URLs of resources downloaded by the application\. These resources include HTML files, images, CSS files, JavaScript files, and so on\. For some applications, URLs may contain personally identifiable information \(PII\)\.

If this is the case for your application, we strongly recommend that you disable the collection of resource URLs by setting `recordResourceUrl: false` in the code snippet configuration, before inserting it into your application\.

## Manually recording page views<a name="CloudWatch-RUM-pageload"></a>

By default, the web client records page views when the page first loads and when the browser's history API is called\. The page ID is `window.location.pathname`\. In some cases, the web client's instrumentation will not record the desired page ID\. In this case, you must disable the web client's page view automation by using the `disableAutoPageView` configuration, and the application must be instrumented to record page views using the `recordPageView command`\.

## Enabling X\-Ray end\-to\-end tracing<a name="CloudWatch-RUM-xraytraceheader"></a>

When you create the app monitor, selecting **Trace my service with AWS X\-Ray** enables the tracing of `XMLHttpRequest` and `fetch` requests made during user sessions that are sampled by the app monitor\. You can then see traces from these HTTP requests in the CloudWatch RUM dashboard, the CloudWatch ServiceLens console, and the X\-Ray console\.

By default, these client\-side traces are not connected to downstream server\-side traces\. To connect client\-side traces to server\-side traces and enable end\-to\-end tracing, set the `addXRayTraceIdHeader` option to `true` in the web client\. This causes the CloudWatch RUM web client to add an X\-Ray trace header to HTTP requests\.

The following code block shows an example of adding client\-side traces\. Some configuration options are omitted from this sample for readibility\.

```
<script>
    (function(n,i,v,r,s,c,u,x,z){...})(
        'cwr',
        '00000000-0000-0000-0000-000000000000',
        '1.0.0',
        'us-west-2',
        'https://client.rum.us-east-1.amazonaws.com/1.0.2/cwr.js',
        {
            enableXRay: true,
            telemetries: [ 
                'errors', 
                'performance',
                [ 'http', { addXRayTraceIdHeader: true } ]
            ]
        }
    );
</script>
```

**Warning**  
Configuring the CloudWatch RUM web client to add an X\-Ray trace header to HTTP requests can cause cross\-origin resource sharing \(CORS\) to fail or invalidate the request's signature if the request is signed with SigV4\. For more information, see the [ CloudWatch RUM web client documentation](https://github.com/aws-observability/aws-rum-web/blob/main/docs/cdn_installation.md)\. We strongly recommend that you test your application before adding a client\-side X\-Ray trace header in a production environment\.

For more information, see the [ CloudWatch RUM web client documentation](https://github.com/aws-observability/aws-rum-web/blob/main/docs/cdn_installation.md#http)
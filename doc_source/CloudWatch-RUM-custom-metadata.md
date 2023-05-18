# Specify custom metadata<a name="CloudWatch-RUM-custom-metadata"></a>

CloudWatch RUM attaches additional data to each event as metadata\. Event metadata consists of attributes in the form of key\-value pairs\. You can use these attributes to search or filter events in the CloudWatch RUM console\. By default, CloudWatch RUM creates some metadata for you\. For more information about the default metadata, see [RUM event metadata](CloudWatch-RUM-datacollected.md#CloudWatch-RUM-datacollected-metadata)\.

You can also use the CloudWatch RUM web client to add custom metadata to CloudWatch RUM events\. The custom metadata can include session attributes and page attributes\.

To add custom metadata, you must use version 1\.10\.0 or later of the CloudWatch RUM web client\.

## Requirements and syntax<a name="CloudWatch-RUM-custom-metadata-syntax"></a>

Each event can include as many as 10 custom attributes in the metadata\. The syntax requirements for custom attributes are as follows:
+ **Keys**
  + Maximum of 128 characters
  + Can include alphanumeric characters, colons \(:\), and underscores \(\_\)
  + Can't begin with `aws:`\.
  + Can't consist entirely of any of the reserved keywords listed in the following section\. Can use those keywords as part of a longer key name\.
+ **Values**
  + Maximum of 256 characters
  + Must be strings, numbers, or Boolean values

**Reserved keywords**

You can't use the following reserved keywords as complete key names\. You can use the following keywords as part of a longer key name, such as `applicationVersion`\.
+ `browserLanguage`
+ `browserName`
+ `browserVersion`
+ `countryCode`
+ `deviceType`
+ `domain`
+ `interaction`
+ `osName`
+ `osVersion`
+ `pageId`
+ `pageTags`
+ `pageTitle`
+ `pageUrl`
+ `parentPageId`
+ `platformType`
+ `referrerUrl`
+ `subdivisionCode`
+ `title`
+ `url`
+ `version`

**Note**  
CloudWatch RUM removes custom attributes from RUM events if an attribute includes a key or value that is not valid, or if the limit of 10 custom attributes per event has already been reached\. 

## Add session attributes<a name="CloudWatch-RUM-session-attributes"></a>

If you configure custom session attributes, they are added to all events in a session\. You configure session attributes either during CloudWatch RUM web client initialization or at runtime by using the `addSessionAttributes` command\.

For example, you can add your application’s version as a session attribute\. Then, in the CloudWatch RUM console, you can filter errors by version to find whether an increased error rate is associated with a particular version of your application\. 

**Adding a session attribute at initialization, NPM example**

The code section in bold adds the session attribute\.

```
import { AwsRum, AwsRumConfig } from 'aws-rum-web';

try {
  const config: AwsRumConfig = {
    allowCookies: true,
    endpoint: "https://dataplane.rum.us-west-2.amazonaws.com",
    guestRoleArn: "arn:aws:iam::000000000000:role/RUM-Monitor-us-west-2-000000000000-00xx-Unauth",
    identityPoolId: "us-west-2:00000000-0000-0000-0000-000000000000",
    sessionSampleRate: 1,
    telemetries: ['errors', 'performance'],
    sessionAttributes: {
        applicationVersion: "1.3.8"
    }
  };

  const APPLICATION_ID: string = '00000000-0000-0000-0000-000000000000';
  const APPLICATION_VERSION: string = '1.0.0';
  const APPLICATION_REGION: string = 'us-west-2';

  const awsRum: AwsRum = new AwsRum(
    APPLICATION_ID,
    APPLICATION_VERSION,
    APPLICATION_REGION,
    config
  );
} catch (error) {
  // Ignore errors thrown during CloudWatch RUM web client initialization
}
```

**Adding a session attribute at runtime, NPM example**

```
awsRum.addSessionAttributes({ 
    applicationVersion: "1.3.8"    
})
```

**Adding a session attribute at initialization, embedded script example**

The code section in bold adds the session attribute\.

```
<script>
    (function(n,i,v,r,s,c,u,x,z){...})(
        'cwr',
        '00000000-0000-0000-0000-000000000000',
        '1.0.0',
        'us-west-2',
        'https://client.rum.us-east-1.amazonaws.com/1.0.2/cwr.js',
        {
            sessionSampleRate:1,
            guestRoleArn:'arn:aws:iam::000000000000:role/RUM-Monitor-us-west-2-000000000000-00xx-Unauth',
            identityPoolId:'us-west-2:00000000-0000-0000-0000-000000000000',
            endpoint:'https://dataplane.rum.us-west-2.amazonaws.com',
            telemetries:['errors','http','performance'],
            allowCookies:true,
            sessionAttributes: {
                applicationVersion: "1.3.8"
            }
        }
    );
</script>
```

**Adding a session attribute at runtime, embedded script example**

```
<script>
    function addSessionAttribute() {
        cwr('addSessionAttributes', {
            applicationVersion: "1.3.8"
        })
    }
            
</script>
```

## Add page attributes<a name="CloudWatch-RUM-page-attributes"></a>

If you configure custom page attributes, they are added to all events on the current page\. You configure page attributes either during CloudWatch RUM web client initialization or at runtime by using the `recordPageView` command\.

For example, you can add your page template as a page attribute\. Then, in the CloudWatch RUM console, you can filter errors by page templates to find whether an increased error rate is associated with a particular page template of your application\. 

**Adding a page attribute at initialization, NPM example**

The code section in bold adds the page attribute\.

```
const awsRum: AwsRum = new AwsRum(
    APPLICATION_ID,
    APPLICATION_VERSION,
    APPLICATION_REGION,
    { disableAutoPageView:  true // optional }
);
awsRum.recordPageView({  
    pageId:'/home',  
    pageAttributes: {
      template: 'artStudio'
    }
});
const credentialProvider = new CustomCredentialProvider();
if(awsCreds) awsRum.setAwsCredentials(credentialProvider);
```

**Adding a page attribute at runtime, NPM example**

```
awsRum.recordPageView({ 
    pageId: '/home', 
    pageAttributes: {
        template: 'artStudio'
    } 
});
```

**Adding a page attribute at initialization, embedded script example**

The code section in bold adds the page attribute\.

```
<script>
    (function(n,i,v,r,s,c,u,x,z){...})(
        'cwr',
        '00000000-0000-0000-0000-000000000000',
        '1.0.0',
        'us-west-2',
        'https://client.rum.us-east-1.amazonaws.com/1.0.2/cwr.js',
        {
            disableAutoPageView: true //optional
        }
    );
    cwr('recordPageView', { 
       pageId: '/home',  
       pageAttributes: {
           template: 'artStudio'
       }
    });
    const awsCreds = localStorage.getItem('customAwsCreds');
    if(awsCreds) cwr('setAwsCredentials', awsCreds)
</script>
```

**Adding a page attribute at runtime, embedded script example**

```
<script>
    function recordPageView() {
        cwr('recordPageView', { 
            pageId: '/home', 
            pageAttributes: {
                template: 'artStudio'
            }
        });
    }        
</script>
```

## Filtering by metadata attributes in the console<a name="CloudWatch-RUM-custom-attiributes-console"></a>

To filter the visualizations in the CloudWatch RUM console with any built\-in or custom metadata attribute, use the search bar\. In the search bar, you can specify as many as 20 filter terms in the form of **key=value** to apply to the visualizations\. For example, to filter data for only the Chrome browser, you could add the filter term **browserName=Chrome**\.

By default, the CloudWatch RUM console retrieves the 100 most common attributes keys and values to display in the dropdown in the search bar\. To add more metadata attributes as filter terms, enter the complete attribute key and value into the search bar\. 

A filter can include as many as 20 filter terms, and you can save up to 20 filters per app monitor\. When you save a filter, it is saved in the **Saved filters** dropdown\. You can also delete a saved filter\. 
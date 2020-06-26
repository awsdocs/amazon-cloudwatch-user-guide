# Using Canary Blueprints<a name="CloudWatch_Synthetics_Canaries_Blueprints"></a>

This section provides details about each of the canary blueprints and the tasks each blueprint is best suited for\. Blueprints are provided for the following canary types: 
+ Heartbeat monitor
+ API Canary
+ Broken link checker
+ GUI Workflow

When you use a blueprint to create a canary, as you fill out the fields in the CloudWatch console, the **Script editor** area of the page displays the canary you are creating as a Node\.js script\. You can also edit your canary in this area to customize it further\.

## Heartbeat Monitoring<a name="CloudWatch_Synthetics_Canaries_Blueprints_Heartbeat"></a>

Hearbeat scripts load the specified URL and store a screenshot of the page and an HTTP archive file \(HAR file\)\. They also store logs of accessed URLs\. 

You can use the HAR files to view detailed performance data about the web pages\. You can analyze the list of web requests and catch performance issues such as time to load for an item\.

## API Canary<a name="CloudWatch_Synthetics_Canaries_Blueprints_API"></a>

API canaries can test the basic Read and Write functions of a REST API\. REST stands for *representational state transfer* and is a set of rules that developers follow when creating an API\. One of these rules states that a link to a specific URL should return a piece of data\.

In a REST API, each URL is called a *request* and the data sent back is the *response*\. Each request consists of the following information:
+ The *endpoint*, which is the URL that you request\.
+ The *method*, which is the type of request that is sent to the server\. REST APIs support GET \(read\), POST \(write\), PUT \(update\), PATCH \(update\), and DELETE \(delete\) operations\.
+ The *headers*, which provide information to both the client and the server\. They are used for authentication and providing information about the body content\. For a list ov valid headers, see [HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)\.
+ The *data* \(or *body*\), contains information to be sent to the server\. This is used only for POST, PUT, PATCH, or DELETE requests\.
+ The URL that you request\.

The API canary blueprint supports GET and POST methods\. When you use this blueprint, you must specify headers\. For example, you can specify **Authorization** as a **Key** and specify the necessary authorization data as the **Value** for that key\.

If you are testing a POST request, you also specify the content to post in the **Data** field\.

![\[The create canary page in the console, with fields filled in for the API canary blueprint.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/canary_create_api_checker.PNG)

## Broken Link Checker<a name="CloudWatch_Synthetics_Canaries_Blueprints_Broken_Links"></a>

The broken link checker collects all the links inside the URL that you are testing by using `document.getElementsByTagName('a')` It tests only up to the number of links that you specify, and the URL itself is counted as the first link\. For example, if you want to check all the links on a page that contains five links, you must specify for the canary to follow six links\.

The canary detects the following types of link errors:
+ 404 Page Not Found
+ Invalid Host Name
+ Bad URL\. For example, the URL is missing a bracket, has extra slashes, or is the wrong protocol\.
+ Invalid HTTP response code
+ The host server returns empty responses with no content and no response code\.
+ The HTTP requests constantly time out during the canary's run\.
+ The host consistently drops connections because it is misconfigured or is too busy\.

## GUI Workflow Builder<a name="CloudWatch_Synthetics_Canaries_Blueprints_GUI_Workflow"></a>

The GUI Workflow Builder blueprint verifies that actions can be taken on your web page\. For example, if you have a web page with a login form, the canary can populate the user and password fields and submit the form to verify that the web page is working correctly\.

When you use a blueprint to create this type of canary, you specify the actions that you want the canary to take on the web page\. The actions that you can use are the following:
+ **Click**— Selects the element that you specify and simulates a user clicking or choosing the element\. To specify the element, use `[id=]` or `a[class=]`\.
+ **Verify selector**— Verifies that the specified element exists on the web page\. This test is useful for verifying that a previous action has causes the correct elements to populate the page\. To specify the element to verify, use `[id=]` or `a[class=]`\.
+ **Verify text**— Verifies that the specified string is contained within the target element\. This test is useful for verifying that a previous action has causes the correct text to be displayed\. To specify the element, use a format such as `div[@id=]//h1` because this action uses the `waitForXPath` function in Puppeteer\.
+ **Input text**— Writes the specified text in the target element\. To specify the element to verify, use `[id=]` or `a[class=]`\.
+ **Click with navigation**— Waits for the whole page to load after choosing the specified element\. This is most useful when you need to reload the page\. To specify the element, use `[id=]` or `a[class=]`\.

For example, the following blueprint clicks the **firstButton** on the specified URL, verifies that the expected selector with the expected text appears, inputs the name `Test_Customer` into the **Name** field, clicks the **Login** button, and then verifies that the login is successful by checking for the **Welcome** text on the next page\.

![\[The create canary page in the console, with fields filled in for the GUI Workflow blueprint.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/canary_create_gui_workflow.PNG)
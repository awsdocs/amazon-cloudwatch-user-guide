# Writing a Canary Script<a name="CloudWatch_Synthetics_Canaries_WritingCanary"></a>

The following sections explain how to write a canary script and how to integrate a canary with other AWS Services\.

**Topics**
+ [Creating a Synthetics Canary From Scratch](#CloudWatch_Synthetics_Canaries_write_from_scratch)
+ [Changing an Existing Puppeteer Script to Use as a Synthetics Canary](#CloudWatch_Synthetics_Canaries_modify_puppeteer_script)
+ [Integrating Your Canary with Other AWS Services](#CloudWatch_Synthetics_Canaries_AWS_integrate)

## Creating a Synthetics Canary From Scratch<a name="CloudWatch_Synthetics_Canaries_write_from_scratch"></a>

Here is an example minimal Synthetics Canary script\. This script passes as a successful run, and returns a string\. To see what a failing canary looks like, change `let fail = false;` to `let fail = true;`\. 

You must define an entry point function for the canary script\. To see how files are uploaded to the Amazon S3 location specified as the canary's `ArtifactS3Location`, create these files under the /tmp folder\. After the script runs, the pass/fail status and the duration metrics are published to CloudWatch and the files under /tmp are uploaded to S3\. 

```
const basicCustomEntryPoint = async function () {

    // Insert your code here

    // Perform multi-step pass/fail check

    // Log decisions made and results to /tmp

    // Be sure to wait for all your code paths to complete 
    // before returning control back to Synthetics.
    // In that way, your canary will not finish and report success
    // before your code has finished executing

    // Throw to fail, return to succeed
    let fail = false;
    if (fail) {
        throw "Failed basicCanary check.";
    }

    return "Successfully completed basicCanary checks.";
};

exports.handler = async () => {
    return await basicCustomEntryPoint();
};
```

Next, we'll expand the script to use Synthetics logging and make a call using the AWS SDK\. For demonstration purposes, this script will create a Amazon DynamoDB client and make a call to the DynamoDB listTables API\. It logs the response to the request and logs either pass or fail depending on whether the request was successful\.

If you have more than a single `.js` file or you have a dependency that your script depends on, you can bundle them all into a single ZIP file that contains the folder structure `nodejs/node_modules/myCanaryFilename.js file and other folders and files`\.

Be sure to set your canary’s script entry point as `myCanaryFilename.handler` to match the filename of your script’s entry point\.

```
const log = require('SyntheticsLogger');
const AWS = require('aws-sdk');
// Require any dependencies that your script needs
// Bundle additional files and dependencies into a .zip file with folder structure
// nodejs/node_modules/additional files and folders

const basicCustomEntryPoint = async function () {

    log.info("Starting DynamoDB:listTables canary.");
    
    let dynamodb = new AWS.DynamoDB();
    var params = {};
    let request = await dynamodb.listTables(params);
    try {
        let response = await request.promise();
        log.info("listTables response: " + JSON.stringify(response));
    } catch (err) {
        log.error("listTables error: " + JSON.stringify(err), err.stack);
        throw err;
    }

    return "Successfully completed DynamoDB:listTables canary.";
};

exports.handler = async () => {
    return await basicCustomEntryPoint();
};
```

## Changing an Existing Puppeteer Script to Use as a Synthetics Canary<a name="CloudWatch_Synthetics_Canaries_modify_puppeteer_script"></a>

This section explains how to take Puppeteer scripts and modify them to run as Synthetics canary scripts\. For more information about Puppeteer, see [Puppeteer API v1\.14\.0](https://github.com/puppeteer/puppeteer/blob/v1.14.0/docs/api.md)\. 

We'll start with this example Puppeteer script:

```
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```

The conversion steps are as follows:
+ Create and export a `handler` function\. The handler is the entry point function for the script\.

  ```
  const basicPuppeteerExample = async function () {};
  
  exports.handler = async () => {
      return await basicPuppeteerExample();
  };
  ```
+ Use the `Synthetics` dependency\.

  ```
  var synthetics = require('Synthetics');
  ```
+ Use the `Synthetics.getPage` function to get a Puppeteer `Page` object\.

  ```
  const page = await synthetics.getPage();
  ```

  The page object returned by the Synthetics\.getPage function has the **page\.on** `request`, `response` and `requestfailed` events instrumented for logging\. Synthetics also sets up HAR file generation for requests and responses on the page, and adds the canary ARN to the user\-agent headers of outgoing requests on the page\.

The script is now ready to be run as a Synthetics canary\. Here is the updated script:

```
var synthetics = require('Synthetics');  // Synthetics dependency

const basicPuppeteerExample = async function () {
    const page = await synthetics.getPage(); // Get instrumented page from Synthetics
    await page.goto('https://example.com');
    await page.screenshot({path: '/tmp/example.png'}); // Write screenshot to /tmp folder
};

exports.handler = async () => {  // Exported handler function 
    return await basicPuppeteerExample();
};
```

## Integrating Your Canary with Other AWS Services<a name="CloudWatch_Synthetics_Canaries_AWS_integrate"></a>

All canaries can use the AWS SDK library\. You can use this library when you write your canary to integrate the canary with other AWS services\.

To do so, you need to add the following code to your canary\. AWS For these examples, AWS Secrets Manager is used as the service that the canary is integrating with\.
+ Import the AWS SDK\.

  ```
  const AWS = require('aws-sdk');
  ```
+ Create a client for the AWS service that you are integrating with\.

  ```
  const secretsManager = new AWS.SecretsManager();
  ```
+ Use the client to make API calls to that service\.

  ```
  var params = {
    SecretId: secretName
  };
  return await secretsManager.getSecretValue(params).promise();
  ```

The following canary script code snippet demonstrates an example of integration with Secrets Manager in more detail\.

```
var synthetics = require('Synthetics');
const log = require('SyntheticsLogger');
 
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();
 
const getSecrets = async (secretName) => {
    var params = {
        SecretId: secretName
    };
    return await secretsManager.getSecretValue(params).promise();
}
 
const secretsExample = async function () {
    let URL = "<URL>";
    let page = await synthetics.getPage();
    
    log.info(`Navigating to URL: ${URL}`);
    const response = await page.goto(URL, {waitUntil: 'domcontentloaded', timeout: 30000});
    
    // Fetch secrets
    let secrets = await getSecrets("secretname")
   
    /**
    * Use secrets to login. 
    *
    * Assuming secrets are stored in a JSON format like:
    * {
    *   "username": "<USERNAME>",
    *   "password": "<PASSWORD>"
    * }
    **/
    let secretsObj = JSON.parse(secrets.SecretString);
    await synthetics.executeStep('login', async function () {
        await page.type(">USERNAME-INPUT-SELECTOR<", secretsObj.username);
        await page.type(">PASSWORD-INPUT-SELECTOR<", secretsObj.password);
        
        await Promise.all([
          page.waitForNavigation({ timeout: 30000 }),
          await page.click(">SUBMIT-BUTTON-SELECTOR<")
        ]);
    });
   
    // Verify login was successful
    await synthetics.executeStep('verify', async function () {
        await page.waitForXPath(">SELECTOR<", { timeout: 30000 });
    });
};

exports.handler = async () => {
    return await secretsExample();
};
```
# Tutorial: A/B testing with the Evidently sample application<a name="CloudWatch-Evidently-sample-application"></a>

This section provides a tutorial for using Amazon CloudWatch Evidently for A/B testing\. This tutorial the Evidently sample application, which is a simple react application\. The sample app will be configured to either display a `showDiscount` feature or not\. When the feature is shown to a user, the price displayed on the shopping website it shown at a 20% discount\. 

In addition to showing the discount to some users and not to others, in this tutorial you set up Evidently to collect page load time metrics from both variations\.

## Step 1: Download the sample application<a name="CloudWatch-Evidently-samplestep1"></a>

Start by downloading the Evidently sample application\.

**To download the sample application**

1. Download the sample application from the following Amazon S3 bucket:

   ```
   https://evidently-sample-application.s3.us-west-2.amazonaws.com/evidently-sample-shopping-app.zip
   ```

1. Unzip the package\.

## Step 2: Add the Evidently endpoint and set up credentials<a name="CloudWatch-Evidently-samplestep2"></a>

Next, add the Region and endpoint for Evidently to the `config.js` file in the `src` directory in the sample app package, as in the following example:

```
evidently: { 
    REGION: "us-west-2",
    ENDPOINT: "https://evidently.us-west-2.amazonaws.com (https://evidently.us-west-2.amazonaws.com/)",
},
```

You also must make sure that the application has permission to call CloudWatch Evidently\.

**To grant the sample app permissions to call Evidently**

1. Federate to your AWS account\.

1. Create an IAM user and attach the **AmazonCloudWatchEvidentlyFullAccess** policy to this user\.

1. Make a note of the IAM user's access key id and secret access key, because you will need them in the next step\.

1. In the same `config.js` file that you modified earlier in this section, enter the values of the access key ID and the secrect access key, as in the following example:

   ```
   credential: {
      accessKeyId: "Access key ID",
      secretAccessKey: "Secret key"
   }
   ```
**Important**  
We use this step to make the sample app as simple as possible for you to try out\. We do not recommend that you put your IAM user credential into your actual production application\. Instead, we recommend that you use Amazon Cognito for authentication\. For more information, see [ Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-integrate-apps.html)\.

## Step 3: Set up code for the feature evaluation<a name="CloudWatch-Evidentlysamplestep3"></a>

When you use CloudWatch Evidently to evaluate a feature, you must use the **EvaluateFeature** operation to randomly select a feature variation for each user session\. This operation assigns user sessions to each variation of the feature, according to the percentages that you specified in the experiment\.

**To set up the feature evaluation code for the bookstore demo app**

1. Add the client builder in the `src/App.jsx` file so that the sample app can call Evidently\.

   ```
   import Evidently from 'aws-sdk/clients/evidently';
   import config from './config';
   
   const defaultClientBuilder = (
       endpoint,
       region,
   ) => {
       const credentials = {
           accessKeyId: config.credential.accessKeyId,
           secretAccessKey: config.credential.secretAccessKey
       }
       return new Evidently({
           endpoint,
           region,
           credentials,
       });
   };
   ```

1. Add the following to the `const App` code section to initiate the client\.

   ```
     if (client == null) {
         client = defaultClientBuilder(
           config.evidently.ENDPOINT,
           config.evidently.REGION,
         );
   ```

1. Construct `evaluateFeatureRequest` by adding the following code\. This code pre\-fills the project name and feature name that we recommend later in this tutorial\. You can substitute your own project and feature names, as long as you also specify those project and feature names in the Evidently console\.

   ```
   const evaluateFeatureRequest = {
                 entityId: id,
                 // Input Your feature name
                 feature: 'showDiscount',
                 // Input Your project name'
                 project: 'EvidentlySampleApp',
               };
   ```

1. Add the code to call Evidently for feature evaluation\. When the request is sent, Evidently randomly assigns the user session to either see the `showDiscount` feature or not\.

   ```
   client.evaluateFeature(evaluateFeatureRequest).promise().then(res => {
       if(res.value?.boolValue !== undefined) {
           setShowDiscount(res.value.boolValue);
       }
       getPageLoadTime()
    })
   ```

## Step 4: Set up code for the experiment metrics<a name="CloudWatch-Evidentlysamplestep4"></a>

For the custom metric, use Evidently's `PutProjectEvents` API to send metric results to Evidently\. The following examples show how to set up the custom metric and send experiment data to Evidently\.

Add the following function to calculate the page load time and use `PutProjectEvents` to send the metric values to Evidently\. Add the following function to into `Home.tsx` and call this function within the `EvaluateFeature` API:

```
const getPageLoadTime = () => {
    const timeSpent = (new Date().getTime() - startTime.getTime()) * 1.000001;
    const pageLoadTimeData = `{
      "details": {
        "pageLoadTime": ${timeSpent}
      },
      "UserDetails": { "userId": "${id}", "sessionId": "${id}"}
    }`;
    const putProjectEventsRequest = {
      project: 'EvidentlySampleApp',
      events: [
        {
          timestamp: new Date(),
          type: 'aws.evidently.custom',
          data: JSON.parse(pageLoadTimeData)
        },
      ],
    };
    client.putProjectEvents(putProjectEventsRequest).promise();
  }
```

Here is what the `App.js` file should look like after all the editing that you have done since downloading it\.

```
import React, { useEffect, useState } from "react";
import { BrowserRouter as Router, Switch } from "react-router-dom";
import AuthProvider from "contexts/auth";
import CommonProvider from "contexts/common";
import ProductsProvider from "contexts/products";
import CartProvider from "contexts/cart";
import CheckoutProvider from "contexts/checkout";
import RouteWrapper from "layouts/RouteWrapper";
import AuthLayout from "layouts/AuthLayout";
import CommonLayout from "layouts/CommonLayout";
import AuthPage from "pages/auth";
import HomePage from "pages/home";
import CheckoutPage from "pages/checkout";
import "assets/scss/style.scss";
import { Spinner } from 'react-bootstrap';

import Evidently from 'aws-sdk/clients/evidently';
import config from './config';

const defaultClientBuilder = (
  endpoint,
  region,
) => {
  const credentials = {
    accessKeyId: config.credential.accessKeyId,
    secretAccessKey: config.credential.secretAccessKey
  }
  return new Evidently({
    endpoint,
    region,
    credentials,
  });
};

const App = () => {
  const [isLoading, setIsLoading] = useState(true);
  const [startTime, setStartTime] = useState(new Date());
  const [showDiscount, setShowDiscount] = useState(false);
  let client = null;
  let id = null;

  useEffect(() => {
    id = new Date().getTime().toString();
    setStartTime(new Date());
    if (client == null) {
      client = defaultClientBuilder(
        config.evidently.ENDPOINT,
        config.evidently.REGION,
      );
    }
    const evaluateFeatureRequest = {
      entityId: id,
      // Input Your feature name
      feature: 'showDiscount',
      // Input Your project name'
      project: 'EvidentlySampleApp',
    };

    // Launch
    client.evaluateFeature(evaluateFeatureRequest).promise().then(res => {
      if(res.value?.boolValue !== undefined) {
        setShowDiscount(res.value.boolValue);
      }
    });

    // Experiment
    client.evaluateFeature(evaluateFeatureRequest).promise().then(res => {
      if(res.value?.boolValue !== undefined) {
        setShowDiscount(res.value.boolValue);
      }
      getPageLoadTime()
    })

    setIsLoading(false);
  },[]);

  const getPageLoadTime = () => {
    const timeSpent = (new Date().getTime() - startTime.getTime()) * 1.000001;
    const pageLoadTimeData = `{
      "details": {
        "pageLoadTime": ${timeSpent}
      },
      "UserDetails": { "userId": "${id}", "sessionId": "${id}"}
    }`;
    const putProjectEventsRequest = {
      project: 'EvidentlySampleApp',
      events: [
        {
          timestamp: new Date(),
          type: 'aws.evidently.custom',
          data: JSON.parse(pageLoadTimeData)
        },
      ],
    };
    client.putProjectEvents(putProjectEventsRequest).promise();
  }
  return (
    !isLoading? (
    <AuthProvider>
      <CommonProvider>
        <ProductsProvider>
          <CartProvider>
            <CheckoutProvider>
              <Router>
                <Switch>
                  <RouteWrapper
                    path="/"
                    exact
                    component={() => <HomePage showDiscount={showDiscount}/>}
                    layout={CommonLayout}
                  />
                  <RouteWrapper
                    path="/checkout"
                    component={CheckoutPage}
                    layout={CommonLayout}
                  />
                  <RouteWrapper
                    path="/auth"
                    component={AuthPage}
                    layout={AuthLayout}
                  />
                </Switch>
              </Router>
            </CheckoutProvider>
          </CartProvider>
        </ProductsProvider>
      </CommonProvider>
    </AuthProvider> ) : (
      <Spinner animation="border" />
    )
  );
};

export default App;
```

Each time a user visits the sample app, a custom metric is sent to Evidently for analysis\. Evidently analyzes each metric and displays results in real time on the Evidently dashboard\. The following example shows a metric payload:

```
[ {"timestamp": 1637368646.468, "type": "aws.evidently.custom", "data": "{\"details\":{\"pageLoadTime\":2058.002058},\"userDetails\":{\"userId\":\"1637368644430\",\"sessionId\":\"1637368644430\"}}" } ]
```

## Step 5: Create the project, feature, and experiment<a name="CloudWatch-Evidently-samplestep5"></a>

Next, you create the project, feature, and experiment in the CloudWatch Evidently console\.

**To create the project, feature, and experiment for this tutorial**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose **Create project** and fill out the fields\. You must use **EvidentlySampleApp** for the project name for the sample to work correctly\. For **Evaluation event storage**, choose **Don't store Evaluation events**\.

   After filling out the fields, choose **Create Project**\.

    For more details, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

1. After the project is created, create a feature in that project\. Name the feature **showDiscount**\. In this feature, create two variations of the **Boolean** type\. Name the first variation **disable** with a value of **False** and name the second variation **enable** with a value of **True**\.

   For more information about creating a feature, see [Add a feature to a project](CloudWatch-Evidently-newfeature.md)\.

1. After you have finished creating the feature, create an experiment in the project\. Name the experiment **pageLoadTime**\.

   This experiment will use a custom metric called `pageLoadTime` that measures the page load time of the page being tested\. Custom metrics for experiments are created using Amazon EventBridge\. For more information about EventBridge, see [ What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)\.

   To create that custom metric, do the following when you create the experiment:
   + Under **Metrics**, for **Metric source**, choose **Custom metrics**\.
   + For **Metric name**, enter **pageLoadTime**\.
   + For **Goal** choose **Decrease**\. This indicates that we want a lower value of this metric to indicate the best variation of the feature\.
   + For **Metric rule**, enter the following:
     + For **Entity ID**, enter **UserDetails\.userId**\.
     + For **Value key**, enter **details\.pageLoadTime**\.
     + For **Units**, enter **ms**\.
   + Choose **Add metric**\.

   For **Audiences**, select **100%** so that all users are entered in the experiment\. Set up the traffic split between the variations to be 50% each\.

   Then, choose **Create experiment** to create the experiment\. After you create it, it does not start until you tell Evidently to start it\.

## Step 6: Start the experiment and test CloudWatch Evidently<a name="CloudWatch-Evidently-samplestep6"></a>

The final steps are starting the experiment and starting the sample app\.

**To start the tutorial experiment**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the **EvidentlySampleApp** project\.

1. Choose the **Experiments** tab\.

1. Choose the button next to **pageLoadTime** and choose **Actions**, **Start experiment**\.

1. Choose a time for the experiment to end\.

1. Choose **Start experiment**\.

   The experiment starts immediately\.

Next, start the Evidently sample app with the following command:

```
npm install -f && npm start
```

Once the app has started, you will be assigned to one of the two feature variations being tested\. One variation displays "20% discount" and the other doesn't\. Keep refreshing the page to see the different variations\.

**Note**  
Evidently has sticky evaluations\. Feature evaluations are deterministic, meaning for the same `entityId` and feature, a user will always receive the same variation assignment\. The only time variation assignments change is when an entity is added to an override or experiment traffic is dialed up\.  
However, to make the use of the sample app tutorial easy for you, Evidently reassigns the sample app feature evaluation every time that you refresh the page, so that you can experience both variations without having to add overrides\.

**Troubleshooting**

We recommend that you use `npm` version 6\.14\.14\. If you see any errors about building or starting the sample app and you are using a different version of npm, do the following\.

**To install `npm` version 6\.14\.14**

1. Use a browser to connect to [https://nodejs\.org/download/release/v14\.17\.5/](https://nodejs.org/download/release/v14.17.5/)\.

1. Download [node\-v14\.17\.5\.pkg](https://nodejs.org/download/release/v14.17.5/node-v14.17.5.pkg) and run this pkg to install npm\.

   If you see a `webpack not found` error, navigate to the `evidently-sample-shopping-app` folder and try the following:

   1. Delete `package-lock.json`

   1. Delete `yarn-lock.json`

   1. Delete `node_modules`

   1. Delete the webpack dependency from `package.json`

   1. Run the following:

      ```
      npm install -f && npm
      ```
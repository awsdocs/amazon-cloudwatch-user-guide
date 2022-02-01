# Tutorial: A/B testing with the sample bookstore app<a name="CloudWatch-Evidently-bookstoreexample"></a>

This section provides a tutorial for using Amazon CloudWatch Evidently for A/B testing\. This tutorial uses the AWS bookstore demo app\. The bookstore demo app is a sample full\-stack web application that creates a storefront and backend\. For more information about this demo app, see [AWS Bookstore Demo App](https://github.com/aws-samples/aws-bookstore-demo-app)\.

## Step 1: Launch the bookstore demo app<a name="CloudWatch-Evidently-bookstorestep1"></a>

Start by launching the bookstore demo app\.

**To launch the bookstore demo app with AWS CloudFormation**

1. Use the following link to open the AWS CloudFormation console and load the template for the bookstore demo app\.

   ```
   https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=MyBookstore&templateURL=https://bookstoretemplate.s3.amazonaws.com/bookstoreTemplate.yaml
   ```

1. Choose **Next**\.

1. Confirm that **Stack name** is set to **MyBookstore** and that **ProjectName** is set to **mybookstore**, and then choose **Next**\.

1. For **Configure stack options**, choose **Next**\.

1. At the bottom of the next page, select **I acknowledge that AWS CloudFormation might create IAM resources with custom names\.** and then choose **Create stack**\.

   The AWS CloudFormation stack deployment will take about 30 minutes to complete\.

1. After the deployment is complete, you can access the bookstore app\.

   To access the bookstore app remotely, sign in to the AWS CloudFormation console and select the stack that you just deployed\. Then choose the **Outputs** tab, and choose the link next to **WebApplication**\.

   To access the bookstore app locally, sign in to the CodeCommit console\. Then, under **Repositories**, select the name of the application and choose **Clone URL**\. Using HTTPS, SSH, or HTTPS\(GRC\), use `git clone` to pull the code\. The following example uses an HTTPS link\.

   ```
   git clone https://git-codecommit.us-west-2.amazonaws.com/v1/repos/mybookstore-WebAssets
   ```

   You can generate your HTTPS Git credentials for CodeCommit in the IAM console\. To do so, select your user and choose the **Security credentials** tab\.

1. After you open the bookstore app, choose **Sign up**\. Use a valid email address\. You will receive a confirmation code in email, and enter that code to complete the sign\-up process\. Then, sign in using that account\.

## Step 2: Add Evidently endpoint and unauthenticated role<a name="CloudWatch-Evidently-bookstorestep2"></a>

This step is required if you want to test Evidently on your own website application\. 

**To configure the Evidently endpoint and unauthenticated role**

1. Add the Evidently Region and endpoint to both the `config.js` and `config.ts` files in the `src/` directory in the bookstore app package:

   ```
   evidently: {
       // Region
       REGION: "us-west-2",
       // Evidently endpoint
       ENDPOINT: "https://evidently.us-west-2.amazonaws.com",
     }
   ```

1. Add the unauthenticated role to both `config.js` and `config.ts` with a prefix, as shown in the following example:

   ```
   arn:aws:iam::
   
   //EXAMPLE 
   UN_AUTH_ROLE: "arn:aws:iam::1234567891011:role/MyBookstore-CognitoUnAuthorizedRole-10PGTHJ0DZ3D2"
   ```

   The `config.js` and `config.ts` files should now look like this\.

   ```
   export default {
     // Default setting
     MAX_ATTACHMENT_SIZE: 5000000,
     // Default setting
     apiGateway: {
       REGION: "us-west-2",
       API_URL: "https://hdk8s9ev64.execute-api.us-west-2.amazonaws.com/prod",
     },
     cognito: {
       // Default setting no need to change
       REGION: "us-west-2",
       // Default setting no need to change
       USER_POOL_ID: "us-west-2_HcWLl7xMr",
       // Default setting no need to change
       APP_CLIENT_ID: "71jti5mq3j65p0ci1lfturefno",
       // Default setting no need to change
       IDENTITY_POOL_ID: "us-west-2:0f283cf1-43ac-4410-b310-0c531ac6ce9e",
    ** *//* Newly added *Unauthenticated role* for bookstore app from Cognito identity pool
       UN_AUTH_ROLE: "arn:aws:iam::750664202209:role/MyBookstore-CognitoUnAuthorizedRole-10PGTHJ0DZ3D2"
     },
    ** *// Evidently endpoint*
     evidently: {
       REGION: "us-west-2",
       ENDPOINT: "https://evidently.us-west-2.amazonaws.com",
     }
   };
   ```

1. Add permissions to call Evidently APIs to the bookstore's unauthenticated role:
   + Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
   + In the navigation pane, choose **Roles**\.
   + Find the bookstore's unauthenticated role and attach the following policy to it:

     ```
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Action": [
                     "evidently:*"
                 ],
                 "Resource": "*",
                 "Effect": "Allow"
             }
         ]
     }
     ```

## Step 3: Create the project, feature, and experiment<a name="CloudWatch-Evidently-bookstorestep3"></a>

Next, you create the project, feature, and experiment in the CloudWatch Evidently console\.

**To create the project, feature, and experiment for this tutorial**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Create a project\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose **Create project** and fill out the fields\. Use **BookstoreProject** for the project name\. For more details, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

1. After the project is created, create a feature in that project\. Name the feature **showDiscount**\. In this feature, create two variations of the **Boolean** type\. Name the first variation **Variation1\-False** with a value of **False** and name the second variation **Variation2\-True** with a value of **True**\.

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

## Step 4: Set up code for the feature evaluation<a name="CloudWatch-Evidently-bookstorestep4"></a>

When you use CloudWatch Evidently to evaluate a feature, you must use the **EvaluateFeature** operation to randomly select a feature variation for each user session\. This operation assigns user sessions to each variation of the feature, according to the percentages that you specified in the experiment\.

**To set up the feature evaluation code for the bookstore demo app**

1. Install the AWS SDK for JavaScript using npm, the Node\.js package manager:

   ```
   npm install aws-sdk
   ```

   You can copy the following script to replace your `package.json` file and run `npm install -f`:

   ```
   {
     "name": "aws-bookstore-demo-app",
     "version": "0.1.0",
     "private": true,
     "dependencies": {
       "@aws-sdk/credential-provider-cognito-identity": "3.30.0",
       "@awsui/components-react": "^3.0.148",
       "@types/graphql": "^14.0.5",
       "@types/jest": "^23.3.10",
       "@types/node": "^10.12.12",
       "@types/react": "^16.7.12",
       "@types/react-bootstrap": "^0.32.15",
       "@types/react-dom": "^16.0.11",
       "@types/react-router-bootstrap": "^0.24.5",
       "@types/react-router-dom": "^4.3.1",
       "aws-amplify": "^1.0.10",
       "aws-sdk": "^2.1033.0",
       "bootstrap": "^3.3.7",
       "react": "^16.5.0",
       "react-bootstrap": "^0.32.4",
       "react-dom": "^16.5.0",
       "react-router-bootstrap": "^0.24.4",
       "react-router-dom": "^4.3.1",
       "react-scripts": "^3.0.1",
       "typescript": "^4.2.4"
     },
     "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test --env=jsdom",
       "eject": "react-scripts eject"
     },
     "devDependencies": {
       "@aws-sdk/types": "3.1.0"
     },
     "browserslist": [
       ">0.2%",
       "not dead",
       "not ie <= 11",
       "not op_mini all"
     ]
   }
   ```

1. To set up credentials to call the AWS SDK, add the following code to your `Home.tsx` file\.

   ```
   import Evidently from 'aws-sdk/clients/evidently';
   import { STS, AssumeRoleWithWebIdentityCommand } from '@aws-sdk/client-sts';
   import { PutProjectEventsRequest , EvaluateFeatureRequest} from 'aws-sdk/clients/evidently';
   import { CredentialProvider, Credentials } from '@aws-sdk/types';
   
   import {
     CognitoIdentityClient,
     GetOpenIdTokenCommand,
     GetIdCommand,
   } from '@aws-sdk/client-cognito-identity';
   
   export type ClientBuilder = (
     endpoint: string,
     region: string,
     credentials: CredentialProvider
   ) => Promise<Evidently>;
   
   interface HomeState {
     isLoading: boolean;
     startTime?: any;
     id?: string;
     showDiscount: boolean
   }
   ```

1. Modify the home component by adding these variable names to modify the constructor\.

   ```
     private client!: Evidently;
     private stsClient: STS;
     private cognitoIdentityClient: CognitoIdentityClient;
   
     constructor(props: HomeProps) {
       super(props);
       this.state = {
         isLoading: true,
         id: "",
         showDiscount: false,
       };
   
       this.stsClient = new STS({ region: config.cognito.REGION });
       this.cognitoIdentityClient = new CognitoIdentityClient({
         region: config.cognito.REGION,
       });
     }
   ```

1. Set up the credentials by adding the following code immediately before `async componentDidMount()`:

   ```
     private defaultClientBuilder: ClientBuilder = async (
       endpoint: string,
       region: string,
       credentialProvider: CredentialProvider
     ) => {
       let credentials: Credentials = await credentialProvider();
       return new Evidently({
         endpoint,
         region,
         credentials,
       });
     };
     //Get credentials from *Unauthenticated role*
     //You need to add *@aws-sdk/credential-provider-cognito-identity* & *@aws-sdk/client-sts* in your package.json
     private cognitoCredentialsProvider = async (): Promise<Credentials> => {
       return this.cognitoIdentityClient
         .send(
           new GetIdCommand({
             IdentityPoolId: config.cognito.IDENTITY_POOL_ID,
           })
         )
         .then((getIdResponse) =>
           this.cognitoIdentityClient.send(
             new GetOpenIdTokenCommand({
               IdentityId: getIdResponse.IdentityId,
             })
           )
         )
         .then((getOpenIdTokenResponse) =>
           this.stsClient.send(
             new AssumeRoleWithWebIdentityCommand({
               RoleArn: config.cognito.UN_AUTH_ROLE,
               RoleSessionName: 'evidently',
               WebIdentityToken: getOpenIdTokenResponse.Token,
             })
           )
         )
         .then((assumeRoleResponse) => {
           if (!assumeRoleResponse.Credentials) {
             throw new Error('STS response did not contain credentials.');
           }
   
           const credentials = {
             accessKeyId: assumeRoleResponse.Credentials.AccessKeyId as string,
             secretAccessKey: assumeRoleResponse.Credentials
               .SecretAccessKey as string,
             sessionToken: assumeRoleResponse.Credentials.SessionToken,
             expiration: assumeRoleResponse.Credentials.Expiration,
           };
           return credentials;
         });
     };
   ```

After completing the previous procedure, the **Home\.tsx** file should look like the following\.

```
import React, { Component } from "react";
import screenshot from "../../images/screenshot.png";
import yourpastorders from "../../images/yourpastorders.png";
import bestSellers from "../../images/bestSellers.png";
import yourshoppingcart from "../../images/yourshoppingcart.png";
import { Hero } from "../../common/hero/Hero";
import { CategoryNavBar } from "../category/categoryNavBar/CategoryNavBar";
import { SearchBar } from "../search/searchBar/SearchBar";
import { BestSellersBar } from "../bestSellers/bestSellersBar/BestSellersBar";
import { CategoryGalleryTeaser } from "../category/CategoryGalleryTeaser";
import { FriendsBought } from "../friends/FriendsBought";
import { LinkContainer } from "react-router-bootstrap";
*import Evidently from 'aws-sdk/clients/evidently';
import { STS, AssumeRoleWithWebIdentityCommand } from '@aws-sdk/client-sts';
import { PutProjectEventsRequest , EvaluateFeatureRequest} from 'aws-sdk/clients/evidently';
import { CredentialProvider, Credentials } from '@aws-sdk/types';
*
*import {
  CognitoIdentityClient,
  GetOpenIdTokenCommand,
  GetIdCommand,
} from '@aws-sdk/client-cognito-identity';
import { Spinner } from '@awsui/components-react';
import "./home.css";
import config from '../../config';


export type ClientBuilder = (
  endpoint: string,
  region: string,
  credentials: CredentialProvider
) => Promise<Evidently>;

*
interface HomeProps {
  isAuthenticated: boolean;
}

*interface HomeState {
  isLoading: boolean;
  startTime?: any;
  id?: string;
  showDiscount: boolean
}
*
export default class Home extends Component<HomeProps, HomeState> {
 *private client!: Evidently;
  private stsClient: STS;
  private cognitoIdentityClient: CognitoIdentityClient;*

 *constructor(props: HomeProps) {
    super(props);
    this.state = {
      isLoading: true,
      id: "",
      showDiscount: false,
    };

    this.stsClient = new STS({ region: config.cognito.REGION });
    this.cognitoIdentityClient = new CognitoIdentityClient({
      region: config.cognito.REGION,
    });
  }
*
 *private defaultClientBuilder: ClientBuilder = async (
    endpoint: string,
    region: string,
    credentialProvider: CredentialProvider
  ) => {
    let credentials: Credentials = await credentialProvider();
    return new Evidently({
      endpoint,
      region,
      credentials,
    });
  };
    //Get credentials from *Unauthenticated role*
  //You need to add *@aws-sdk/credential-provider-cognito-identity* & *@aws-sdk/client-sts* in your package.json
  private cognitoCredentialsProvider = async (): Promise<Credentials> => {
    return this.cognitoIdentityClient
      .send(
        new GetIdCommand({
          IdentityPoolId: config.cognito.IDENTITY_POOL_ID,
        })
      )
      .then((getIdResponse) =>
        this.cognitoIdentityClient.send(
          new GetOpenIdTokenCommand({
            IdentityId: getIdResponse.IdentityId,
          })
        )
      )
      .then((getOpenIdTokenResponse) =>
        this.stsClient.send(
          new AssumeRoleWithWebIdentityCommand({
            RoleArn: config.cognito.UN_AUTH_ROLE,
            RoleSessionName: 'evidently',
            WebIdentityToken: getOpenIdTokenResponse.Token,
          })
        )
      )
      .then((assumeRoleResponse) => {
        if (!assumeRoleResponse.Credentials) {
          throw new Error('STS response did not contain credentials.');
        }

        const credentials = {
          accessKeyId: assumeRoleResponse.Credentials.AccessKeyId as string,
          secretAccessKey: assumeRoleResponse.Credentials
            .SecretAccessKey as string,
          sessionToken: assumeRoleResponse.Credentials.SessionToken,
          expiration: assumeRoleResponse.Credentials.Expiration,
        };
        return credentials;
* ** });
  };

 *async componentDidMount() {
    const randomizedID = new Date().getTime().toString();
    this.setState({id: randomizedID})

    if (!this.props.isAuthenticated) {
      return;
    }
      if (this.client == null) {
      this.client = await this.defaultClientBuilder(
        config.evidently.ENDPOINT,
        config.evidently.REGION,
        this.cognitoCredentialsProvider
      );
    }
    this.setState({ isLoading: false });
    const evaluateFeatureRequest: EvaluateFeatureRequest = {
      entityId: randomizedID,
      // Your feature name
      feature: 'showDiscount',
      // Your project name'
      project: 'BookstoreProject',
    };
    this.client.evaluateFeature(evaluateFeatureRequest).promise().then(res => {
      console.log('evaluateFeature response: ', res);
      if(res.value?.boolValue !== undefined) {
        this.setState({showDiscount: res.value.boolValue});
      }
    })
  }
*
  renderLanding() {
    return (
      <div className="lander">
        <h1>Bookstore Demo</h1>
        <hr />
        <p>This is a sample application demonstrating how different types of databases and AWS services can work together to deliver a delightful user experience.  In this bookstore demo, users can browse and search for books, view recommendations, see the leaderboard, view past orders, and more.  You can get this sample application up and running in your own environment and learn more about the architecture of the app by looking at the <a href="https://github.com/aws-samples/aws-bookstore-demo-app" target="_blank">github repository</a>.</p>
        <div className="button-container col-md-12">
          <LinkContainer to="/signup">
          <a href="/signup">Sign up to explore the demo</a>
          </LinkContainer>
        </div>
        <img src={screenshot} className="img-fluid full-width" alt="Screenshot"></img>
        <div className="product-section">
          <h1>Databases on AWS</h1>
          <hr />
          <h2>Purpose-built databases for all your application needs</h2>
          <div className="col-md-12">
            <p>As the cloud continues to drive down the cost of storage and compute, a new generation of applications have emerged, creating a new set of requirements for databases. These applications need databases to store terabytes to petabytes of new types of data, provide access to the data with millisecond latency, process millions of requests per second, and scale to support millions of users anywhere in the world. To support these requirements, you need both relational and non-relational databases that are purpose-built to handle the specific needs of your applications. AWS offers the broadest range of databases purpose-built for your specific application use cases. </p>
          </div>
        </div>

        <div className="product-section">
          <div className="col-md-12">
            <hr />
            <h3><a href="https://aws.amazon.com/dynamodb/">Amazon DynamoDB</a></h3>
            <h4>NoSQL Database</h4>
            <p><a href="https://aws.amazon.com/dynamodb/">Amazon DynamoDB</a> is a fast and flexible <a href="https://aws.amazon.com/nosql/">NoSQL database service</a> for all applications that need consistent, single-digit millisecond latency at any scale. It is a fully managed cloud database and supports both document and key-value store models. Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad tech, IoT, and many other applications. New for DynamoDB is global tables, which fully automate the replication of tables across AWS regions for a fully managed, multi-master, multi-region database. DynamoDB also adds support for on-demand and continuous backups for native data protection.</p>
            <p>For more information visit the <a href="https://aws.amazon.com/dynamodb/">Amazon DynamoDB product page.</a></p>
          </div>
          <div className="col-md-12">
            <hr />
            <h3><a href="https://aws.amazon.com/neptune/">Amazon Neptune</a></h3>
            <h4>Graph Database</h4>
            <p><a href="https://aws.amazon.com/neptune/">Amazon Neptune</a> is a fast, reliable, fully-managed <a href="https://aws.amazon.com/nosql/graph/">graph database</a> service that makes it easy to build and run applications that work with highly connected datasets. The core of Amazon Neptune is a purpose-built, high-performance graph database engine optimized for storing billions of relationships and querying the graph with milliseconds latency. Amazon Neptune supports popular graph models Apache TinkerPop and W3C's RDF, and their associated query languages TinkerPop Gremlin and RDF SPARQL, allowing you to easily build queries that efficiently navigate highly connected datasets. Neptune powers graph use cases such as recommendation engines, fraud detection, knowledge graphs, drug discovery, and network security.</p>
            <p>For more information visit the <a href="https://aws.amazon.com/neptune/">Amazon Neptune product page.</a></p>
          </div>
          <div className="col-md-12">
            <hr />
            <h3><a href="https://aws.amazon.com/opensearch-service/">Amazon OpenSearch Service</a></h3>
            <h4>In-Memory Data Store</h4>
            <p><a href="https://aws.amazon.com/opensearch-service/">Amazon OpenSearch Service</a> makes it easy to deploy, operate, and scale an <a href="https://aws.amazon.com/nosql/key-value/">in-memory data store</a> or cache in the cloud. The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases. </p>
           </div>
          
          <div className="col-md-12">
            <hr />
            <h3><a href="https://aws.amazon.com/opensearch-service/">Amazon OpenSearch Service</a></h3>
            <h4>Fully managed, scalable, and reliable OpenSearch service</h4>
            <p><a href="https://aws.amazon.com/opensearch-service/">Amazon Opensearch Service</a> is a fully managed service that makes it easy for you to deploy, secure, operate, and scale OpenSearch to search, analyze, and visualize data in real-time. </p>
            <p>For more information visit the <a href="https://aws.amazon.com/opensearch-service/">Amazon OpenSearch Service product page.</a></p>
        </div>
      </div>
    </div>);
  }
  
   renderHome() {
    return (
      <div className="bookstore">
 *{this.state.showDiscount && <div>Enjoy 25% off your order!</div>}
*        <Hero />
        <SearchBar />
        <CategoryNavBar />
        <BestSellersBar />
        <div className="well-bs col-md-12 ad-container-padding">
          <div className="col-md-4 ad-padding">
            <div className="container-category no-padding">
              <LinkContainer to="/past">
                <img src={yourpastorders} alt="Past orders"></img> 
              </LinkContainer>
            </div>
          </div>
          <div className="col-md-4 ad-padding">
            <div className="container-category no-padding">
              <LinkContainer to="/cart">
                <img src={yourshoppingcart} alt="Shopping cart"></img> 
              </LinkContainer>
            </div>
          </div>
          <div className="col-md-4 ad-padding">
            <div className="container-category no-padding">
              <LinkContainer to="/best">
                <img src={bestSellers} alt="Best sellers"></img> 
              </LinkContainer>
            </div>
          </div>
        </div>
        <CategoryGalleryTeaser />
        <FriendsBought />
      </div>
    );
  }
  render() {
    return (
      <div className="Home">
        {this.props.isAuthenticated ? this.renderHome() : this.renderLanding()}
      </div>
    );
  }
}
```

## Step 5: Set up code for the experiment metrics<a name="CloudWatch-Evidently-bookstore-codemetric"></a>

For the custom metric, use Evidently's `PutProjectEvents` API to send metric results to Evidently\. The following examples show how to set up the custom metric and send experiment data to Evidently\.

Add the following code immediately before `async componentDidMount()`:

```
componentWillMount() {
  this.setState({startTime: new Date()})
}
```

Then, add the following function to calculate the page load time and use `PutProjectEvents` to send the metric values to Evidently\. Put the following code into `Home.tsx` and call this function within the `EvaluateFeature` API:

```
getPageLoadTime = () => {
  const timeSpent = (new Date().getTime() - this.state.startTime.getTime()) * 1.000001;
  const pageLoadTimeData = `{
    "details": {
      "pageLoadTime": ${timeSpent}
    },
    "userDetails": { "userId": "${this.state.id}", "sessionId": "${this.state.id}"}
  }`;
  const putProjectEventsRequest: PutProjectEventsRequest = {
    project: 'BookstoreProject',
    events: [
      {
        timestamp: new Date(),
        type: 'aws.evidently.custom',
        data: JSON.parse(pageLoadTimeData)
      },
    ],
  };
  this.client.putProjectEvents(putProjectEventsRequest).promise();
}
```

Now the evaluate feature API code inside `async componentDidMount()` should look like the following:

```
this.client.evaluateFeature(evaluateFeatureRequest).promise().then(res => {
  if(res.value?.boolValue !== undefined) {
    this.setState({showDiscount: res.value.boolValue});
  }
  // Send custom metric to Evidently
  *this.getPageLoadTime()*
})
```

Each time a user visits the bookstore app, a custom metric is sent to Evidently for analysis\. Evidently analyzes each metric and displays results in real time on the Evidently dashboard\. The following example shows a metric payload:

```
[
    {
        "timestamp": 1637368646.468,
        "type": "aws.evidently.custom",
        "data": "{\"details\":{\"pageLoadTime\":2058.002058},\"userDetails\":{\"userId\":\"1637368644430\",\"sessionId\":\"1637368644430\"}}"
    }
]
```

## Step 6: Start the experiment and start the bookstore app<a name="CloudWatch-Evidently-bookstorestep5"></a>

The final steps are starting the experiment and starting the bookstore app\.

**To start the tutorial experiment**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the **BookstoreProject** project\.

1. Choose the **Experiments** tab\.

1. Choose the button next to **pageLoadTime** and choose **Actions**, **Start experiment**\.

1. Choose a time for the experiment to end\.

1. Choose **Start experiment**\.

   The experiment starts immediately\.

Next, start the bookstore app with the following command:

```
npm install -f && npm start
```

Once the app has started, you can sign in and create a bookstore account\. After you have the account created and you are logged in, you will be assigned to one of the two variations being tested\. One variation displays "Enjoy 25% off your order," and the other doesn't\.
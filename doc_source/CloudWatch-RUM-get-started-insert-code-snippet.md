# Step 4: Insert the code snippet into your application<a name="CloudWatch-RUM-get-started-insert-code-snippet"></a>

Next, you insert the code snippet that you created in the previous section into your application\.

**Warning**  
The web client, downloaded and configured by the code snippet, uses cookies \(or similar technologies\) to help you collect end user data\. Before you insert the code snippet, see [Data protection and data privacy with CloudWatch RUM](CloudWatch-RUM-privacy.md)\.

If you don't have the code snippet that was previously generated, you can find it by following the directions in [How do I find a code snippet that I've already generated?](CloudWatch-RUM-find-code-snippet.md)\.

**To insert the CloudWatch RUM code snippet into your application**

1. Insert the code snippet that you copied or downloaded in the previous section inside the `<head>` element of your application\. Insert it before the `<body>` element or any other `<script>` tags\.

   The following is an example of a generated code snippet:

   ```
   <script>
   (function (n, i, v, r, s, c, x, z) {
       x = window.AwsRumClient = {q: [], n: n, i: i, v: v, r: r, c: c};
       window[n] = function (c, p) {
           x.q.push({c: c, p: p});
       };
       z = document.createElement('script');
       z.async = true;
       z.src = s;
       document.head.insertBefore(z, document.getElementsByTagName('script')[0]);
   })('cwr',
       '194a1c89-87d8-41a3-9d1b-5c5cd3dafbd0',
       '1.0.0',
       'us-east-2',
       'https://client.rum.us-east-1.amazonaws.com/1.0.2/cwr.js',
       {
           sessionSampleRate: 1,
           guestRoleArn: "arn:aws:iam::123456789012:role/RUM-Monitor-us-east-2-123456789012-5934510917361-Unauth",
           identityPoolId: "us-east-2:c90ef0ac-e3b8-4d1a-b313-7e73cfd21443",
           endpoint: "https://dataplane.rum.us-east-2.amazonaws.com",
           telemetries: ["performance", "errors", "http"],
           allowCookies: true,
           enableXRay: false
       });
   </script>
   ```

1. If your application is a multipage web application, you must repeat step 1 for each HTML page that you want included in the data collection\.
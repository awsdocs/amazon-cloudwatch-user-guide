# Add a feature to a project<a name="CloudWatch-Evidently-newfeature"></a>

A *feature* in CloudWatch Evidently represents a feature that you want to launch or that you want to test variations of\.

Before you can add a feature, you must create a project\. For more information, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

**To add a feature to a project**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project\.

1. Choose **Add feature**\.

1. For **Feature name**, enter a name to be used to identify this feature within this project\.

   You can optionally add a feature description\.

1. For **Feature variations**, for **Variation type** choose **Boolean**, **Long**, **Double**, or **String**\. For more information, see [Variation types](#CloudWatch-Evidently-variationtypes)\.

1. Add up to five variations for your feature\. The **Value** for each variation must be valid for the **Variation type** that you selected\.

   Specify one of the variations to be the default\. This is the baseline that the other variations will be compared to, and should be the variation that is being served to your users now\. This is also the variation that is served to users who are not added to a launch or experiment for this feature\.

1. Choose **Sample code**\. The code example shows what you need to add to your application to set up the variations and assign user sessions to them\. You can choose between JavaScript, Java, and Python for the code\.

   You don't need to add the code to your application right now, but you must do so before you start a launch or an experiment\.

   For more information, see [Adding code to your application](CloudWatch-Evidently-code-application.md)\.

1. \(Optional\) To specify that certain users always see a certain variation, choose **Overrides**, **Add override**\. Then, specify a user by entering their user ID, account ID, or some other identifier in **Identifier**, and specify which variation they should see\.

   This can be useful for members of your own testing team or other internal users when you want to make sure they see a specific variation\. The sessions of users who are assigned overrides do not contribute to launch or experiment metrics\.

   You can repeat this for as many as 10 users by choosing **Add override** again\.

1. \(Optional\) To add tags to this feature, choose **Tags**, **Add new tag**\.

   Then, for **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\. 

   To add another tag, choose **Add new tag** again\.

   For more information, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

1. Choose **Add feature**\.

## Variation types<a name="CloudWatch-Evidently-variationtypes"></a>

When you create a feature and define the variations, you must select a *variation type*\. The possible types are:
+ Boolean
+ Long integer
+ Double precision floating\-point number
+ String

The variation type sets how the different variations are differentiated in your code\. You can use the variation type to simplify the implementation of CloudWatch Evidently and also to simplify the process of modifying the features in your launches and experiments\.

For example, if you define a feature with the long integer variation type, the integers that you specify to differentiate the variations can be numbers passed directly into your code\. One example might be testing the pixel size of a button The values for the variation types can be the number of pixels used in each variation\. The code for each variation can read the variation type value and use that as the button size\. To test a new button size, you can change the number used for the value of the variation, without making any other code changes\.

When you set the values for your variation types within a feature, you should avoid assigning the same values to multiple variations, unless you want to do A/A testing to initially try out CloudWatch Evidently, or have other reasons to do so\.

Evidently doesn't have native support for JSON as a type, but you can pass in JSON in the String variation type, and parse that JSON in your code\. 
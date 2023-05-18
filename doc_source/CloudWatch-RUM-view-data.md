# Viewing the CloudWatch RUM dashboard<a name="CloudWatch-RUM-view-data"></a>

CloudWatch RUM helps you collect data from user sessions about your application's performance, including page load times, Apdex score, browsers and devices used, geolocation of user sessions, and sessions with errors\. All of this information is displayed in a dashboard\.

**To view the RUM dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **RUM**\.

   The **Overview** tab displays information collected by one of the app monitors that you have created\. 

   The top row of panes displays the following information for this app monitor: 
   + Number of page loads
   + Average page load speed
   +  Apdex score
   + Status of any alarms associated with the app monitor

   The application performance index \(Apdex\) score indicates end users' level of satisfaction\. Scores range from 0 \(least satisfied\) to 1 \(most satisfied\)\. The scores are based on application performance only\. Users are not asked to rate the application\. For more information about Apdex scores, see [How CloudWatch RUM sets Apdex scores](CloudWatch-RUM-apdex.md)\.

   Several of these panes include links that you can use to further examine the data\. Choosing any of these links displays a detailed view with **Performance**, **Errors & Sessions**, **Browsers & Devices**, and **User Journey** tabs at the top of the display\. The views in each of these tabs include filters you can use to narrow the displayed results down to specific browsers, devices, and countries\. 

1. To focus further, choose the **List view** tab and then choose the name of the app monitor that you want to focus on\. This displays the following tabs for the chosen app monitor\.
   + The **Performance** tab displays page performance information including load times, session information, request information, web vitals, and page loads over time\. This view includes controls to toggle the view between focusing on **Page loads**, **Requests**, and **Location**\.
   + The **Errors & Sessions** tab displays page error information including the error message most frequently seen by users, devices and browsers with the most errors, and session error information\. This view includes controls to toggle the view between focusing on **Errors**, and **Sessions**\.

     Near the bottom of the **Errors** view is a list view of errors\. To see more details about one, choose the button at the left of that row in the list, and choose **View details**\.

     Near the bottom of the **Sessions** view is a list view of sessions\. To see more details about one, choose the button at the left of that row in the list, and choose **View session events**\.
   + The **Browsers & Devices** tab displays information such as the performance and usage of different browsers and devices to access your application\. This view includes controls to toggle the view between focusing on **Browsers**, \\ and **Devices**\.

     If you narrow the scope to a single browser, you see the data broken down by browser version\.
   + The **User Journey** tab displays the paths that your customers use to navigate your application\. You can see where your customers enter your application and what page they exit your application from\. You can also see the paths that they take and the percentage of customers that follow those paths\. You can pause on a node to get more details about that page\. You can choose a single path to highlight the connections for easier viewing\.

1. \(Optional\) On any of the first three tabs, you can choose the **Pages** button and select a page or page group from the list\. This narrows down the displayed data to a single page or group of pages of your application\. You can also mark pages and page groups in the list as favorites\.
# Verify Prerequisites<a name="Container-Insights-prerequisites"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

Before you install Container Insights, verify the following:
+ You have a functional Amazon EKS cluster or K8s cluster with nodes attached in one of the Regions that supports the Container Insights preview\. For the list of supported Regions, see [Using Container Insights](ContainerInsights.md)\.
+ You have `kubectl` installed and running\. For more information, see [Installing `kubectl`](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) in the *Amazon EKS User Guide*\.
+ If you are using K8s running on AWS, the following prerequisites are also necessary:
  + Be sure that your K8s cluster has enabled role\-based access control \(RBAC\)\. For more information, see [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) in the Kubernetes Reference\. 
  + Your kubelet has enabled Webhook authorization mode\. For more information, see [Kubelet authentication/authorization](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet-authentication-authorization/) in the Kubernetes Reference\.

You must also attach a policy to the IAM role of your Amazon EKS worker nodes, to enable them to send metrics and logs to CloudWatch\. 

**To add the necessary policy to the IAM role for your worker nodes**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select one of the worker node instances and choose the IAM role in the description\.

1. On the IAM role page, choose **Attach policies**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find this policy\.

1. Choose **Attach policies**\.

If you're running K8s, you might not already have an IAM role attached to the instance\. If not, you need to first attach an IAM role to the instance and then add the policy as explained in the previous steps\. For more information on attaching a role to an instance, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*\.
# Using CloudWatch and CloudWatch Synthetics with interface VPC endpoints<a name="cloudwatch-and-interface-VPC"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC, CloudWatch, and CloudWatch Synthetics\. You can use these connections to enable CloudWatch and CloudWatch Synthetics to communicate with your resources on your VPC without going through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. To connect your VPC to CloudWatch or CloudWatch Synthetics, you define an *interface VPC endpoint* to connect your VPC to AWS services\. The endpoint provides reliable, scalable connectivity to CloudWatch or CloudWatch Synthetics without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [What Is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/) in the *Amazon VPC User Guide*\.

Interface VPC endpoints are powered by AWS PrivateLink, an AWS technology that enables private communication between AWS services using an elastic network interface with private IP addresses\. For more information, see the [New – AWS PrivateLink for AWS Services](https://aws.amazon.com/blogs/aws/new-aws-privatelink-endpoints-kinesis-ec2-systems-manager-and-elb-apis-in-your-vpc/) blog post\.

The following steps are for users of Amazon VPC\. For more information, see [Getting Started](https://docs.aws.amazon.com/vpc/latest/userguide/GetStarted.html) in the *Amazon VPC User Guide*\.

## CloudWatch VPC endpoint<a name="cloudwatch-interface-VPC-availability"></a>

CloudWatch currently supports VPC endpoints in the following AWS Regions:
+ US East \(Ohio\)
+ US East \(N\. Virginia\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ South America \(São Paulo\)
+ AWS GovCloud \(US\-East\)
+ AWS GovCloud \(US\-West\)

### Creating a VPC endpoint for CloudWatch<a name="create-VPC-endpoint-for-CloudWatch"></a>

To start using CloudWatch with your VPC, create an interface VPC endpoint for CloudWatch\. The service name to choose is `com.amazonaws.region.monitoring`\. For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint.html) in the *Amazon VPC User Guide*\.

You do not need to change the settings for CloudWatch\. CloudWatch calls other AWS services using either public endpoints or private interface VPC endpoints, whichever are in use\. For example, if you create an interface VPC endpoint for CloudWatch, and you already have metrics flowing to CloudWatch from resources located on your VPC, these metrics begin flowing through the interface VPC endpoint by default\.

### Controlling access to your CloudWatch VPC endpoint<a name="CloudWatch-VPC-endpoint-policy"></a>

A VPC endpoint policy is an IAM resource policy that you attach to an endpoint when you create or modify the endpoint\. If you don't attach a policy when you create an endpoint, Amazon VPC attaches a default policy for you that allows full access to the service\. An endpoint policy doesn't override or replace IAM user policies or service\-specific policies\. It's a separate policy for controlling access from the endpoint to the specified service\. 

Endpoint policies must be written in JSON format\. 

For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

The following is an example of an endpoint policy for CloudWatch\. This policy allows users connecting to CloudWatch through the VPC to send metric data to CloudWatch and prevents them from performing other CloudWatch actions\.

```
{
  "Statement": [
    {
      "Sid": "PutOnly",
      "Principal": "*",
      "Action": [
        "cloudwatch:PutMetricData"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

**To edit the VPC endpoint policy for CloudWatch**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**\.

1. If you have not already created the endpoint for CloudWatch, choose **Create Endpoint**\. Select **com\.amazonaws\.*region*\.monitoring**, and then choose **Create endpoint**\.

1. Select the **com\.amazonaws\.*region*\.monitoring** endpoint, and then choose the **Policy** tab\.

1. Choose **Edit Policy**, and then make your changes\.

## CloudWatch Synthetics VPC endpoint<a name="cloudwatch-synthetics-interface-VPC"></a>

CloudWatch Synthetics currently supports VPC endpoints in the following AWS Regions:
+ US East \(Ohio\)
+ US East \(N\. Virginia\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ South America \(São Paulo\)

### Creating a VPC endpoint for CloudWatch Synthetics<a name="create-VPC-endpoint-for-CloudWatch-Synthetics"></a>

To start using CloudWatch Synthetics with your VPC, create an interface VPC endpoint for CloudWatch Synthetics\. The service name to choose is `com.amazonaws.region.synthetics`\. For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint.html) in the *Amazon VPC User Guide*\.

You do not need to change the settings for CloudWatch Synthetics\. CloudWatch Synthetics communicates with other AWS services using either public endpoints or private interface VPC endpoints, whichever are in use\. For example, if you create an interface VPC endpoint for CloudWatch Synthetics, and you already have an interface endpoint for Amazon S3, CloudWatch Synthetics begins communicating with Amazon S3 through the interface VPC endpoint by default\.

### Controlling access to your CloudWatch Synthetics VPC endpoint<a name="CloudWatch-Synthetics-VPC-endpoint-policy"></a>

A VPC endpoint policy is an IAM resource policy that you attach to an endpoint when you create or modify the endpoint\. If you don't attach a policy when you create an endpoint, we attach a default policy for you that allows full access to the service\. An endpoint policy doesn't override or replace IAM user policies or service\-specific policies\. It's a separate policy for controlling access from the endpoint to the specified service\. 

Endpoint policies affect canaries that are managed privately by VPC\. They are not needed for canaries that run on private subnets\.

Endpoint policies must be written in JSON format\. 

For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

The following is an example of an endpoint policy for CloudWatch Synthetics\. This policy enables users connecting to CloudWatch Synthetics through the VPC to view information about canaries and their runs, but not to create, modify, or delete canaries\.

```
{
    "Statement": [
        {
            "Action": [
                "synthetics:DescribeCanaries",
                "synthetics:GetCanaryRuns"
            ],
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        }
    ]
}
```

**To edit the VPC endpoint policy for CloudWatch Synthetics**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**\.

1. If you have not already created the endpoint for CloudWatch Synthetics, choose **Create Endpoint**\. Select **com\.amazonaws\.*region*\.synthetics** and then choose **Create endpoint**\.

1. Select the **com\.amazonaws\.*region*\.synthetics** endpoint and then choose the **Policy** tab\.

1. Choose **Edit Policy**, and then make your changes\.
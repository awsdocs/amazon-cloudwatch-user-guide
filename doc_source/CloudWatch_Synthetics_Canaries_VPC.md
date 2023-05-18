# Running a canary on a VPC<a name="CloudWatch_Synthetics_Canaries_VPC"></a>

You can run canaries on endpoints on a VPC and public internal endpoints\. To run a canary on a VPC, you must have both the **DNS Resolution** and ** DNS hostnames** options enabled on the VPC\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.

When you run a canary on a VPC endpoint, you must provide a way for it to send its metrics to CloudWatch and its artifacts to Amazon S3\. If the VPC is already enabled for internet access, there's nothing more for you to do\. The canary executes in your VPC, but can access the internet to upload its metrics and artifacts\.

If the VPC is not already enabled for internet access, you have two options:
+ Enable it for internet access\. For more information, see the following section [Giving internet access to your canary on a VPC](#CloudWatch_Synthetics_VPC_Internet)\.
+ If you want to keep your VPC private, you can configure the canary to send its data to CloudWatch and Amazon S3 through private VPC endpoints\. If you have not already done so, you must create endpoints for these services on your VPC\. For more information, see [Using CloudWatch and CloudWatch Synthetics with interface VPC endpoints](cloudwatch-and-interface-VPC.md) and [Amazon VPC Endpoints for Amazon S3](https://docs.aws.amazon.com/glue/latest/dg/vpc-endpoints-s3.html)\. 

## Giving internet access to your canary on a VPC<a name="CloudWatch_Synthetics_VPC_Internet"></a>

Follow these steps to give internet access to your VPC canary\.

**To give internet access to a canary on a VPC**

1. Create a NAT gateway in a public subnet on the VPC\. For instructions, see [Create a NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-creating)\.

1. Add a new route to the route table in the private subnet where the canary is launched\. Specify the following:
   + For **Destination**, enter **0\.0\.0\.0/0**
   + For **Target**, choose **NAT Gateway**, and then choose the ID of the NAT gateway that you created\.
   + Choose **Save routes**\.

   For more information about adding the route to the route table, see [Add and remove routes from a route table](https://docs.aws.amazon.com/vpc/latest/userguide/WorkWithRouteTables.html#AddRemoveRoutes)\.

**Note**  
Be sure that the routes to your NAT gateway are in an **active** status\. If the NAT gateway is deleted and you haven't updated the routes, they're in a blackhole status\. For more information, see [Work with NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-working-with)\.
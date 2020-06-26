# Infrastructure Security in Amazon CloudWatch<a name="infrastructure-security"></a>

As a managed service, Amazon CloudWatch is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Amazon CloudWatch through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Network Isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) is a virtual network in your own logically isolated area in the AWS cloud\. A subnet is a range of IP addresses in a VPC\. You can deploy a variety of AWS resources in the subnets of your VPCs\. For example, you can deploy Amazon EC2 instances, EMR clusters, and DynamoDB tables in subnets\. For more information, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

To enable CloudWatch to communicate with resources in a VPC without going through the public internet, use AWS PrivateLink\. For more information, see [Using CloudWatch and CloudWatch Synthetics with Interface VPC Endpoints](cloudwatch-and-interface-VPC.md)\.

A private subnet is a subnet with no default route to the public internet\. Deploying an AWS resource in a private subnet does not prevent Amazon CloudWatch from collecting built\-in metrics from the resource\.

If you need to publish custom metrics from an AWS resource in a private subnet, you can do so using a proxy server\. The proxy server forwards those HTTPS requests to the public API endpoints for CloudWatch\.
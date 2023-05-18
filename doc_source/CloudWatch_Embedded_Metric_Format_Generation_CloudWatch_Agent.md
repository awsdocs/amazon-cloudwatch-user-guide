# Using the CloudWatch agent to send embedded metric format logs<a name="CloudWatch_Embedded_Metric_Format_Generation_CloudWatch_Agent"></a>

To use this method, first install the CloudWatch agent for the services you want to send embedded metric format logs from, and then you can begin sending the events\.

The CloudWatch agent must be version 1\.230621\.0 or later\.

**Note**  
You do not need to install the CloudWatch agent to send logs from Lambda functions\.  
Lambda function timeouts are not handled automatically\. This means that if your function times out before the metrics get flushed, then the metrics for that invocation will not be captured\.

## Installing the CloudWatch agent<a name="CloudWatch_Embedded_Metric_Format_Generation_Install_Agent"></a>

Install the CloudWatch agent for each service which is to send embedded metric format logs\.

### Installing the CloudWatch agent on EC2<a name="CloudWatch_Embedded_Metric_Format_Generation_Install_Agent_EC2"></a>

First, install the CloudWatch agent on the instance\. For more information, see [Installing the CloudWatch agent](install-CloudWatch-Agent-on-EC2-Instance.md)\.

Once you have installed the agent, configure the agent to listen on a UDP or TCP port for the embedded metric format logs\. The following is an example of this configuration that listens on the default socket `tcp:25888`\. For more information about agent configuration, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

```
{
  "logs": {
    "metrics_collected": {
      "emf": { }
    }
  }
}
```

### Installing the CloudWatch agent on Amazon ECS<a name="CloudWatch_Embedded_Metric_Format_Generation_Install_Agent_ECS"></a>

The easiest way to deploy the CloudWatch agent on Amazon ECS is to run it as a sidecar, defining it in the same task definition as your application\.

**Create agent configuration file**

Create your CloudWatch agent configuration file locally\. In this example, the relative file path will be `amazon-cloudwatch-agent.json`\.

For more information about agent configuration, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

```
{
  "logs": {
    "metrics_collected": {
      "emf": { }
    }
  }
}
```

**Push configuration to SSM Parameter Store **

Enter the following command to push the CloudWatch agent configuration file to the AWS Systems Manager \(SSM\) Parameter Store\.

```
aws ssm put-parameter \
    --name "cwagentconfig" \
    --type "String" \
    --value "`cat amazon-cloudwatch-agent.json`" \
    --region "{{region}}"
```

**Configure the task definition**

Configure your task definition to use the CloudWatch Agent and expose the TCP or UDP port\. The sample task definition that you should use depends on your networking mode\.

Notice that the `webapp` specifies the `AWS_EMF_AGENT_ENDPOINT` environment variable\. This is used by the library and should point to the endpoint that the agent is listening on\. Additionally, the `cwagent` specifies the `CW_CONFIG_CONTENT` as a “valueFrom” parameter that points to the SSM configuration that you created in the previous step\.

This section contains one example for bridge mode and one example for host or awsvpc mode\. For more examples of how you can configure the CloudWatch agent on Amazon ECS, see the [Github samples repository](https://github.com/aws-samples/amazon-cloudwatch-container-insights/tree/master/ecs-task-definition-templates/deployment-mode/sidecar)

The following is an example for bridge mode\. When bridge mode networking is enabled, the agent needs to be linked to your application using the `links` parameter and must be addressed using the container name\.

```
{
  "containerDefinitions": [
          {
              "name": "webapp",
              "links": [ "cwagent" ],
              "image": "my-org/web-app:latest",
              "memory": 256,
              "cpu": 256,
              "environment": [{
                "name": "AWS_EMF_AGENT_ENDPOINT",
                "value": "tcp://cwagent:25888"
              }],
          },
          {
              "name": "cwagent",
              "mountPoints": [],
              "image": "public.ecr.aws/cloudwatch-agent/cloudwatch-agent:latest",
              "memory": 256,
              "cpu": 256,
              "portMappings": [{
                  "protocol": "tcp",
                  "containerPort": 25888
              }],
              "environment": [{
                "name": "CW_CONFIG_CONTENT",
                "valueFrom": "cwagentconfig"
              }],
          }
      ],
}
```

The following is an example for host mode or awsvpc mode\. When running on these network modes, the agent can be addressed over `localhost`\.

```
{
  "containerDefinitions": [
          {
              "name": "webapp",
              "image": "my-org/web-app:latest",
              "memory": 256,
              "cpu": 256,
              "environment": [{
                "name": "AWS_EMF_AGENT_ENDPOINT",
                "value": "tcp://127.0.0.1:25888"
              }],
          },
          {
              "name": "cwagent",
              "mountPoints": [],
              "image": "public.ecr.aws/cloudwatch-agent/cloudwatch-agent:latest",
              "memory": 256,
              "cpu": 256,
              "portMappings": [{
                  "protocol": "tcp",
                  "containerPort": 25888
              }],
              "environment": [{
                "name": "CW_CONFIG_CONTENT",
                "valueFrom": "cwagentconfig"
              }],
          }
      ],
}
```

**Note**  
In awsvpc mode, you must either give a public IP address to the VPC \(Fargate only\), set up a NAT gateway, or set up a CloudWatch Logs VPC endpoint\. For more information about setting up a NAT, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\. For more information about setting up a CloudWatch Logs VPC endpoint, see [Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html)\.  
The following is an example of how to assign a public IP address to a task that uses the Fargate launch type\.  

```
aws ecs run-task \ 
--cluster {{cluster-name}} \
--task-definition cwagent-fargate \
--region {{region}} \
--launch-type FARGATE \
--network-configuration "awsvpcConfiguration={subnets=[{{subnetId}}],securityGroups=[{{sgId}}],assignPublicIp=ENABLED}"
```

**Ensure permissions**

Ensure the IAM role executing your tasks has permission to read from the SSM Parameter Store\. You can add this permission by attaching the **AmazonSSMReadOnlyAccess** policy\. To do so, enter the following command\.

```
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMReadOnlyAccess \
--role-name CWAgentECSExecutionRole
```

### Installing the CloudWatch agent on Amazon EKS<a name="CloudWatch_Embedded_Metric_Format_Generation_Install_Agent_EKS"></a>

Parts of this process can be skipped if you have already installed CloudWatch Container Insights on this cluster\.

Permissions

If you have not already installed Container Insights, then first ensure that your Amazon EKS nodes have the appropriate IAM permissions\. They should have the **CloudWatchAgentServerPolicy** attached\. For more information, see [Verify prerequisites](Container-Insights-prerequisites.md)\.

**Create ConfigMap**

Create a ConfigMap for the agent\. The ConfigMap also tells the agent to listen on a TCP or UDP port\. Use the following ConfigMap\.

```
# cwagent-emf-configmap.yaml
apiVersion: v1
data:
  # Any changes here must not break the JSON format
  cwagentconfig.json: |
    {
      "agent": {
        "omit_hostname": true
      },
      "logs": {
        "metrics_collected": {
          "emf": { }
        }
      }
    }
kind: ConfigMap
metadata:
  name: cwagentemfconfig
  namespace: default
```

If you have already installed Container Insights, add the following `"emf": { }` line to your existing ConfigMap\.

**Apply the ConfigMap**

Enter the following command to apply the ConfigMap\.

```
kubectl apply -f cwagent-emf-configmap.yaml
```

**Deploy the agent**

To deploy the CloudWatch agent as a sidecar, add the agent to your pod definition, as in the following example\.

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp
  namespace: default
spec:
  containers:
    # Your container definitions go here
    - name: web-app
      image: my-org/web-app:latest
    # CloudWatch Agent configuration
    - name: cloudwatch-agent
      image: public.ecr.aws/cloudwatch-agent/cloudwatch-agent:latest
      imagePullPolicy: Always
      resources:
        limits:
          cpu: 200m
          memory: 100Mi
        requests:
          cpu: 200m
          memory: 100Mi
      volumeMounts:
        - name: cwagentconfig
          mountPath: /etc/cwagentconfig
      ports:
  # this should match the port configured in the ConfigMap
        - protocol: TCP
          hostPort: 25888
          containerPort: 25888
  volumes:
    - name: cwagentconfig
      configMap:
        name: cwagentemfconfig
```

## Using the CloudWatch agent to send embedded metric format logs<a name="CloudWatch_Embedded_Metric_Format_Generation_CloudWatch_Agent_Send_Logs"></a>

When you have the CloudWatch agent installed and running, you can send the embedded metric format logs over TCP or UDP\. There are two requirements when sending the logs over the agent:
+ The logs must contain a `LogGroupName` key that tells the agent which log group to use\.
+ Each log event must be on a single line\. In other words, a log event cannot contain the newline \(\\n\) character\.

The log events must also follow the embedded metric format specification\. For more information, see [Specification: Embedded metric format ](CloudWatch_Embedded_Metric_Format_Specification.md)\.

If you plan to create alarms on metrics created using embedded metric format, see [Setting alarms on metrics created with embedded metric format](CloudWatch_Embedded_Metric_Format_Alarms.md) for recommendations\.

The following is an example of sending log events manually from a Linux bash shell\. You can instead use the UDP socket interfaces provided by your programming language of choice\. 

```
echo '{"_aws":{"Timestamp":1574109732004,"LogGroupName":"Foo","CloudWatchMetrics":[{"Namespace":"MyApp","Dimensions":[["Operation"]],"Metrics":[{"Name":"ProcessingLatency","Unit":"Milliseconds","StorageResolution":60}]}]},"Operation":"Aggregator","ProcessingLatency":100}' \
> /dev/udp/0.0.0.0/25888
```

**Note**  
 With the embedded metric format, you can track the processing of your EMF logs by metrics that are published in the `AWS/Logs` namespace of your account\. These can be used to track failed metric generation from EMF, as well as whether failures happen due to parsing or validation\. For more details see [Monitoring with CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatch-Logs-Monitoring-CloudWatch-Metrics.html)\. 
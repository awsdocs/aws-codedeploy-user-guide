# Use CodeDeploy with Amazon Virtual Private Cloud<a name="vpc-endpoints"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC and CodeDeploy\. You can use this connection to enable CodeDeploy to communicate with your resources on your VPC without going through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. With VPC endpoints, the routing between the VPC and AWS services is handled by the AWS network, and you can use IAM policies to control access to service resources\.

To connect your VPC to CodeDeploy, you define an *interface VPC endpoint* for CodeDeploy\. An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported AWS service\. The endpoint provides reliable, scalable connectivity to CodeDeploy without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [What Is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/) in the *Amazon VPC User Guide*\.

 Interface VPC endpoints are powered by AWS PrivateLink, an AWS technology that enables private communication between AWS services using an elastic network interface with private IP addresses\. For more information, see [AWS PrivateLink](https://aws.amazon.com/privatelink/)\.

The following steps are for users of Amazon VPC\. For more information, see [Getting Started](https://docs.aws.amazon.com/vpc/latest/userguide/GetStarted.html) in the *Amazon VPC User Guide*\.

## Availability<a name="codecommit-interface-VPC-availability"></a>

CodeDeploy has two VPC endpoints: one for CodeDeploy agent operations, and one for CodeDeploy API operations\. The table below shows the supported AWS Regions for each endpoint\.


| Region name | Region code | Agent endpoint | API endpoint | 
| --- | --- | --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  |  Yes  |  Yes  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  |  Yes  |  Yes  | 
|  US West \(N\. California\)  |  us\-west\-1  |  Yes  |  Yes  | 
|  US West \(Oregon\)  |  us\-west\-2  |  Yes  |  Yes  | 
|  Africa \(Cape Town\)  |  af\-south\-1  |  Yes  |  No  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  |  Yes  |  Yes  | 
|  Asia Pacific \(Hyderabad\)  |  ap\-south\-2  |  No  |  No  | 
|  Asia Pacific \(Jakarta\)  |  ap\-southeast\-3  |  Yes  |  No  | 
|  Asia Pacific \(Melbourne\)  |  ap\-southeast\-4  |  No  |  No  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  |  Yes  |  Yes  | 
|  Asia Pacific \(Osaka\)  |  ap\-northeast\-3  |  Yes  |  No  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  |  Yes  |  Yes  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  |  Yes  |  Yes  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  |  Yes  |  Yes  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  |  Yes  |  Yes  | 
|  Canada \(Central\)  |  ca\-central\-1  |  Yes  |  Yes  | 
|  China \(Beijing\)  |  cn\-north\-1  |  Yes  |  No  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  |  No  |  No  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  |  Yes  |  Yes  | 
|  Europe \(Ireland\)  |  eu\-west\-1  |  Yes  |  Yes  | 
|  Europe \(London\)  |  eu\-west\-2  |  Yes  |  Yes  | 
|  Europe \(Milan\)  |  eu\-south\-1  |  No  |  No  | 
|  Europe \(Paris\)  |  eu\-west\-3  |  Yes  |  Yes  | 
|  Europe \(Spain\)  |  eu\-south\-2  |  No  |  No  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  |  Yes  |  Yes  | 
|  Europe \(Zurich\)  |  eu\-central\-2  |  No  |  No  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  |  Yes  |  Yes  | 
|  Middle East \(UAE\)  |  me\-central\-1  |  Yes  |  No  | 
|  South America \(São Paulo\)  |  sa\-east\-1  |  Yes  |  Yes  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  |  No  |  No  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  |  No  |  No  | 

## Create VPC endpoints for CodeDeploy<a name="create-vpc-endpoint-for-codedeploy"></a>

To start using CodeDeploy with your VPC, create an interface VPC endpoint for CodeDeploy\. CodeDeploy requires separate endpoints for agent Git operations and for CodeDeploy API operations\. Depending on your business needs, you might need to create more than one VPC endpoint\. When you create a VPC endpoint for CodeDeploy, choose **AWS Services**, and in **Service Name**, choose from the following options:
+  **com\.amazonaws\.*region*\.codedeploy**: Choose this option if you want to create a VPC endpoint for CodeDeploy API operations\. For example, choose this option if your users use the AWS CLI, the CodeDeploy API, or the AWS SDKs to interact with CodeDeploy for operations such as `CreateApplication`, `GetDeployment`, and `ListDeploymentGroups`\. 
+  **com\.amazonaws\.*region*\.codedeploy\-commands\-secure**: Choose this option if you want to create a VPC endpoint for CodeDeploy agent operations\. You will also need to set `:enable_auth_policy:` to `true` in your agent configuration file and attach the required permissions\. For more information, see [Configure the CodeDeploy agent and IAM permissions](#vpc-codedeploy-agent-configuration)\. 

If you are using Lambda or ECS deployments, you only need to create a VPC endpoint for **com\.amazonaws\.*region*\.codedeploy**\. Customers using Amazon EC2 deployments will need VPC endpoints for both **com\.amazonaws\.*region*\.codedeploy** and **com\.amazonaws\.*region*\.codedeploy\-commands\-secure**\. 

## Configure the CodeDeploy agent and IAM permissions<a name="vpc-codedeploy-agent-configuration"></a>

To use Amazon VPC endpoints with CodeDeploy, you must set the value of `:enable_auth_policy:` to `true` in the agent configuration file located on your EC2 or on\-premises instances\. For more information on the agent configuration file, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\.

You must also add the following IAM permissions to your Amazon EC2 instance profile or IAM user or role if you are using on\-premises instances\.

```
{
  "Statement": [
    {
      "Action": [
        "codedeploy-commands-secure:GetDeploymentSpecification",
        "codedeploy-commands-secure:PollHostCommand",
        "codedeploy-commands-secure:PutHostCommandAcknowledgement",
        "codedeploy-commands-secure:PutHostCommandComplete"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint.html) in the *Amazon VPC User Guide*\.
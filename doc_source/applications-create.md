# Create an application with CodeDeploy<a name="applications-create"></a>

An *application* is simply a name or container used by CodeDeploy to ensure that the correct revision, deployment configuration, and deployment group are referenced during a deployment\. You can use the CodeDeploy console, the AWS CLI, the CodeDeploy APIs, or an AWS CloudFormation template to create applications\.

Your code, or application revision, is installed to instances through a process called a deployment\. CodeDeploy supports two types of deployments: 
+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an in\-place deployment](welcome.md#welcome-deployment-overview-in-place)\.
+ **Blue/green deployment**: The behavior of your deployment depends on which compute platform you use:
  + **Blue/green on an EC2/On\-Premises compute platform**: The instances in a deployment group \(the original environment\) are replaced by a different set of instances \(the replacement environment\) using these steps:
    + Instances are provisioned for the replacement environment\.
    + The latest application revision is installed on the replacement instances\.
    + An optional wait time occurs for activities such as application testing and system verification\.
    + Instances in the replacement environment are registered with an Elastic Load Balancing load balancer, causing traffic to be rerouted to them\. Instances in the original environment are deregistered and can be terminated or kept running for other uses\.
**Note**  
If you use an EC2/On\-Premises compute platform, be aware that blue/green deployments work with Amazon EC2 instances only\.
  + **Blue/green on an AWS Lambda compute platform**: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions\. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shifting occurs\. All AWS Lambda compute platform deployments are blue/green deployments\. For this reason, you do not need to specify a deployment type\. 
  + **Blue/green on an Amazon ECS compute platform**: Traffic is shifted from the task set with the original version of an application in an Amazon ECS service to a replacement task set in the same service\. You can set the traffic shifting to linear or canary through the deployment configuration\. The protocol and port of a specified load balancer listener is used to reroute production traffic\. During a deployment, a test listener can be used to serve traffic to the replacement task set while validation tests are run\. 
  + **Blue/green deployments through AWS CloudFormation**: Traffic is shifted from your current resources to your updated resources as part of an AWS CloudFormation stack update\. Currently, only ECS blue/green deployments are supported\. 

  For more information about blue/green deployments, see [Overview of a blue/green deployment](welcome.md#welcome-deployment-overview-blue-green)\.

When you use the CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

To view a list of applications already registered to your AWS account, see [View application details with CodeDeploy](applications-view-details.md)\. For information about using an AWS CloudFormation template to create an application, see [AWS CloudFormation templates for CodeDeploy reference](reference-cloudformation-templates.md)\.

 Both deployment types do not apply to all destinations\. The following table lists which deployment types work with deployments to the three types of deployment destinations\.


****  

| Deployment destination | In\-place | Blue/green | 
| --- | --- | --- | 
| Amazon EC2  | Yes | Yes | 
| On\-premises | Yes | No | 
| Serverless AWS Lambda functions | No | Yes | 
| Amazon ECS applications | No | Yes | 

**Topics**
+ [Create an application for an in\-place deployment \(console\)](applications-create-in-place.md)
+ [Create an application for a blue/green deployment \(console\)](applications-create-blue-green.md)
+ [Create an application for an Amazon ECS service deployment \(console\)](applications-create-ecs.md)
+ [Create an application for an AWS Lambda function deployment \(console\)](applications-create-lambda.md)
+ [Create an application \(CLI\)](applications-create-cli.md)
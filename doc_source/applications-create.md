# Create an Application with CodeDeploy<a name="applications-create"></a>

An *application* is simply a name or container used by CodeDeploy to ensure that the correct revision, deployment configuration, and deployment group are referenced during a deployment\. You can use the CodeDeploy console, the AWS CLI, the CodeDeploy APIs, or an AWS CloudFormation template to create applications\.

Your code, or application revision, is installed to instances through a process called a deployment\. CodeDeploy supports two types of deployments: 
+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an In\-Place Deployment](welcome.md#welcome-deployment-overview-in-place)\.
+ **Blue/green deployment**: The behavior of your deployment depends on which compute platform you use:
  + **Blue/green on an EC2/On\-Premises compute platform**: The instances in a deployment group \(the original environment\) are replaced by a different set of instances \(the replacement environment\) using these steps:
    + Instances are provisioned for the replacement environment\.
    + The latest application revision is installed on the replacement instances\.
    + An optional wait time occurs for activities such as application testing and system verification\.
    + Instances in the replacement environment are registered with an Elastic Load Balancing load balancer, causing traffic to be rerouted to them\. Instances in the original environment are deregistered and can be terminated or kept running for other uses\.
**Note**  
If you use an EC2/On\-Premises compute platform, be aware that blue/green deployments work with Amazon EC2 instances only\.
  + **Blue/green on an AWS Lambda compute platform**: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions\. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs\. All AWS Lambda compute platform deployments are blue/green deployments\. For this reason, you do not need to specify a deployment type\. 
  + **Blue/green on an Amazon ECS compute platform**: Traffic is shifted from the task set with the original version of a containerized application in an Amazon ECS service to a replacement task set in the same service\. The protocol and port of a specified load balancer listener is used to reroute production traffic\. During a deployment, a test listener can be used to serve traffic to the replacement task set while validation tests are run\. 

  For more information about blue/green deployments, see [Overview of a Blue/Green Deployment](welcome.md#welcome-deployment-overview-blue-green)\.

When you use the CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

To view a list of applications already registered to your AWS account, see [View Application Details with CodeDeploy](applications-view-details.md)\. For information about using an AWS CloudFormation template to create an application, see [AWS CloudFormation Templates for CodeDeploy Reference](reference-cloudformation-templates.md)\.

 Both deployment types do not apply to all destinations\. The following table lists which deployment types work with deployments to the three types of deployment destinations\.


****  

| Deployment destination | In\-place | Blue/green | 
| --- | --- | --- | 
| Amazon EC2  | Yes | Yes | 
| On\-premises | Yes | No | 
| Serverless AWS Lambda functions | No | Yes | 
| Amazon ECS applications | No | Yes | 

**Topics**
+ [Create an Application for an In\-Place Deployment \(Console\)](applications-create-in-place.md)
+ [Create an Application for a Blue/Green Deployment \(Console\)](applications-create-blue-green.md)
+ [Create an Application for an Amazon ECS Service Deployment \(Console\)](applications-create-ecs.md)
+ [Create an Application for an AWS Lambda Function Deployment \(Console\)](applications-create-lambda.md)
+ [Create an Application \(CLI\)](applications-create-cli.md)
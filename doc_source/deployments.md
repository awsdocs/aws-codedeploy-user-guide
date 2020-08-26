# Working with deployments in CodeDeploy<a name="deployments"></a>

In CodeDeploy, a deployment is the process, and the components involved in the process, of installing content on one or more instances\. This content can consist of code, web and configuration files, executables, packages, scripts, and so on\. CodeDeploy deploys content that is stored in a source repository, according to the configuration rules you specify\.

 If you use the EC2/On\-Premises compute platform, then two deployments to the same set of instances can run concurrently\. 

CodeDeploy provides two deployment type options, in\-place deployments and blue/green deployments\.
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

For information about automatically deploying from Amazon S3, see [Automatically deploy from Amazon S3 using CodeDeploy](http://aws.amazon.com/blogs/devops/automatically-deploy-from-amazon-s3-using-aws-codedeploy/)\.

**Topics**
+ [Create a deployment](deployments-create.md)
+ [View deployment details](deployments-view-details.md)
+ [View deployment log data](deployments-view-logs.md)
+ [Stop a deployment](deployments-stop.md)
+ [Redeploy and roll back a deployment](deployments-rollback-and-redeploy.md)
+ [Deploy an application in a different AWS account](deployments-cross-account.md)
+ [Validate a deployment package on a local machine](deployments-local.md)
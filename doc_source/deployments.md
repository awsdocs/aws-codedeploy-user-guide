# Working with Deployments in AWS CodeDeploy<a name="deployments"></a>

In AWS CodeDeploy, a deployment is the process, and the components involved in the process, of installing content on one or more instances\. This content can consist of code, web and configuration files, executables, packages, scripts, and so on\. AWS CodeDeploy deploys content that is stored in a source repository, according to the configuration rules you specify\.

AWS CodeDeploy provides two deployment type options, in\-place deployments and blue/green deployments\.

+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an In\-Place Deployment](welcome.md#welcome-deployment-overview-in-place)\.

+ **Blue/green deployment**: The behavior of your deployment depends on which compute platform you use:

  + **Blue/green on an EC2/On\-Premises compute platform**: The instances in a deployment group \(the original environment\) are replaced by a different set of instances \(the replacement environment\) using these steps:

    + Instances are provisioned for the replacement environment\.

    + The latest application revision is installed on the replacement instances\.

    + An optional wait time occurs for activities such as application testing and system verification\.

    + Instances in the replacement environment are registered with an Elastic Load Balancing load balancer, causing traffic to be rerouted to them\. Instances in the original environment are deregistered and can be terminated or kept running for other uses\.
**Note**  
When using an EC2/On\-Premises compute platform, blue/green deployments work with Amazon EC2 instances only\.

  + **Blue/green on an AWS Lambda compute platform**: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions\. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs\. All AWS Lambda compute platform deployments are blue/green deployments\. For this reason, you do not need to specify a deployment type\. 

  For more information about blue/green deployments, see [Overview of a Blue/Green Deployment](welcome.md#welcome-deployment-overview-blue-green)\.

For information about automatically deploying from Amazon S3, see [Automatically Deploy from Amazon S3 Using AWS CodeDeploy](http://aws.amazon.com/blogs/devops/automatically-deploy-from-amazon-s3-using-aws-codedeploy/)\.


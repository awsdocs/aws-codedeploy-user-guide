# What Is AWS CodeDeploy?<a name="welcome"></a>

AWS CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances, on\-premises instances, or serverless Lambda functions\.

You can deploy a nearly unlimited variety of application content, such as code, serverless AWS Lambda functions, web and configuration files, executables, packages, scripts, multimedia files, and so on\. AWS CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories\. AWS CodeDeploy can also deploy a serverless Lambda function\. You do not need to make changes to your existing code before you can use AWS CodeDeploy\. 

AWS CodeDeploy makes it easier for you to:

+ Rapidly release new features\.

+ Update AWS Lambda function versions\.

+ Avoid downtime during application deployment\.

+ Handle the complexity of updating your applications, without many of the risks associated with error\-prone manual deployments\.

The service scales with your infrastructure so you can easily deploy to one instance or thousands\.

AWS CodeDeploy works with various systems for configuration management, source control, [continuous integration](https://aws.amazon.com/devops/continuous-integration/), [continuous delivery](https://aws.amazon.com/devops/continuous-delivery/), and continuous deployment\. For more information, see [Product Integrations](https://aws.amazon.com/codedeploy/product-integrations/)\.


+ [Video Introduction to AWS CodeDeploy](#intro-video-welcome)
+ [Benefits of AWS CodeDeploy](#benefits)
+ [Overview of AWS CodeDeploy Compute Platforms](#compute-platform)
+ [Overview of AWS CodeDeploy Deployment Types](#welcome-deployment-overview)
+ [We Want to Hear from You](#welcome-contact-us)
+ [Primary Components](primary-components.md)
+ [Deployments](deployment-steps.md)
+ [Application Specification Files](application-specification-files.md)

## Video Introduction to AWS CodeDeploy<a name="intro-video-welcome"></a>

This short video \(2:10\) describes how AWS CodeDeploy automates code deployments to Amazon EC2 instances, making it easier for you to rapidly release new features, eliminate downtime during deployment, and avoid the need for error\-prone, manual operations\.

[![AWS Videos](http://img.youtube.com/vi/Wx-ain8UryM/0.jpg)](http://www.youtube.com/watch?v=Wx-ain8UryM)

## Benefits of AWS CodeDeploy<a name="benefits"></a>

AWS CodeDeploy offers these benefits:

+ **Server and serverless applications**\. AWS CodeDeploy lets you deploy both traditional applications on servers and applications that deploy a serverless AWS Lambda function version\.

+ **Automated deployments**\. AWS CodeDeploy fully automates your application deployments across your development, test, and production environments\. AWS CodeDeploy scales with your infrastructure so that you can deploy to one instance or thousands\.

+ **Minimize downtime**\. AWS CodeDeploy helps maximize your application availability\. During an in\-place deployment, AWS CodeDeploy performs a rolling update across Amazon EC2 instances\. You can specify the number of instances to be taken offline at a time for updates\. During a blue/green deployment, the latest application revision is installed on replacement instances, and traffic is rerouted to these instances when you choose, either immediately or as soon as you are done testing the new environment\. For both deployment types, AWS CodeDeploy tracks application health according to rules you configure\. 

+ **Stop and roll back**\. You can automatically or manually stop and roll back deployments if there are errors\. 

+ **Centralized control**\. You can launch and track the status of your deployments through the AWS CodeDeploy console or the AWS CLI\. You receive a report that lists when each application revision was deployed and to which Amazon EC2 instances\. 

+ **Easy to adopt**\. AWS CodeDeploy is platform\-agnostic and works with any application\. You can easily reuse your setup code\. AWS CodeDeploy can also integrate with your software release process or continuous delivery toolchain\.

## Overview of AWS CodeDeploy Compute Platforms<a name="compute-platform"></a>

AWS CodeDeploy is able to deploy applications to two compute platforms:

+ **EC2/On\-Premises**: Describes instances of physical servers that can be Amazon EC2 cloud instances, on\-premises servers, or both\. Applications created using the EC2/On\-Premises compute platform can be composed of executable files, configuration files, images, and more\.

  Delpoyments that use the EC2/On\-Premises compute platform manage the way in which traffic is directed to instances by using an in\-place or blue/green deployment type\. For more information, see [Overview of AWS CodeDeploy Deployment Types](#welcome-deployment-overview)\.

+ **AWS Lambda**: Used to deploy applications that consist of updated versions of Lambda functions\. AWS Lambda manages the Lambda functions in a serverless compute environment made up of a high\-availability compute structure\. All administration of the compute resources is performed by AWS Lambda\. For more information, see [Serverless Computing and Applications](aws.amazon.comserverless/)\. For more information about AWS Lambda and Lambda functions, see [AWS Lambda](aws.amazon.com/lambda/)\.

  Applications created using the AWS Lambda compute platform can manage the way in which traffic is directed to the updated Lambda function versions during a deployment by choosing a canary, linear, or all\-at\-once configuration\. 
**Important**  
 AWS CodeDeploy currently cannot deploy to the AWS Lambda compute platform in the EU \(Paris\) Region \(eu\-west\-3\)\.

The following table describes how AWS CodeDeploy components are used with each compute platform\. 


| AWS CodeDeploy Component | EC2/On\-Premises | AWS Lambda | 
| --- | --- | --- | 
| Deployment group | Deploys a  set of instances to which a new revision is deployed\. | Deploys a Lambda function version on a high\-availability compute infrastructure\. | 
| Deployment | Deploys a new revision that consists of an application and AppSpec file\. The AppSpec specifies how to deploy the application to the instances in a deployment group\. | Deploys a new revision that consists of an AppSpec file\. The AppSpec specifies which Lambda function version to deploy\. | 
| Deployment configuration | Settings that determine the deployment speed and the minimum number of instances that must be healthy at any point during a deployment\. | Settings that determine how traffic is shifted to the updated Lambda function versions\. | 
| Revision | A combination of an AppSpec file and application files, such as executables, configuration files, and so on\. | An AppSpec file that specifies which Lambda functions to deploy and update\. | 
| Application | A collection of deployment groups and revisions\. An EC2/On\-Premises application uses the EC2/On\-Premises compute platform\. | A collection of revisions\. A Lambda application uses the AWS Lambda compute platform\. | 

## Overview of AWS CodeDeploy Deployment Types<a name="welcome-deployment-overview"></a>

AWS CodeDeploy provides two deployment type options:

+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an In\-Place Deployment](#welcome-deployment-overview-in-place)\.
**Note**  
AWS Lambda compute platform deployments cannot use an in\-place deployment type\.

+ **Blue/green deployment**: The behavior of your deployment depends on which compute platform you use:

  + **Blue/green on an EC2/On\-Premises compute platform**: The instances in a deployment group \(the original environment\) are replaced by a different set of instances \(the replacement environment\) using these steps:

    + Instances are provisioned for the replacement environment\.

    + The latest application revision is installed on the replacement instances\.

    + An optional wait time occurs for activities such as application testing and system verification\.

    + Instances in the replacement environment are registered with an Elastic Load Balancing load balancer, causing traffic to be rerouted to them\. Instances in the original environment are deregistered and can be terminated or kept running for other uses\.
**Note**  
When using an EC2/On\-Premises compute platform, blue/green deployments work with Amazon EC2 instances only\.

  + **Blue/green on an AWS Lambda compute platform**: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions\. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs\. All AWS Lambda compute platform deployments are blue/green deployments\. For this reason, you do not need to specify a deployment type\. 

  For more information about blue/green deployments, see [Overview of a Blue/Green Deployment](#welcome-deployment-overview-blue-green)\.

**Note**  
Using the AWS CodeDeploy agent, you can perform a deployment on an instance you are logged on to without the need for an application, deployment group, or even an AWS account\. For information, see [Use the AWS CodeDeploy Agent to Validate a Deployment Package on a Local Machine](deployments-local.md)\.


+ [Overview of an In\-Place Deployment](#welcome-deployment-overview-in-place)
+ [Overview of a Blue/Green Deployment](#welcome-deployment-overview-blue-green)

### Overview of an In\-Place Deployment<a name="welcome-deployment-overview-in-place"></a>

The following diagram shows the flow of a typical AWS CodeDeploy in\-place deployment\.

**Note**  
AWS Lambda compute platform deployments cannot use an in\-place deployment type\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/sds_architecture.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

Here's how it works:

1. First, you create deployable content on your local development machine or similar environment, and then you add an *application specification file* \(AppSpec file\)\. The AppSpec file is unique to AWS CodeDeploy\. It defines the deployment actions you want AWS CodeDeploy to execute\. You bundle your deployable content and the AppSpec file into an archive file, and then upload it to an Amazon S3 bucket or a GitHub repository\. This archive file is called an *application revision* \(or simply a *revision*\)\.

1. Next, you provide AWS CodeDeploy with information about your deployment, such as which Amazon S3 bucket or GitHub repository to pull the revision from and to which set of Amazon EC2 instances to deploy its contents\. AWS CodeDeploy calls a set of Amazon EC2 instances a *deployment group*\. A deployment group contains individually tagged Amazon EC2 instances, Amazon EC2 instances in Auto Scaling groups, or both\.

   Each time you successfully upload a new application revision that you want to deploy to the deployment group, that bundle is set as the *target revision* for the deployment group\. In other words, the application revision that is currently targeted for deployment is the target revision\. This is also the revision that will be pulled for automatic deployments\.

1. Next, the AWS CodeDeploy agent on each instance polls AWS CodeDeploy to determine what and when to pull from the specified Amazon S3 bucket or GitHub repository\.

1. Finally, the AWS CodeDeploy agent on each instance pulls the target revision from the specified Amazon S3 bucket or GitHub repository and, using the instructions in the AppSpec file, deploys the contents to the instance\.

 AWS CodeDeploy keeps a record of your deployments so that you can get deployment status, deployment configuration parameters, instance health, and so on\.

### Overview of a Blue/Green Deployment<a name="welcome-deployment-overview-blue-green"></a>

A blue/green deployment, in which traffic is rerouted from one set of instances \(the original environment\) to a different set \(the replacement environment\), offers a number of advantages over an in\-place deployment:

+ An application can be installed and tested on the new instances ahead of time and deployed to production simply by switching traffic to the new servers\.

+  Switching back to the most recent version of an application is faster and more reliable because traffic can be routed back to the original instances as long as they have not been terminated\. With an in\-place deployment, versions must be rolled back by redeploying the previous version of the application\.

+ If you're using the EC2/On\-Premises compute platform,new instances are provisioned for a blue/green deployment and reflect the most up\-to\-date server configurations\. This helps you avoid the types of problems that sometimes occur on long\-running instances\.

+ If you're using the AWS Lambda compute platform,you control how traffic is shifted from your original AWS Lambda function versions to your new AWS Lambda function versions\.

How you configure a blue/green deployment depends on which compute platform your deployment is using\.

#### Blue/Green Deployment on an AWS Lambda Compute Platform<a name="blue-green-lambda-compute-type"></a>

If you're using the AWS Lambda compute platform, you must choose one of the following deployment configuration types to specify how traffic is shifted from the original AWS Lambda function version to the new AWS Lambda function version:

+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 

+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each incrememnt and the number of minutes between each increment\.

+ **All\-at\-once**: All traffic is shifted from the original Lambda function to the updated Lambda function version at once\.

For more information about AWS Lambda deployment configurations, see [Predefined Deployment Configurations for an AWS Lambda Compute Platform ](deployment-configurations.md#deployment-configurations-predefined-lambda)\.

#### Blue/Green Deployment on an EC2/On\-Premises Compute Platform<a name="blue-green-server-compute-type"></a>

**Note**  
You must use Amazon EC2 instances for blue/green deployments on the EC2/On\-Premises compute platform\. On\-premises instances are not supported for the blue/green deployment type\.

If you're using the EC2/On\-Premises compute platform, the following applies:

 You must have one or more Amazon EC2 instances with identifying Amazon EC2 tags or an Auto Scaling group\. The instances must meet these additional requirements:

+ Each Amazon EC2 instance must have the correct IAM instance profile attached\.

+ The AWS CodeDeploy agent must be installed and running on each instance\.

**Note**  
You typically also have an application revision running on the instances in your original environment, but this is not a requirement for a blue/green deployment\.

When you create a deployment group that is used in blue/green deployments, you can choose how your replacement environment is specified:

**Copy an existing Auto Scaling group**: During the blue/green deployment, AWS CodeDeploy creates the instances for your replacement environment automatically during the deployment\. With this option, AWS CodeDeploy uses the Auto Scaling group you specify as a template for the replacement environment, including the same number of running instances and many other configuration options\.

**Choose instances manually**: You can specify the instances to be counted as your replacement using Amazon EC2 instance tags, Auto Scaling group names, or both\. If you choose this option, you do not need to specify the instances for the replacement environment until you create a deployment\.

Here's how it works:

1. You already have instances or an Auto Scaling group that will serve as your original environment\. The first time you run a blue/green deployment, you'll typically use instances that were already used in  an in\-place deployment\.

1. In an existing AWS CodeDeploy application, you create a blue/green deployment group where, in addition to the options required for an in\-place deployment, you specify the following:

   + The load balancer that will route traffic from your original environment to your replacement environment during the blue/green deployment process\.

   + Whether to reroute traffic to the replacement environment immediately or wait for you to reroute it manually\. 

   + The rate at which traffic is routed to the replacement instances\.

   + Whether the instances that are replaced are terminated or kept running\.

1. You create a deployment for this deployment group during which the following occur:

   1. If you chose to copy an Auto Scaling group, instances are provisioned for your replacement environment\.

   1. The application revision you specify for the deployment is installed on the replacement instances\.

   1. If you specified a wait time in the deployment group settings, the deployment is paused\. This is the time when you can run tests and verifications in your replacement environment\. If you don't manually reroute the traffic before the end of the wait period, the deployment is stopped\.

   1. Instances in the replacement environment are registered with an Elastic Load Balancing load balancer and traffic begins to be routed to them\.

   1. Instances in the original environment are deregistered and handled according to your specification in the deployment group, either terminated or kept running\.

## We Want to Hear from You<a name="welcome-contact-us"></a>

We welcome your feedback\. To contact us, visit [the AWS CodeDeploy forum](https://forums.aws.amazon.com//forum.jspa?forumID=179)\.

**Topics**

+ [Primary Components](primary-components.md)

+ [Deployments](deployment-steps.md)

+ [Application Specification Files](application-specification-files.md)
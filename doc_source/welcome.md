# What is CodeDeploy?<a name="welcome"></a>

CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances, on\-premises instances, serverless Lambda functions, or Amazon ECS services\.

You can deploy a nearly unlimited variety of application content, including:
+ Code
+ Serverless AWS Lambda functions
+ Web and configuration files
+ Executables
+ Packages
+ Scripts
+ Multimedia files

CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories\. CodeDeploy can also deploy a serverless Lambda function\. You do not need to make changes to your existing code before you can use CodeDeploy\. 

CodeDeploy makes it easier for you to:
+ Rapidly release new features\.
+ Update AWS Lambda function versions\.
+ Avoid downtime during application deployment\.
+ Handle the complexity of updating your applications, without many of the risks associated with error\-prone manual deployments\.

The service scales with your infrastructure so you can easily deploy to one instance or thousands\.

CodeDeploy works with various systems for configuration management, source control, [continuous integration](https://aws.amazon.com/devops/continuous-integration/), [continuous delivery](https://aws.amazon.com/devops/continuous-delivery/), and continuous deployment\. For more information, see [Product integrations](https://aws.amazon.com/codedeploy/product-integrations/)\.

 The CodeDeploy console also provides a way to quickly search for your resources, such as repositories, build projects, deployment applications, and pipelines\. Choose **Go to resource** or press the `/` key, and then type the name of the resource\. Any matches appear in the list\. Searches are case insensitive\. You only see resources that you have permissions to view\. For more information, see [Identity and access management for AWS CodeDeploy](security-iam.md)\. 

**Topics**
+ [Video introduction to AWS CodeDeploy](#intro-video-welcome)
+ [Benefits of AWS CodeDeploy](#benefits)
+ [Overview of CodeDeploy compute platforms](#compute-platform)
+ [Overview of CodeDeploy deployment types](#welcome-deployment-overview)
+ [We want to hear from you](#welcome-contact-us)
+ [Primary Components](primary-components.md)
+ [Deployments](deployment-steps.md)
+ [Application Specification Files](application-specification-files.md)

## Video introduction to AWS CodeDeploy<a name="intro-video-welcome"></a>

This short video \(2:10\) describes how CodeDeploy automates code deployments to Amazon EC2 instances\.

[![AWS Videos](http://img.youtube.com/vi/Wx-ain8UryM/0.jpg)](http://www.youtube.com/watch?v=Wx-ain8UryM)

## Benefits of AWS CodeDeploy<a name="benefits"></a>

CodeDeploy offers these benefits:
+ **Server, serverless, and container applications**\. CodeDeploy lets you deploy both traditional applications on servers and applications that deploy a serverless AWS Lambda function version or an Amazon ECS application\.
+ **Automated deployments**\. CodeDeploy fully automates your application deployments across your development, test, and production environments\. CodeDeploy scales with your infrastructure so that you can deploy to one instance or thousands\.
+ **Minimize downtime**\. If your application uses the EC2/On\-Premises compute platform, CodeDeploy helps maximize your application availability\. During an in\-place deployment, CodeDeploy performs a rolling update across Amazon EC2 instances\. You can specify the number of instances to be taken offline at a time for updates\. During a blue/green deployment, the latest application revision is installed on replacement instances\. Traffic is rerouted to these instances when you choose, either immediately or as soon as you are done testing the new environment\. For both deployment types, CodeDeploy tracks application health according to rules you configure\. 
+ **Stop and roll back**\. You can automatically or manually stop and roll back deployments if there are errors\. 
+ **Centralized control**\. You can launch and track the status of your deployments through the CodeDeploy console or the AWS CLI\. You receive a report that lists when each application revision was deployed and to which Amazon EC2 instances\. 
+ **Easy to adopt**\. CodeDeploy is platform\-agnostic and works with any application\. You can easily reuse your setup code\. CodeDeploy can also integrate with your software release process or continuous delivery toolchain\.
+ **Concurrent deployments**\. If you have more than one application that uses the EC2/On\-Premises compute platform, CodeDeploy can deploy them concurrently to the same set of instances\.

## Overview of CodeDeploy compute platforms<a name="compute-platform"></a>

CodeDeploy is able to deploy applications to three compute platforms:
+ **EC2/On\-Premises**: Describes instances of physical servers that can be Amazon EC2 cloud instances, on\-premises servers, or both\. Applications created using the EC2/On\-Premises compute platform can be composed of executable files, configuration files, images, and more\.

  Deployments that use the EC2/On\-Premises compute platform manage the way in which traffic is directed to instances by using an in\-place or blue/green deployment type\. For more information, see [Overview of CodeDeploy deployment types](#welcome-deployment-overview)\.
+ **AWS Lambda**: Used to deploy applications that consist of an updated version of a Lambda function\. AWS Lambda manages the Lambda function in a serverless compute environment made up of a high\-availability compute structure\. All administration of the compute resources is performed by AWS Lambda\. For more information, see [Serverless Computing and Applications](https://aws.amazon.com/serverless/)\. For more information about AWS Lambda and Lambda functions, see [AWS Lambda](https://aws.amazon.com/lambda/)\.

  You can manage the way in which traffic is shifted to the updated Lambda function versions during a deployment by choosing a canary, linear, or all\-at\-once configuration\. 
+ **Amazon ECS**: Used to deploy an Amazon ECS containerized application as a task set\. CodeDeploy performs a blue/green deployment by installing an updated version of the application as a new replacement task set\. CodeDeploy reroutes production traffic from the original application task set to the replacement task set\. The original task set is terminated after a successful deployment\. For more information about Amazon ECS, see [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)\.

  You can manage the way in which traffic is shifted to the updated task set during a deployment by choosing a canary, linear, or all\-at\-once configuration\.
**Note**  
Amazon ECS blue/green deployments are supported using both CodeDeploy and AWS CloudFormation\. Details for these deployments are described in subsequent sections\.

The following table describes how CodeDeploy components are used with each compute platform\. For more information, see: 
+  [Working with deployment groups in CodeDeploy](deployment-groups.md) 
+  [Working with deployments in CodeDeploy](deployments.md) 
+  [Working with deployment configurations in CodeDeploy](deployment-configurations.md) 
+  [Working with application revisions for CodeDeploy](application-revisions.md) 
+  [Working with applications in CodeDeploy](applications.md) 


| CodeDeploy component | EC2/On\-Premises | AWS Lambda | Amazon ECS | 
| --- | --- | --- | --- | 
| Deployment group | Deploys a revision to a set of instances\. | Deploys a new version of a serverless Lambda function on a high\-availability compute infrastructure\. | Specifies the Amazon ECS service with the containerized application to deploy as a task set, a production and optional test listener used to serve traffic to the deployed application, when to reroute traffic and terminate the deployed application's original task set, and optional trigger, alarm, and rollback settings\. | 
| Deployment | Deploys a new revision that consists of an application and AppSpec file\. The AppSpec specifies how to deploy the application to the instances in a deployment group\. | Shifts production traffic from one version of a Lambda function to a new version of the same function\. The AppSpec file specifies which Lambda function version to deploy\. | Deploys an updated version of an Amazon ECS containerized application as a new, replacement task set\. CodeDeploy reroutes production traffic from the task set with the original version to the new replacement task set with the updated version\. When the deployment completes, the original task set is terminated\. | 
| Deployment configuration | Settings that determine the deployment speed and the minimum number of instances that must be healthy at any point during a deployment\. | Settings that determine how traffic is shifted to the updated Lambda function versions\. | Settings that determine how traffic is shifted to the updated Amazon ECS task set\. | 
| Revision | A combination of an AppSpec file and application files, such as executables, configuration files, and so on\. | An AppSpec file that specifies which Lambda function to deploy and Lambda functions that can run validation tests during deployment lifecycle event hooks\. |  An AppSpec file that specifies: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html)  | 
| Application | A collection of deployment groups and revisions\. An EC2/On\-Premises application uses the EC2/On\-Premises compute platform\. | A collection of deployment groups and revisions\. An application used for an AWS Lambda deployment uses the serverless AWS Lambda compute platform\. | A collection of deployment groups and revisions\. An application used for an Amazon ECS deployment uses the Amazon ECS compute platform\. | 

## Overview of CodeDeploy deployment types<a name="welcome-deployment-overview"></a>

CodeDeploy provides two deployment type options:
+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an in\-place deployment](#welcome-deployment-overview-in-place)\.
**Note**  
AWS Lambda and Amazon ECS deployments cannot use an in\-place deployment type\.
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

  For more information about blue/green deployments, see [Overview of a blue/green deployment](#welcome-deployment-overview-blue-green)\.

**Note**  
Using the CodeDeploy agent, you can perform a deployment on an instance you are signed in to without the need for an application, deployment group, or even an AWS account\. For information, see [Use the CodeDeploy agent to validate a deployment package on a local machine](deployments-local.md)\.

**Topics**
+ [Overview of an in\-place deployment](#welcome-deployment-overview-in-place)
+ [Overview of a blue/green deployment](#welcome-deployment-overview-blue-green)

### Overview of an in\-place deployment<a name="welcome-deployment-overview-in-place"></a>

The following diagram shows the flow of a typical CodeDeploy in\-place deployment\.

**Note**  
AWS Lambda and Amazon ECS deployments cannot use an in\-place deployment type\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/sds_architecture.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

Here's how it works:

1. First, you create deployable content on your local development machine or similar environment, and then you add an *application specification file* \(AppSpec file\)\. The AppSpec file is unique to CodeDeploy\. It defines the deployment actions you want CodeDeploy to execute\. You bundle your deployable content and the AppSpec file into an archive file, and then upload it to an Amazon S3 bucket or a GitHub repository\. This archive file is called an *application revision* \(or simply a *revision*\)\.

1. Next, you provide CodeDeploy with information about your deployment, such as which Amazon S3 bucket or GitHub repository to pull the revision from and to which set of Amazon EC2 instances to deploy its contents\. CodeDeploy calls a set of Amazon EC2 instances a *deployment group*\. A deployment group contains individually tagged Amazon EC2 instances, Amazon EC2 instances in Amazon EC2 Auto Scaling groups, or both\.

   Each time you successfully upload a new application revision that you want to deploy to the deployment group, that bundle is set as the *target revision* for the deployment group\. In other words, the application revision that is currently targeted for deployment is the target revision\. This is also the revision that is pulled for automatic deployments\.

1. Next, the CodeDeploy agent on each instance polls CodeDeploy to determine what and when to pull from the specified Amazon S3 bucket or GitHub repository\.

1. Finally, the CodeDeploy agent on each instance pulls the target revision from the Amazon S3 bucket or GitHub repository and, using the instructions in the AppSpec file, deploys the contents to the instance\.

 CodeDeploy keeps a record of your deployments so that you can get deployment status, deployment configuration parameters, instance health, and so on\.

### Overview of a blue/green deployment<a name="welcome-deployment-overview-blue-green"></a>

A blue/green deployment is used to update your applications while minimizing interruptions caused by the changes of a new application version\. CodeDeploy provisions your new application version alongside the old version before rerouting your production traffic\. 
+  **AWS Lambda**: Traffic is shifted from one version of a Lambda function to a new version of the same Lambda function\. 
+  **Amazon ECS**: Traffic is shifted from a task set in your Amazon ECS service to an updated, replacement task set in the same Amazon ECS service\. 
+  **EC2/On\-Premises**: Traffic is shifted from one set of instances in the original environment to a replacement set of instances\. 

All AWS Lambda and Amazon ECS deployments are blue/green\. An EC2/On\-Premises deployment can be in\-place or blue/green\. A blue/green deployment offers a number of advantages over an in\-place deployment:
+ You can install and test an application in the new replacement environment and deploy it to production simply by rerouting traffic\.
+  If you're using the EC2/On\-Premises compute platform, switching back to the most recent version of an application is faster and more reliable\. That's because traffic can be routed back to the original instances as long as they have not been terminated\. With an in\-place deployment, versions must be rolled back by redeploying the previous version of the application\.
+ If you're using the EC2/On\-Premises compute platform, new instances are provisioned for a blue/green deployment and reflect the most up\-to\-date server configurations\. This helps you avoid the types of problems that sometimes occur on long\-running instances\.
+ If you're using the AWS Lambda compute platform, you control how traffic is shifted from your original AWS Lambda function version to your new AWS Lambda function version\.
+ If you're using the Amazon ECS compute platform, you control how traffic is shifted from your original task set to your new task set\.

A blue/green deployment with AWS CloudFormation can use one of the following methods:
+ **AWS CloudFormation templates for deployments**: When you configure deployments with AWS CloudFormation templates, your deployments are triggered by AWS CloudFormation updates\. When you change a resource and upload a template change, a stack update in AWS CloudFormation initiates the new deployment\. For a list of resources you can use in AWS CloudFormation templates, see [AWS CloudFormation templates for CodeDeploy reference](reference-cloudformation-templates.md)\.
+ **Blue/green deployments through AWS CloudFormation**: You can use AWS CloudFormation to manage your blue/green deployments through stack updates\. You define both your blue and green resources, in addition to specifying the traffic routing and stabilization settings, within the stack template\. Then, if you update selected resources during a stack update, AWS CloudFormation generates all the necessary green resources, shifts the traffic based on the specified traffic routing parameters, and deletes the blue resources\. For more information, see [Automate Amazon ECS blue/green deployments through CodeDeploy using AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/blue-green.html) in the *AWS CloudFormation User Guide*\.
**Note**  
Supported for Amazon ECS blue/green deployments only\.

How you configure a blue/green deployment depends on which compute platform your deployment is using\.

#### Blue/Green deployment on an AWS Lambda compute platform<a name="blue-green-lambda-compute-type"></a>

If you're using the AWS Lambda compute platform, you must choose one of the following deployment configuration types to specify how traffic is shifted from the original AWS Lambda function version to the new AWS Lambda function version:
+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Lambda function to the updated Lambda function version all at once\.

For more information about AWS Lambda deployment configurations, see [Predefined deployment configurations for an AWS Lambda compute platform ](deployment-configurations.md#deployment-configurations-predefined-lambda)\.

#### Blue/Green deployment on an Amazon ECS compute platform<a name="blue-green-ecs-compute-type"></a>

If you're using the Amazon ECS compute platform, you must choose one of the following deployment configuration types to specify how traffic is shifted from the original Amazon ECS task set to the new Amazon ECS task set:
+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Amazon ECS task set in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Amazon ECS task set to the updated Amazon ECS task set all at once\.

For more information about the Amazon ECS deployment configuration, see [ Deployment configurations on an Amazon ECS compute platform ](deployment-configurations.md#deployment-configuration-ecs)\.

#### Blue/Green deployment on an EC2/on\-premises compute platform<a name="blue-green-server-compute-type"></a>

**Note**  
You must use Amazon EC2 instances for blue/green deployments on the EC2/On\-Premises compute platform\. On\-premises instances are not supported for the blue/green deployment type\.

If you're using the EC2/On\-Premises compute platform, the following applies:

 You must have one or more Amazon EC2 instances with identifying Amazon EC2 tags or an Amazon EC2 Auto Scaling group\. The instances must meet these additional requirements:
+ Each Amazon EC2 instance must have the correct IAM instance profile attached\.
+ The CodeDeploy agent must be installed and running on each instance\.

**Note**  
You typically also have an application revision running on the instances in your original environment, but this is not a requirement for a blue/green deployment\.

When you create a deployment group that is used in blue/green deployments, you can choose how your replacement environment is specified:

**Copy an existing Amazon EC2 Auto Scaling group**: During the blue/green deployment, CodeDeploy creates the instances for your replacement environment during the deployment\. With this option, CodeDeploy uses the Amazon EC2 Auto Scaling group you specify as a template for the replacement environment, including the same number of running instances and many other configuration options\.

**Choose instances manually**: You can specify the instances to be counted as your replacement using Amazon EC2 instance tags, Amazon EC2 Auto Scaling group names, or both\. If you choose this option, you do not need to specify the instances for the replacement environment until you create a deployment\.

Here's how it works:

1. You already have instances or an Amazon EC2 Auto Scaling group that serves as your original environment\. The first time you run a blue/green deployment, you typically use instances that were already used in an in\-place deployment\.

1. In an existing CodeDeploy application, you create a blue/green deployment group where, in addition to the options required for an in\-place deployment, you specify the following:
   + The load balancer that routes traffic from your original environment to your replacement environment during the blue/green deployment process\.
   + Whether to reroute traffic to the replacement environment immediately or wait for you to reroute it manually\. 
   + The rate at which traffic is routed to the replacement instances\.
   + Whether the instances that are replaced are terminated or kept running\.

1. You create a deployment for this deployment group during which the following occur:

   1. If you chose to copy an Amazon EC2 Auto Scaling group, instances are provisioned for your replacement environment\.

   1. The application revision you specify for the deployment is installed on the replacement instances\.

   1. If you specified a wait time in the deployment group settings, the deployment is paused\. This is the time when you can run tests and verifications in your replacement environment\. If you don't manually reroute the traffic before the end of the wait period, the deployment is stopped\.

   1. Instances in the replacement environment are registered with an Elastic Load Balancing load balancer and traffic starts being routed to them\.

   1. Instances in the original environment are deregistered and handled according to your specification in the deployment group, either terminated or kept running\.

#### Blue/Green deployment through AWS CloudFormation<a name="blue-green-cfn-config-type"></a>

You can manage CodeDeploy blue/green deployments by modeling your resources with an AWS CloudFormation template\.

When you model your blue/green resources using an AWS CloudFormation template, you create a stack update in AWS CloudFormation that updates your task set\. Production traffic shifts from your service's original task set to a replacement task set either all at once, with linear deployments and bake times, or with canary deployments\. The stack update initiates a deployment in CodeDeploy\. You can view the deployment status and history in CodeDeploy, but you do not otherwise create or manage CodeDeploy resources outside of the AWS CloudFormation template\.

**Note**  
For blue/green deployments through AWS CloudFormation, you don't create a CodeDeploy application or deployment group\.

This method supports Amazon ECS blue/green deployments only\. For more information about blue/green deployments through AWS CloudFormation, see [Create an Amazon ECS blue/green deployment through AWS CloudFormation](deployments-create-ecs-cfn.md)\.

## We want to hear from you<a name="welcome-contact-us"></a>

We welcome your feedback\. To contact us, visit [the CodeDeploy forum](https://forums.aws.amazon.com/forum.jspa?forumID=179)\.

**Topics**
+ [Primary Components](primary-components.md)
+ [Deployments](deployment-steps.md)
+ [Application Specification Files](application-specification-files.md)
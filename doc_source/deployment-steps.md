# CodeDeploy Deployments<a name="deployment-steps"></a>

This topic provides information about the components and workflow of deployments in CodeDeploy\. The deployment process varies, depending on the compute platform \(EC2/On\-Premises or Lambda\) you use for your deployments\. 

**Note**  
 Deployments on an Amazon ECS or an AWS Lambda compute platform are not supported in the following regions: Asia Pacific \(Osaka\-Local\), AWS GovCloud \(US\-East\), AWS GovCloud \(US\-West\), China \(Beijing\), and China \(Ningxia\)\. 

**Topics**
+ [Deployments on an AWS Lambda Compute Platform](#deployment-steps-lambda)
+ [Deployments on an Amazon ECS Compute Platform](#deployment-steps-ecs)
+ [Deployments on an EC2/On\-Premises Compute Platform](#deployment-steps-server)

## Deployments on an AWS Lambda Compute Platform<a name="deployment-steps-lambda"></a>

This topic provides information about the components and workflow of CodeDeploy deployments that use the AWS Lambda compute platform\. 

**Topics**
+ [Deployment Components on an AWS Lambda Compute Platform](#deployment-steps-workflow-lambda)
+ [Deployment Workflow on an AWS Lambda Compute Platform](#deployment-process-workflow-lambda)
+ [Uploading Your Application Revision](#deployment-steps-uploading-your-app-lambda)
+ [Creating Your Application and Deployment Groups](#deployment-steps-registering-app-deployment-groups-lambda)
+ [Deploying Your Application Revision](#deployment-steps-deploy-lambda)
+ [Updating Your Application](#deployment-steps-updating-your-app-lambda)
+ [Stopped and Failed Deployments](#deployment-stop-fail-lambda)
+ [Redeployments and Deployment Rollbacks](#deployment-rollback-lambda)

### Deployment Components on an AWS Lambda Compute Platform<a name="deployment-steps-workflow-lambda"></a>

The following diagram shows the components in a CodeDeploy deployment on an AWS Lambda compute platform\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-components-workflow-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Deployment Workflow on an AWS Lambda Compute Platform<a name="deployment-process-workflow-lambda"></a>

The following diagram shows the primary steps in the deployment of new and updated AWS Lambda functions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-process-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

These steps include:

1. Create an application and give it a name that uniquely identifies the application revisions you want to deploy\. To deploy Lambda functions, choose AWS Lambda compute platform when you create your application\. CodeDeploy uses this name during a deployment to make sure it is referencing the correct deployment components, such as the deployment group, deployment configuration, and application revision\. For more information, see [Create an Application with CodeDeploy](applications-create.md)\. 

1. Set up a deployment group by specifying your deployment group's name\.

1. Choose a deployment configuration to specify how traffic is shifted from your original AWS Lambda function version to your new Lambda function version\. For more information, see [View Deployment Configuration Details with CodeDeploy](deployment-configurations-view-details.md)\.

1. Specify an *application specification file* \(AppSpec file\)\. You can upload the file to Amazon S3, enter it into the console in YAML or JSON format, or specify it with the AWS CLI or SDK\. The AppSpec file specifies an Amazon ECS task definition for the deployment, a container name and port mapping used to route traffic, and Lambda functions that run after deployment lifecycle hooks\. If you don't want to create an AppSpec file, you can enter this information directly into the console in YAML or JSON format\. For more information, see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.

1. Deploy your application revision to the deployment group\. AWS CodeDeploy deploys the Lambda function revision you specified\. The traffic is shifted to your Lambda function revision using the deployment AppSpec file you chose when you created your application\. For more information, see [Create a Deployment with CodeDeploy](deployments-create.md)\.

1. Check the deployment results\. For more information, see [Monitoring Deployments in CodeDeploy](monitoring.md)\.

### Uploading Your Application Revision<a name="deployment-steps-uploading-your-app-lambda"></a>

Place an AppSpec file in Amazon S3 or enter it directly into the console or AWS CLI\. For more information, see [CodeDeploy Application Specification Files](application-specification-files.md)\.

### Creating Your Application and Deployment Groups<a name="deployment-steps-registering-app-deployment-groups-lambda"></a>

A CodeDeploy deployment group on an AWS Lambda compute platform identifies a collection of one or more AppSpec files\. Each AppSpec file can deploy one Lambda function version\. A deployment group also defines a set of configuration options for future deployments, such as alarms and rollback configurations\.

### Deploying Your Application Revision<a name="deployment-steps-deploy-lambda"></a>

Now you're ready to deploy the function revision specified in the AppSpec file to the deployment group\. You can use the CodeDeploy console or the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command\. There are parameters you can specify to control your deployment, including the revision, deployment group, and deployment configuration\.

### Updating Your Application<a name="deployment-steps-updating-your-app-lambda"></a>

You can make updates to your application and then use the CodeDeploy console or call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to push a revision\. 

### Stopped and Failed Deployments<a name="deployment-stop-fail-lambda"></a>

You can use the CodeDeploy console or the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command to stop a deployment\. When you attempt to stop the deployment, one of three things happens:
+ The deployment stops, and the operation returns a status of succeeded\. In this case, no more deployment lifecycle events are run on the deployment group for the stopped deployment\. 
+ The deployment does not immediately stop, and the operation returns a status of pending\. In this case, some deployment lifecycle events might still be running on the deployment group\. After the pending operation is complete, subsequent calls to stop the deployment return a status of succeeded\.
+ The deployment cannot stop, and the operation returns an error\. For more information, see [ErrorInformation](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ErrorInformation.html) and [Common Errors](https://docs.aws.amazon.com/codedeploy/latest/APIReference/CommonErrors.html) in the AWS CodeDeploy API Reference\.

Like stopped deployments, failed deployments might result in some deployment lifecycle events having already been run\. To find out why a deployment failed, you can use the CodeDeploy console or analyze the log file data from the failed deployment\. For more information, see [Application Revision and Log File Cleanup](codedeploy-agent.md#codedeploy-agent-revisions-logs-cleanup) and [View Log Data for CodeDeploy EC2/On\-Premises Deployments](deployments-view-logs.md)\.

### Redeployments and Deployment Rollbacks<a name="deployment-rollback-lambda"></a>

CodeDeploy implements rollbacks by redeploying, as a new deployment, a previously deployed revision\. 

You can configure a deployment group to automatically roll back deployments when certain conditions are met, including when a deployment fails or an alarm monitoring threshold is met\. You can also override the rollback settings specified for a deployment group in an individual deployment\.

You can also choose to roll back a failed deployment by manually redeploying a previously deployed revision\. 

In all cases, the new or rolled\-back deployment is assigned its own deployment ID\. The list of deployments you can view in the CodeDeploy console shows which ones are the result of an automatic deployment\. 

For more information, see [Redeploy and Roll Back a Deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.

## Deployments on an Amazon ECS Compute Platform<a name="deployment-steps-ecs"></a>

This topic provides information about the components and workflow of CodeDeploy deployments that use the Amazon ECS compute platform\. 

**Topics**
+ [Before You Begin an Amazon ECS Deployment](#deployment-steps-prerequisites-ecs)
+ [Deployment Components on an Amazon ECS Compute Platform](#deployment-steps-components-ecs)
+ [Deployment Workflow \(High Level\) on an Amazon ECS Compute Platform](#deployment-process-workflow-ecs)
+ [What Happens During an Amazon ECS Deployment](#deployment-steps-what-happens)
+ [Uploading Your Application Revision](#deployment-steps-uploading-your-app-ecs)
+ [Creating Your Application and Deployment Groups](#deployment-steps-registering-app-deployment-groups-ecs)
+ [Deploying Your Application Revision](#deployment-steps-deploy-ecs)
+ [Updating Your Application](#deployment-steps-updating-your-app-ecs)
+ [Stopped and Failed Deployments](#deployment-stop-fail-ecs)
+ [Redeployments and Deployment Rollbacks](#deployment-rollback-ecs)

### Before You Begin an Amazon ECS Deployment<a name="deployment-steps-prerequisites-ecs"></a>

 Before you begin an Amazon ECS application deployment, you must have the following ready\. Some requirements are specified when you create your deployment group, and some are specified in the AppSpec file\.


****  

| Requirement | Where specified | 
| --- | --- | 
| Amazon ECS cluster | Deployment group | 
| Amazon ECS service | Deployment group | 
| Application Load Balancer or Network Load Balancer | Deployment group | 
| Production listener | Deployment group | 
| Test listener \(optional\) | Deployment group | 
| Two target groups | Deployment group | 
| Amazon ECS task definition | AppSpec file | 
| Container name | AppSpec file | 
| Container port | AppSpec file | 

**Amazon ECS cluster**  
An Amazon ECS *cluster* is a logical grouping of tasks or services\. You specify the Amazon ECS cluster that contains your Amazon ECS service when you create your CodeDeploy application's deployment group\. For more information, see [Amazon ECS Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_clusters.html) in the *Amazon Elastic Container Service User Guide*\.

**Amazon ECS service**  
An Amazon ECS *service* maintains and runs specified instances of a task definition in an Amazon ECS cluster\. Your Amazon ECS service must be enabled for CodeDeploy\. By default, an Amazon ECS service is enabled for Amazon ECS deployments\. When you create your deployment group, you choose to deploy an Amazon ECS service that is in your Amazon ECS cluster\. For more information, see [Amazon ECS Services](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) in the *Amazon Elastic Container Service User Guide*\.

**Application Load Balancer or Network Load Balancer**  
 You must use Elastic Load Balancing with the Amazon ECS service you want to update with an Amazon ECS deployment\. You can use an Application Load Balancer or an Network Load Balancer\. We recommend an Application Load Balancer so you can take advantage of features such as dynamic port mapping and path\-based routing and priority rules\. You specify the a load balancer when you create your CodeDeploy application's deployment group\. For more information, see [Creating a Load Balancer](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-load-balancer.html) in the *Amazon Elastic Container Service User Guide*\. 

**One or two listeners**  
A *listener* is used by your load balancer to direct traffic to your target groups\. One production listener is required\. You can specify an optional second test listener that directs traffic to your replacement task set while you run validation tests\. You specify one or both listeners when you create your deployment group\. If you use the Amazon ECS console to create your Amazon ECS service, your listeners are created for you\. For more information, see [Listeners for Your Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listener.html) in the *Elastic Load Balancing User Guide* and [Creating a Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service.html) in the *Amazon Elastic Container Service User Guide*\.

**Two Amazon ECS target groups**  
 A *target group* is used to route traffic to a registered target\. An Amazon ECS deployment requires two target groups: one for your Amazon ECS application's original task set and one for its replacement task set\. During deployment, CodeDeploy creates a replacement task set and reroutes traffic from the original task set to the new one\. You specify the target groups when you create your CodeDeploy application's deployment group\.   
 During a deployment, CodeDeploy determines which target group is associated with the task set in your Amazon ECS service that has the status `PRIMARY` \(this is the original task set\) and associates one target group with it, and then associates the other target group with the replacement task set\. If you do another deployment, the target group associated with the current deployment's original task set is associated with the next deployment's replacement task set\. For more information, see [Target Groups for Your Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html) in the *Elastic Load Balancing User Guide*\. 

**An Amazon ECS task definition**  
 A *task definition* is required to run the Docker container that contains your Amazon ECS application\. You specify the ARN of your task definition in your CodeDeploy application's AppSpec file\. For more information, see [Amazon ECS Task Definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) in the *Amazon Elastic Container Service User Guide* and [ AppSpec 'resources' Section for Amazon ECS Deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs)\. 

**A container for your Amazon ECS application**  
 A Docker *container* is a unit of software that packages up code and its dependencies so your application can run\. A container isolates your its application so it runs in different computing environments\. Your load balancer directs traffic to a container in your Amazon ECS application's task set\. You specify the name of your container in your CodeDeploy application's AppSpec file\. The container specified in your AppSpec file must be one of the containers specified in your Amazon ECS task definition\. For more information, see [What Is Amazon Elastic Container Service?](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) in the *Amazon Elastic Container Service User Guide* and [ AppSpec 'resources' Section for Amazon ECS Deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs)\. 

**A port for your replacement task set**  
 During your Amazon ECS deployment, your load balancer directs traffic to this *port* on the container specified in your CodeDeploy application's AppSpec file\. You specify the port in your CodeDeploy application's AppSpec file\. For more information, see [ AppSpec 'resources' Section for Amazon ECS Deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs)\. 

### Deployment Components on an Amazon ECS Compute Platform<a name="deployment-steps-components-ecs"></a>

The following diagram shows the components in a CodeDeploy deployment on an Amazon ECS compute platform\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-components-workflow-ecs.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Deployment Workflow \(High Level\) on an Amazon ECS Compute Platform<a name="deployment-process-workflow-ecs"></a>

The following diagram shows the primary steps in the deployment of updated Amazon ECS services\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-process-ecs.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

These steps include:

1. Create an AWS CodeDeploy application by specifying a name that uniquely represents what you want to deploy\. To deploy an Amazon ECS application, in your AWS CodeDeploy application, choose the Amazon ECS compute platform\. CodeDeploy uses an application during a deployment to reference the correct deployment components, such as the deployment group, target groups, listeners, and traffic rerouting behavior, and application revision\. For more information, see [Create an Application with CodeDeploy](applications-create.md)\. 

1. Set up a deployment group by specifying:
   +  The deployment group name\. 
   +  Your Amazon ECS cluster and service name\. The Amazon ECS service's deployment controller must be set to CodeDeploy\. 
   +  The production listener, an optional test listener, and target groups used during a deployment\. 
   +  Deployment settings, such as when to reroute production traffic to the replacement Amazon ECS task set in your Amazon ECS service and when to terminate the original Amazon ECS task set in your Amazon ECS service\. 
   +  Optional settings, such as triggers, alarms, and rollback behavior\. 

1. Specify an *application specification file* \(AppSpec file\)\. You can upload it to Amazon S3, enter it into the console in YAML or JSON format, or specify it with the AWS CLI or SDK\. The AppSpec file specifies an Amazon ECS task definition for the deployment, a container name and port mapping used to route traffic, and Lambda functions run after deployment lifecycle hooks\. The container name must be a container in your Amazon ECS task definition\. For more information, see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.

1. Deploy your application revision\. AWS CodeDeploy reroutes traffic from the original version of a task set in your Amazon ECS service to a new, replacement task set\. Target groups specified in the deployment group are used to serve traffic to the original and replacement task sets\. After the deployment is complete, the original task set is terminated\. You can specify an optional test listener to serve test traffic to your replacement version before traffic is rerouted to it\. For more information, see [Create a Deployment with CodeDeploy](deployments-create.md)\.

1. Check the deployment results\. For more information, see [Monitoring Deployments in CodeDeploy](monitoring.md)\.

### What Happens During an Amazon ECS Deployment<a name="deployment-steps-what-happens"></a>

Before an Amazon ECS deployment with a test listener starts, you must configure its components\. For more information, see [Before You Begin an Amazon ECS Deployment](#deployment-steps-prerequisites-ecs)\.

 The following diagram shows the relationship between these components when an Amazon ECS deployment is ready to start\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-1.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

When the deployment starts, the deployment lifecycle events start to execute one at a time\. Some lifecycle events are hooks that only execute Lambda functions specified in the AppSpec file\. The deployment lifecycle events in the following table are listed in the order they execute\. For more information, see [AppSpec 'hooks' Section for an Amazon ECS Deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\.


| Lifecycle Event | Lifecycle Event Action | 
| --- | --- | 
| BeforeInstall \(a hook for Lambda functions\) | Run Lambda functions\. | 
| Install | Set up the replacement task set\. | 
| AfterInstall \(a hook for Lambda functions\) | Run Lambda functions\. | 
| AllowTestTraffic | Route traffic from the test listener to target group 2\. | 
| AfterAllowTestTraffic \(a hook for Lambda functions\) | Run Lambda functions\. | 
| BeforeAllowTraffic \(a hook for Lambda functions\) | Run Lambda functions\. | 
| AllowTraffic | Route traffic from the production listener to target group 2\. | 
| AfterAllowTraffic | Run Lambda functions\. | 

**Note**  
Lambda functions in a hook are optional\.

1. <a name="ecs-before-install"></a>

****

   Execute any Lambda functions specified in the `BeforeInstall` hook in the AppSpec file\.

1. <a name="ecs-install"></a>

****

   During the `Install` lifecycle event:

   1.  A replacement task set is created in your Amazon ECS service\. 

   1.  The updated containerized application is installed into the replacement task set\. 

   1.  The second target group is associated with the replacement task set\. 

    This diagram shows deployment components with the new replacement task set\. The containerized application is inside this task set\. The task set is composed of three tasks\. \(An application can have any number of tasks\.\) The second target group is now associated with the replacement task set\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-2.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

1. <a name="ecs-after-install"></a>

****

   Execute any Lambda functions specified in the `AfterInstall` hook in the AppSpec file\.

1. <a name="ecs-allow-test-traffic"></a>

****

   The `AllowTestTraffic` event is invoked\. During this lifecycle event, the test listener routes traffic to the updated containerized application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-3.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

1. <a name="ecs-after-allow-test-traffic"></a>

****

   Execute any Lambda functions specified in the `AfterAllowTestTraffic` hook in the AppSpec file\. Lambda functions can validate the deployment using the test traffic\. For example, a Lambda function can serve traffic to the test listener and track metrics from the replacement task set\. If rollbacks are configured, you can configure a CloudWatch alarm that triggers a rollback when the validation test in your Lambda function fails\.

    After the validation tests are complete, one of the following occurs: 
   +  If validation fails and rollbacks are configured, the deployment status is marked `Failed` and components return to their state when the deployment started\. 
   +  If validation fails and rollbacks are not configured, the deployment status is marked `Failed` and components remain in their current state\.
   +  If validation succeeds, the deployment continues to the `BeforeAllowTraffic` hook\.

    For more information, see [Monitoring Deployments with CloudWatch Alarms in CodeDeploy](monitoring-create-alarms.md), [Automatic Rollbacks](deployments-rollback-and-redeploy.md#deployments-rollback-and-redeploy-automatic-rollbacks), and [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)\. 

1. <a name="ecs-before-allow-traffic"></a>

****

   Execute any Lambda functions specified in the `BeforeAllowTraffic` hook in the AppSpec file\.

1. <a name="ecs-allow-traffic"></a>

****

   The `AllowTraffic` event is invoked\. Production traffic is rerouted from the original task set to the replacement task set\. The following diagram shows the replacement task set receiving production traffic\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-4.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

1. <a name="ecs-after-allow-traffic"></a>

****

   Execute any Lambda functions specified in the `AfterAllowTraffic` hook in the AppSpec file\.

1. 

****

   After all events succeed, the deployment status is set to `Succeeded` and the original task set is removed\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-5.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Uploading Your Application Revision<a name="deployment-steps-uploading-your-app-ecs"></a>

Place an AppSpec file in Amazon S3 or enter it directly into the console or AWS CLI\. For more information, see [CodeDeploy Application Specification Files](application-specification-files.md)\.

### Creating Your Application and Deployment Groups<a name="deployment-steps-registering-app-deployment-groups-ecs"></a>

A CodeDeploy deployment group on an Amazon ECS compute platform identifies listeners to serve traffic to your updated Amazon ECS application and two target groups used during your deployment\. A deployment group also defines a set of configuration options, such as alarms and rollback configurations\.

### Deploying Your Application Revision<a name="deployment-steps-deploy-ecs"></a>

Now you're ready to deploy the updated Amazon ECS service specified in your deployment group\. You can use the CodeDeploy console or the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command\. There are parameters you can specify to control your deployment, including the revision and deployment group\.

### Updating Your Application<a name="deployment-steps-updating-your-app-ecs"></a>

You can make updates to your application and then use the CodeDeploy console or call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to push a revision\. 

### Stopped and Failed Deployments<a name="deployment-stop-fail-ecs"></a>

You can use the CodeDeploy console or the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command to stop a deployment\. When you attempt to stop the deployment, one of three things happens:
+ The deployment stops, and the operation returns a status of succeeded\. In this case, no more deployment lifecycle events are run on the deployment group for the stopped deployment\. 
+ The deployment does not immediately stop, and the operation returns a status of pending\. In this case, some deployment lifecycle events might still be running on the deployment group\. After the pending operation is complete, subsequent calls to stop the deployment return a status of succeeded\.
+ The deployment cannot stop, and the operation returns an error\. For more information, see [Error Information](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ErrorInformation.html) and [Common Errors](https://docs.aws.amazon.com/codedeploy/latest/APIReference/CommonErrors.html) in the AWS CodeDeploy API Reference\.

### Redeployments and Deployment Rollbacks<a name="deployment-rollback-ecs"></a>

CodeDeploy implements rollbacks by rerouting traffic from the replacement task set to the original task set\. 

You can configure a deployment group to automatically roll back deployments when certain conditions are met, including when a deployment fails or an alarm monitoring threshold is met\. You can also override the rollback settings specified for a deployment group in an individual deployment\.

You can also choose to roll back a failed deployment by manually redeploying a previously deployed revision\. 

In all cases, the new or rolled\-back deployment is assigned its own deployment ID\. The CodeDeploy console displays a list of deployments that are the result of an automatic deployment\. 

If you redeploy, the target group associated with the current deployment's original task set is associated with the redeployment's replacement task set\.

For more information, see [Redeploy and Roll Back a Deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.

## Deployments on an EC2/On\-Premises Compute Platform<a name="deployment-steps-server"></a>

This topic provides information about the components and workflow of CodeDeploy deployments that use the EC2/On\-Premises compute platform\. For information about blue/green deployments, see [Overview of a Blue/Green Deployment](welcome.md#welcome-deployment-overview-blue-green)

**Topics**
+ [Deployment Components on an EC2/On\-Premises Compute Platform](#deployment-steps-components-server)
+ [Deployment Workflow on an EC2/On\-Premises Compute Platform](#deployment-steps-workflow)
+ [Setting Up Instances](#deployment-steps-setting-up-instances)
+ [Uploading Your Application Revision](#deployment-steps-uploading-your-app)
+ [Creating Your Application and Deployment Groups](#deployment-steps-registering-app-deployment-groups)
+ [Deploying Your Application Revision](#deployment-steps-deploy)
+ [Updating Your Application](#deployment-steps-updating-your-app)
+ [Stopped and Failed Deployments](#deployment-stop-fail)
+ [Redeployments and Deployment Rollbacks](#deployment-rollback)

### Deployment Components on an EC2/On\-Premises Compute Platform<a name="deployment-steps-components-server"></a>

The following diagram shows the components in a CodeDeploy deployment on an EC2/On\-Premises compute platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-components-workflow.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Deployment Workflow on an EC2/On\-Premises Compute Platform<a name="deployment-steps-workflow"></a>

The following diagram shows the major steps in the deployment of application revisions:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-process.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

These steps include:

1. Create an application and give it a name that uniquely identifies the application revisions you want to deploy and the compute platform for your application\. CodeDeploy uses this name during a deployment to make sure it is referencing the correct deployment components, such as the deployment group, deployment configuration, and application revision\. For more information, see [Create an Application with CodeDeploy](applications-create.md)\.

1. Set up a deployment group by specifying a deployment type and the instances to which you want to deploy your application revisions\. An in\-place deployment updates instances with the latest application revision\. A blue/green deployment registers a replacement set of instances for the deployment group with a load balancer and deregisters the original instances\. 

   You can specify the tags applied to the instances, the Amazon EC2 Auto Scaling group names, or both\.

   If you specify one group of tags in a deployment group, CodeDeploy deploys to instances that have at least one of the specified tags applied\. If you specify two or more tag groups, CodeDeploy deploys only to the instances that meet the criteria for each of the tag groups\. For more information, see [Tagging Instances for Deployment Groups in CodeDeploy](instances-tagging.md)\.

   In all cases, the instances must be configured to be used in a deployment \(that is, they must be tagged or belong to an Amazon EC2 Auto Scaling group\) and have the CodeDeploy agent installed and running\. 

   We provide you with an AWS CloudFormation template that you can use to quickly set up an Amazon EC2 instance based on Amazon Linux or Windows Server\. We also provide you with the standalone CodeDeploy agent so that you can install it on Amazon Linux, Ubuntu Server, Red Hat Enterprise Linux \(RHEL\), or Windows Server instances\. For more information, see [Create a Deployment Group with CodeDeploy](deployment-groups-create.md)\.

   You can also specify the following options: 
   + **Amazon SNS notifications**\. Create triggers that send notifications to subscribers of an Amazon SNS topic when specified events, such as success or failure events, occur in deployments and instances\. For more information, see [Monitoring Deployments with Amazon SNS Event Notifications](monitoring-sns-event-notifications.md)\.
   + **Alarm\-based deployment management**\. Implement Amazon CloudWatch alarm monitoring to stop deployments when your metrics exceed or fall below the thresholds set in CloudWatch\.
   + **Automatic deployment rollbacks**\. Configure a deployment to roll back automatically to the previously known good revision when a deployment fails or an alarm threshold is met\.

1. Specify a deployment configuration to indicate to how many instances your application revisions should be simultaneously deployed and to describe the success and failure conditions for the deployment\. For more information, see [View Deployment Configuration Details with CodeDeploy](deployment-configurations-view-details.md)\.

1. Upload an application revision to Amazon S3 or GitHub\. In addition to the files you want to deploy and any scripts you want to run during the deployment, you must include an *application specification file* \(AppSpec file\)\. This file contains deployment instructions, such as where to copy the files onto each instance and when to run deployment scripts\. For more information, see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.

1. Deploy your application revision to the deployment group\. The CodeDeploy agent on each instance in the deployment group copies your application revision from Amazon S3 or GitHub to the instance\. The CodeDeploy agent then unbundles the revision, and using the AppSpec file, copies the files into the specified locations and executes any deployment scripts\. For more information, see [Create a Deployment with CodeDeploy](deployments-create.md)\.

1. Check the deployment results\. For more information, see [Monitoring Deployments in CodeDeploy](monitoring.md)\.

1. Redeploy a revision\. You might want to do this if you need to fix a bug in the source content, or run the deployment scripts in a different order, or address a failed deployment\. To do this, rebundle your revised source content, any deployment scripts, and the AppSpec file into a new revision, and then upload the revision to the Amazon S3 bucket or GitHub repository\. Then execute a new deployment to the same deployment group with the new revision\. For more information, see [Create a Deployment with CodeDeploy](deployments-create.md)\.

### Setting Up Instances<a name="deployment-steps-setting-up-instances"></a>

 You must set up instances before you can deploy application revisions for the first time\. If an application revision requires three production servers and two backup servers, you launch or use five instances\. 

To manually provision instances:

1. Install the CodeDeploy agent on the instances\. The CodeDeploy agent can be installed on Amazon Linux, Ubuntu Server, RHEL, and Windows Server instances\.

1. Enable tagging, if you are using tags to identify instances in a deployment group\. CodeDeploy relies on tags to identify and group instances into CodeDeploy deployment groups\. Although the Getting Started tutorials used both, you can simply use a key or a value to define a tag for a deployment group\.

1. Launch Amazon EC2 instances with an IAM instance profile attached\. The IAM instance profile must be attached to an Amazon EC2 instance as it is launched for the CodeDeploy agent to verify the identity of the instance\.

1. Create a service role\. Provide service access so that CodeDeploy can expand the tags in your AWS account\.

For an initial deployment, the AWS CloudFormation template does all of this for you\. It creates and configures new, single Amazon EC2 instances based on Amazon Linux or Windows Server with the CodeDeploy agent already installed\. For more information, see [Working with Instances for CodeDeploy](instances.md)\. 

**Note**  
For a blue/green deployment, you can choose between using instances you already have for the replacement environment or letting CodeDeploy provision new instances for you as part of the deployment process\. 

### Uploading Your Application Revision<a name="deployment-steps-uploading-your-app"></a>

Place an AppSpec file under the root folder in your application's source content folder structure\. For more information, see [CodeDeploy Application Specification Files](application-specification-files.md)\.

Bundle the application's source content folder structure into an archive file format such as zip, tar, or compressed tar\. Upload the archive file \(the *revision*\) to an Amazon S3 bucket or GitHub repository\.

**Note**  
The tar and compressed tar archive file formats \(\.tar and \.tar\.gz\) are not supported for Windows Server instances\.

### Creating Your Application and Deployment Groups<a name="deployment-steps-registering-app-deployment-groups"></a>

A CodeDeploy deployment group identifies a collection of instances based on their tags, Amazon EC2 Auto Scaling group names, or both\. Multiple application revisions can be deployed to the same instance\. An application revision can be deployed to multiple instances\. 

For example, you could add a tag of "Prod" to the three production servers and "Backup" to the two backup servers\. These two tags can be used to create two different deployment groups in the CodeDeploy application, allowing you to choose which set of servers \(or both\) should participate in a deployment\.

You can use multiple tag groups in a deployment group to restrict deployments to a smaller set of instances\. For information, see [Tagging Instances for Deployment Groups in CodeDeploy](instances-tagging.md)\.

### Deploying Your Application Revision<a name="deployment-steps-deploy"></a>

Now you're ready to deploy your application revision from Amazon S3 or GitHub to the deployment group\. You can use the CodeDeploy console or the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command\. There are parameters you can specify to control your deployment, including the revision, deployment group, and deployment configuration\.

### Updating Your Application<a name="deployment-steps-updating-your-app"></a>

You can make updates to your application and then use the CodeDeploy console or call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to push a revision\. 

### Stopped and Failed Deployments<a name="deployment-stop-fail"></a>

You can use the CodeDeploy console or the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command to stop a deployment\. When you attempt to stop the deployment, one of three things happens:
+ The deployment stops, and the operation returns a status of succeeded\. In this case, no more deployment lifecycle events are run on the deployment group for the stopped deployment\. Some files might have already been copied to, and some scripts might have already been run on, one or more of the instances in the deployment group\.
+ The deployment does not immediately stop, and the operation returns a status of pending\. In this case, some deployment lifecycle events might still be running on the deployment group\. Some files might have already been copied to, and some scripts might have already been run on, one or more of the instances in the deployment group\. After the pending operation is complete, subsequent calls to stop the deployment return a status of succeeded\.
+ The deployment cannot stop, and the operation returns an error\. For more information, see [ErrorInformation](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ErrorInformation.html) and [Common Errors](https://docs.aws.amazon.com/codedeploy/latest/APIReference/CommonErrors.html) in the AWS CodeDeploy API Reference\.

Like stopped deployments, failed deployments might result in some deployment lifecycle events having already been run on one or more of the instances in the deployment group\. To find out why a deployment failed, you can use the CodeDeploy console, call the [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command, or analyze the log file data from the failed deployment\. For more information, see [Application Revision and Log File Cleanup](codedeploy-agent.md#codedeploy-agent-revisions-logs-cleanup) and [View Log Data for CodeDeploy EC2/On\-Premises Deployments](deployments-view-logs.md)\.

### Redeployments and Deployment Rollbacks<a name="deployment-rollback"></a>

CodeDeploy implements rollbacks by redeploying, as a new deployment, a previously deployed revision\. 

You can configure a deployment group to automatically roll back deployments when certain conditions are met, including when a deployment fails or an alarm monitoring threshold is met\. You can also override the rollback settings specified for a deployment group in an individual deployment\.

You can also choose to roll back a failed deployment by manually redeploying a previously deployed revision\. 

In all cases, the new or rolled\-back deployment is assigned its own deployment ID\. The list of deployments you can view in the CodeDeploy console shows which ones are the result of an automatic deployment\. 

For more information, see [Redeploy and Roll Back a Deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.
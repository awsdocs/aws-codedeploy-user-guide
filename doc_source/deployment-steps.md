--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# AWS CodeDeploy Deployments<a name="deployment-steps"></a>

This topic provides information about the components and workflow of deployments in AWS CodeDeploy\. The deployment process varies, depending on the compute platform \(EC2/On\-Premises or Lambda\) you use for your deployments\. 

**Topics**
+ [Deployments on an AWS Lambda Compute Platform](#deployment-steps-lambda)
+ [Deployments on an EC2/On\-Premises Compute Platform](#deployment-steps-server)

## Deployments on an AWS Lambda Compute Platform<a name="deployment-steps-lambda"></a>

This topic provides information about the components and workflow of AWS CodeDeploy deployments that use the AWS Lambda compute platform\. 

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

The following diagram shows the components in an AWS CodeDeploy deployment on an AWS Lambda compute platform\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-components-workflow-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Deployment Workflow on an AWS Lambda Compute Platform<a name="deployment-process-workflow-lambda"></a>

The following diagram shows the major steps in the deployment of new and updated AWS Lambda functions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-process-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

These steps include:

1. Creating an application by specifying a name that uniquely represents the application revisions you want to deploy\. To deploy Lambda functions, you specify the AWS Lambda compute platform when you create your application\. AWS CodeDeploy uses this name during a deployment to make sure it is referencing the correct deployment components, such as the deployment group, deployment configuration, and application revision\. For more information, see [Create an Application with AWS CodeDeploy](applications-create.md)\. 

1. Setting up a deployment group by specifying your deployment group's name\.

1. Choosing a deployment configuration to specify how traffic is shifted from your original AWS Lambda function version to your new Lambda function version\. For more information, see [View Deployment Configuration Details with AWS CodeDeploy](deployment-configurations-view-details.md)\.

1. Uploading an *application specification file* \(AppSpec file\) to Amazon S3\. The AppSpec file specifies a Lambda function version and Lambda functions used to validate your deployment\. If you don't want to create an AppSpec file, you can specify a Lambda function version and Lambda deployment validation functions directly in the console using YAML or JSON\. For more information, see [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\.

1. Deploying your application revision to the deployment group\. AWS CodeDeploy deploys the Lambda function revision you specified\. The traffic is shifted to your Lambda function revision using the deployment AppSpec file you chose when you created your application\. For more information, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.

1. Checking the deployment results\. For more information, see [Monitoring Deployments in AWS CodeDeploy](monitoring.md)\.

1. Redeploying a revision\. You might want to do this if you need to fix a bug in a Lambda function in your deployment or address a failed deployment\. If you redeploy the same Lambda function version, then you simply execute a new deployment to the same deployment group with the same AppSpec file\. If you rename, add, or remove a Lambda function before you redeploy, you must update your AppSpec file\. For more information, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.

### Uploading Your Application Revision<a name="deployment-steps-uploading-your-app-lambda"></a>

Place an AppSpec file in Amazon S3 or enter it directly into the console or AWS CLI\. For more information, see [AWS CodeDeploy Application Specification Files](application-specification-files.md)\.

### Creating Your Application and Deployment Groups<a name="deployment-steps-registering-app-deployment-groups-lambda"></a>

An AWS CodeDeploy deployment group on an AWS Lambda compute platform identifies a collection of one or more AppSpec files\. Each AppSpec file can deploy one Lambda function version\. A deployment group also defines a set of configuration options for future deployments, such as alarms and rollback configurations\.

### Deploying Your Application Revision<a name="deployment-steps-deploy-lambda"></a>

Now you're ready to deploy the function revision specified in the AppSpec file to the deployment group\. You can use the AWS CodeDeploy console or the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command\. There are parameters you can specify to control your deployment, including the revision, deployment group, and deployment configuration\.

### Updating Your Application<a name="deployment-steps-updating-your-app-lambda"></a>

You can make updates to your application and then use the AWS CodeDeploy console or call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to push a revision\. 

### Stopped and Failed Deployments<a name="deployment-stop-fail-lambda"></a>

You can use the AWS CodeDeploy console or the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command to stop a deployment\. When you attempt to stop the deployment, one of three things happens:
+ The deployment stops, and the operation returns a status of succeeded\. In this case, no more deployment lifecycle events are run on the deployment group for the stopped deployment\. 
+ The deployment does not immediately stop, and the operation returns a status of pending\. In this case, some deployment lifecycle events might still be running on the deployment group\. After the pending operation is complete, subsequent calls to stop the deployment return a status of succeeded\.
+ The deployment cannot stop, and the operation returns an error\. For more information, see [ErrorInformation](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ErrorInformation.html) and [Common Errors](https://docs.aws.amazon.com/codedeploy/latest/APIReference/CommonErrors.html) in the AWS CodeDeploy API Reference\.

Like stopped deployments, failed deployments might result in some deployment lifecycle events having already been run\. To find out why a deployment failed, you can use the AWS CodeDeploy console or analyze the log file data from the failed deployment\. For more information, see [Application Revision and Log File Cleanup](codedeploy-agent.md#codedeploy-agent-revisions-logs-cleanup) and [View Log Data for AWS CodeDeploy Deployments](deployments-view-logs.md)\.

### Redeployments and Deployment Rollbacks<a name="deployment-rollback-lambda"></a>

AWS CodeDeploy implements rollbacks by redeploying, as a new deployment, a previously deployed revision\. 

You can configure a deployment group to automatically roll back deployments when certain conditions are met, including when a deployment fails or an alarm monitoring threshold is met\. You can also override the rollback settings specified for a deployment group in an individual deployment\.

You can also choose to roll back a failed deployment by manually redeploying a previously deployed revision\. 

In all cases, the new or rolled\-back deployment is assigned its own deployment ID\. The list of deployments you can view in the AWS CodeDeploy console shows which ones are the result of an automatic deployment\. 

For more information, see [Redeploy and Roll Back a Deployment with AWS CodeDeploy](deployments-rollback-and-redeploy.md)\.

## Deployments on an EC2/On\-Premises Compute Platform<a name="deployment-steps-server"></a>

This topic provides information about the components and workflow of AWS CodeDeploy deployments that use the EC2/On\-Premises compute platform\. For information about blue/green deployments, see [Overview of a Blue/Green Deployment](welcome.md#welcome-deployment-overview-blue-green)

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

The following diagram shows the components in an AWS CodeDeploy deployment on an EC2/On\-Premises compute platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-components-workflow.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

### Deployment Workflow on an EC2/On\-Premises Compute Platform<a name="deployment-steps-workflow"></a>

The following diagram shows the major steps in the deployment of application revisions:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/deployment-process.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

These steps include:

1. Creating an application by specifying a name that uniquely represents the application revisions you want to deploy and the compute platform for your application\. AWS CodeDeploy uses this name during a deployment to make sure it is referencing the correct deployment components, such as the deployment group, deployment configuration, and application revision\. For more information, see [Create an Application with AWS CodeDeploy](applications-create.md)\.

1. Setting up a deployment group by specifying a deployment type and the instances to which you want to deploy your application revisions\. An in\-place deployment updates instances with the latest application revision\. A blue/green deployment registers a replacement set of instances for the deployment group with a load balancer and deregisters the original instances\. 

   You can specify the tags applied to the instances, the Amazon EC2 Auto Scaling group names, or both\.

   If you specify one group of tags in a deployment group, AWS CodeDeploy deploys to instances that have at least one of the specified tags applied\. If you specify two or more tag groups, AWS CodeDeploy deploys only to the instances that meet the criteria for each of the tag groups\. For more information, see [Tagging Instances for Deployment Groups in AWS CodeDeploy](instances-tagging.md)\.

   In all cases, the instances must be configured to be used in a deployment \(that is, they must be tagged or belong to an Amazon EC2 Auto Scaling group\) and have the AWS CodeDeploy agent installed and running\. 

   We provide you with an AWS CloudFormation template that you can use to quickly set up an Amazon EC2 instance based on Amazon Linux or Windows Server\. We also provide you with the standalone AWS CodeDeploy agent so that you can install it on Amazon Linux, Ubuntu Server, Red Hat Enterprise Linux \(RHEL\), or Windows Server instances\. For more information, see [Create a Deployment Group with AWS CodeDeploy](deployment-groups-create.md)\.

   You can also specify the following options: 
   + **Amazon SNS notifications** — Create triggers that will send notifications to subscribers of an Amazon SNS topic when specified events, such as success or failure events, occur in deployments and instances\. For more information, see [Monitoring Deployments with Amazon SNS Event Notifications](monitoring-sns-event-notifications.md)\.
   + **Alarm\-based deployment management** — Implement Amazon CloudWatch alarm monitoring to stop deployments when your metrics exceed or fall below the thresholds set in CloudWatch\.
   + **Automatic deployment rollbacks** — Configure a deployment to roll back automatically to the previously known good revision when a deployment fails or an alarm threshold is met\.

1. Specifying a deployment configuration to indicate to how many instances to simultaneously deploy your application revisions and describing the success and failure conditions for the deployment\. For more information, see [View Deployment Configuration Details with AWS CodeDeploy](deployment-configurations-view-details.md)\.

1. Uploading an application revision to Amazon S3 or GitHub\. In addition to the files you want to deploy and any scripts you want to run during the deployment, you must include an *application specification file* \(AppSpec file\)\. This file contains deployment instructions, such as where to copy the files onto each instance and when to run deployment scripts\. For more information, see [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\.

1. Deploying your application revision to the deployment group\. The AWS CodeDeploy agent on each instance in the deployment group copies your application revision from Amazon S3 or GitHub to the instance\. The AWS CodeDeploy agent then unbundles the revision, and using the AppSpec file, copies the files into the specified locations and executes any deployment scripts\. For more information, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.

1. Checking the deployment results\. For more information, see [Monitoring Deployments in AWS CodeDeploy](monitoring.md)\.

1. Redeploying a revision\. You might want to do this if you need to fix a bug in the source content, or run the deployment scripts in a different order, or address a failed deployment\. To do this, you rebundle your revised source content, any deployment scripts, and the AppSpec file into a new revision, and then upload the revision to the Amazon S3 bucket or GitHub repository\. You then execute a new deployment to the same deployment group with the new revision\. For more information, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.

### Setting Up Instances<a name="deployment-steps-setting-up-instances"></a>

 You need to set up instances before you can deploy application revisions for the first time\. If an application revision requires three production servers and two backup servers, you will launch or use five instances\. 

To manually provision instances:

1. Install the AWS CodeDeploy agent on the instances\. The AWS CodeDeploy agent can be installed on Amazon Linux, Ubuntu Server, RHEL, and Windows Server instances\.

1. Enable tagging, if you are using tags to identify instances in a deployment group\. AWS CodeDeploy relies on tags to identify and group instances into AWS CodeDeploy deployment groups\. Although the Getting Started tutorials used both, you can simply use a key or a value to define a tag for a deployment group\.

1. Launch Amazon EC2 instances with an IAM instance profile attached\. The IAM instance profile must be attached to an Amazon EC2 instance as it is launched in order for the AWS CodeDeploy agent to verify the identity of the instance\.

1. Create a service role\. Provide service access so that AWS CodeDeploy can expand the tags in your AWS account\.

For an initial deployment, the AWS CloudFormation template does all of this for you\. It creates and configures new, single Amazon EC2 instances based on Amazon Linux or Windows Server with the AWS CodeDeploy agent already installed\. For more information, see [Working with Instances for AWS CodeDeploy](instances.md)\. 

**Note**  
For a blue/green deployment, you can choose between using instances you already have for the replacement environment or letting AWS CodeDeploy provision new instances for you as part of the deployment process\. 

### Uploading Your Application Revision<a name="deployment-steps-uploading-your-app"></a>

Place an AppSpec file under the root folder in your application's source content folder structure\. For more information, see [AWS CodeDeploy Application Specification Files](application-specification-files.md)\.

Bundle the application's source content folder structure into an archive file format such as zip, tar, or compressed tar\. Upload the archive file \(the *revision*\) to an Amazon S3 bucket or GitHub repository\.

**Note**  
The tar and compressed tar archive file formats \(\.tar and \.tar\.gz\) are not supported for Windows Server instances\.

### Creating Your Application and Deployment Groups<a name="deployment-steps-registering-app-deployment-groups"></a>

An AWS CodeDeploy deployment group identifies a collection of instances based on their tags, Amazon EC2 Auto Scaling group names, or both\. Multiple application revisions can be deployed to the same instance\. An application revision can be deployed to multiple instances\. 

For example, you could add a tag of "Prod" to the three production servers and "Backup" to the two backup servers\. These two tags can be used to create two different deployment groups in the AWS CodeDeploy application, allowing you to choose which set of servers \(or both\) should participate in a deployment\.

You can use multiple tag groups in a deployment group to restrict deployments to a smaller set of instances\. For information, see [Tagging Instances for Deployment Groups in AWS CodeDeploy](instances-tagging.md)\.

### Deploying Your Application Revision<a name="deployment-steps-deploy"></a>

Now you're ready to deploy your application revision from Amazon S3 or GitHub to the deployment group\. You can use the AWS CodeDeploy console or the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command\. There are parameters you can specify to control your deployment, including the revision, deployment group, and deployment configuration\.

### Updating Your Application<a name="deployment-steps-updating-your-app"></a>

You can make updates to your application and then use the AWS CodeDeploy console or call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to push a revision\. 

### Stopped and Failed Deployments<a name="deployment-stop-fail"></a>

You can use the AWS CodeDeploy console or the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command to stop a deployment\. When you attempt to stop the deployment, one of three things will happen:
+ The deployment stops, and the operation returns a status of succeeded\. In this case, no more deployment lifecycle events are run on the deployment group for the stopped deployment\. Some files might have already been copied to, and some scripts might have already been run on, one or more of the instances in the deployment group\.
+ The deployment does not immediately stop, and the operation returns a status of pending\. In this case, some deployment lifecycle events might still be running on the deployment group\. Some files might have already been copied to, and some scripts might have already been run on, one or more of the instances in the deployment group\. After the pending operation is complete, subsequent calls to stop the deployment return a status of succeeded\.
+ The deployment cannot stop, and the operation returns an error\. For more information, see [ErrorInformation](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ErrorInformation.html) and [Common Errors](https://docs.aws.amazon.com/codedeploy/latest/APIReference/CommonErrors.html) in the AWS CodeDeploy API Reference\.

Like stopped deployments, failed deployments might result in some deployment lifecycle events having already been run on one or more of the instances in the deployment group\. To find out why a deployment failed, you can use the AWS CodeDeploy console, call the [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command, or analyze the log file data from the failed deployment\. For more information, see [Application Revision and Log File Cleanup](codedeploy-agent.md#codedeploy-agent-revisions-logs-cleanup) and [View Log Data for AWS CodeDeploy Deployments](deployments-view-logs.md)\.

### Redeployments and Deployment Rollbacks<a name="deployment-rollback"></a>

AWS CodeDeploy implements rollbacks by redeploying, as a new deployment, a previously deployed revision\. 

You can configure a deployment group to automatically roll back deployments when certain conditions are met, including when a deployment fails or an alarm monitoring threshold is met\. You can also override the rollback settings specified for a deployment group in an individual deployment\.

You can also choose to roll back a failed deployment by manually redeploying a previously deployed revision\. 

In all cases, the new or rolled\-back deployment is assigned its own deployment ID\. The list of deployments you can view in the AWS CodeDeploy console shows which ones are the result of an automatic deployment\. 

For more information, see [Redeploy and Roll Back a Deployment with AWS CodeDeploy](deployments-rollback-and-redeploy.md)\.
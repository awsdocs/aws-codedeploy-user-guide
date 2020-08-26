# Working with deployment groups in CodeDeploy<a name="deployment-groups"></a>

You can specify one or more deployment groups for a CodeDeploy application\. Each application deployment uses one of its deployment groups\. The deployment group contains settings and configurations used during the deployment\. Most deployment group settings depend on the compute platform used by your application\. Some settings, such as rollbacks, triggers, and alarms can be configured for deployment groups for any compute platform\.

## Deployment groups in Amazon ECS compute platform deployments<a name="deployment-group-ecs"></a>

In an Amazon ECS deployment, a deployment group specifies the Amazon ECS service, load balancer, optional test listener, and two target groups\. It also specifies when to reroute traffic to the replacement task set and when to terminate the original task set and Amazon ECS application after a successful deployment\.

## Deployment groups in AWS Lambda compute platform deployments<a name="deployment-group-lambda"></a>

In an AWS Lambda deployment, a deployment group defines a set of CodeDeploy configurations for future deployments of an AWS Lambda function\. For example, the deployment group specifies how to route traffic to a new version of a Lambda function\. It also might specify alarms and rollbacks\. A single deployment in an AWS Lambda deployment group can override one or more group configurations\.

## Deployment groups in EC2/On\-Premises Compute Platform deployments<a name="deployment-group-server"></a>

In an EC2/On\-Premises deployment, a deployment group is a set of individual instances targeted for a deployment\. A deployment group contains individually tagged instances, Amazon EC2 instances in Amazon EC2 Auto Scaling groups, or both\. 

In an in\-place deployment, the instances in the deployment group are updated with the latest application revision\. 

In a blue/green deployment, traffic is rerouted from one set of instances to another by deregistering the original instances from a load balancer and registering a replacement set of instances that typically has the latest application revision already installed\.

You can associate more than one deployment group with an application in CodeDeploy\. This makes it possible to deploy an application revision to different sets of instances at different times\. For example, you might use one deployment group to deploy an application revision to a set of instances tagged `Test` where you ensure the quality of the code\. Next, you deploy the same application revision to a deployment group with instances tagged `Staging` for additional verification\. Finally, when you are ready to release the latest application to customers, you deploy to a deployment group that includes instances tagged `Production`\.

You can also use multiple tag groups to further refine the criteria for the instances included in a deployment group\. For information, see [Tagging instances for deployment groups in CodeDeploy](instances-tagging.md)\.

When you use the CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

To view a list of deployment groups already associated with your AWS account, see [View deployment group details with CodeDeploy](deployment-groups-view-details.md)\. 

For information about Amazon EC2 instance tags, see [Working with tags using the console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console)\. For information about on\-premises instances, see [Working with on\-premises instances for CodeDeploy](instances-on-premises.md)\. For information about Amazon EC2 Auto Scaling, see [Integrating CodeDeploy with Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md)\.

## <a name="topiclist-deployment-groups"></a>

**Topics**
+ [Create a deployment group with CodeDeploy](deployment-groups-create.md)
+ [View deployment group details with CodeDeploy](deployment-groups-view-details.md)
+ [Change deployment group settings with CodeDeploy](deployment-groups-edit.md)
+ [Configure advanced options for a deployment group](deployment-groups-configure-advanced-options.md)
+ [Delete a deployment group with CodeDeploy](deployment-groups-delete.md)
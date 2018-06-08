# Working with Deployment Groups in AWS CodeDeploy<a name="deployment-groups"></a>

## Deployment Groups in AWS Lambda Compute Platform Deployments<a name="deployment-group-lambda"></a>

In an AWS Lambda deployment, a deployment group defines a set of AWS CodeDeploy configurations for future serverless Lambda deployment to the group\. For example, the deployment group might specify alarms and rollbacks\. A single deployment in an AWS Lambda deployment group can override one or more group configurations\.

## Deployment Groups in EC2/On\-Premises Compute Platform Deployments<a name="deployment-group-server"></a>

In an EC2/On\-Premises deployment, a deployment group is a set of individual instances targeted for a deployment\. A deployment group contains individually tagged instances, Amazon EC2 instances in Auto Scaling groups, or both\. 

In an in\-place deployment, the instances in the deployment group are updated with the latest application revision\. 

In a blue/green deployment, traffic is rerouted from one set of instances to another by deregistering the original instances from a load balancer and registering a replacement set of instances that typically has the latest application revision already installed\.

You can associate more than one deployment group with an application in AWS CodeDeploy\. This makes it possible to deploy an application revision to different sets of instances at different times\. For example, you might use one deployment group to deploy an application revision to a set of instances tagged `Test` where you ensure the quality of the code\. Next, you deploy the same application revision to a deployment group with instances tagged `Staging` for additional verification\. Finally, when you are ready to release the latest application to customers, you deploy to a deployment group that includes instances tagged `Production`\.

You can also use multiple tag groups to further refine the criteria for the instances included in a deployment group\. For information, see [Tagging Instances for Deployment Groups in AWS CodeDeploy](instances-tagging.md)\.

When you use the AWS CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

To view a list of deployment groups already associated with your AWS account, see [View Deployment Group Details with AWS CodeDeploy](deployment-groups-view-details.md)\. 

For information about Amazon EC2 instance tags, see [Working with Tags Using the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console)\. For information about on\-premises instances, see [Working with On\-Premises Instances for AWS CodeDeploy](instances-on-premises.md)\. For information about Auto Scaling, see [Integrating AWS CodeDeploy with Auto Scaling](integrations-aws-auto-scaling.md)\.

## <a name="topiclist-deployment-groups"></a>

**Topics**
+ [Create a Deployment Group with AWS CodeDeploy](deployment-groups-create.md)
+ [View Deployment Group Details with AWS CodeDeploy](deployment-groups-view-details.md)
+ [Change Deployment Group Settings with AWS CodeDeploy](deployment-groups-edit.md)
+ [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)
+ [Delete a Deployment Group with AWS CodeDeploy](deployment-groups-delete.md)
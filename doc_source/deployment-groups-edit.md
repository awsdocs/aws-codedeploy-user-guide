# Change deployment group settings with CodeDeploy<a name="deployment-groups-edit"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to change the settings of a deployment group\.

**Warning**  
Do not use these steps if you want the deployment group to use a not\-yet\-created custom deployment group\. Instead, follow the instructions in [Create a deployment configuration with CodeDeploy](deployment-configurations-create.md), and then return to this topic\. Do not use these steps if you want the deployment group to use a different, not\-yet\-created service role\. The service role must trust CodeDeploy with, at minimum, the permissions described in [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\. To create and configure a service role with the correct permissions, follow the instructions in [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md), and then return to this topic\.

**Topics**
+ [Change deployment group settings \(console\)](#deployment-groups-edit-console)
+ [Change deployment group settings \(CLI\)](#deployment-groups-edit-cli)

## Change deployment group settings \(console\)<a name="deployment-groups-edit-console"></a>

To use the CodeDeploy console to change deployment group settings:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. In the list of applications, choose the name of the application that is associated with the deployment group you want to change\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. CodeDeploy is supported in these regions only\.

1. Choose the **Deployment groups** tab, and then choose the name of the deployment group you want to change\.

1. On the **Depoyment group** page, choose **Edit**\.

1. Edit the deployment group options as needed\.

   For information about deployment group components, see [Create a deployment group with CodeDeploy](deployment-groups-create.md)\.

1. Choose **Save changes**\.

## Change deployment group settings \(CLI\)<a name="deployment-groups-edit-cli"></a>

To use the AWS CLI to change deployment group settings, call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, specifying:
+ For EC2/On\-Premises and AWS Lambda deployments:
  + The application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
  + The current deployment group name\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
  + \(Optional\) A different deployment group name\.
  + \(Optional\) A different Amazon Resource Name \(ARN\) that corresponds to a service role that allows CodeDeploy to act on your AWS account's behalf when interacting with other AWS services\. To get the service role ARN, see [Get the service role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\. For more information about service roles, see [Roles terms and concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/cross-acct-access.html) in *IAM User Guide*\.
  + \(Optional\) The name of the deployment configuration\. To view a list of deployment configurations, see [View deployment configuration details with CodeDeploy](deployment-configurations-view-details.md)\. \(If not specified, CodeDeploy uses a default deployment configuration\.\)
  + \(Optional\) Commands to add one or more existing CloudWatch alarms to the deployment group that are activated if a metric specified in an alarm falls below or exceeds a defined threshold\.
  + \(Optional\) Commands for a deployment to roll back to the last known good revision when a deployment fails or a CloudWatch alarm is activated\.
  + \(Optional\) Commands to create or update a trigger that publishes to a topic in Amazon Simple Notification Service, so that subscribers to that topic receive notifications about deployment and instance events in this deployment group\. For information, see [Monitoring deployments with Amazon SNS event notifications](monitoring-sns-event-notifications.md)\.
+ For EC2/On\-Premises deployments only:
  + \(Optional\) Replacement tags or tag groups that uniquely identify the instances to be included in the deployment group\.
  + \(Optional\) The names of replacement Amazon EC2 Auto Scaling groups to be added to the deployment group\.
+ For Amazon ECS deployments only:
  +  The Amazon ECS service to deploy\. 
  +  Load balancer information, including the Application Load Balancer or Network Load Balancer, the target groups required for an Amazon ECS deployment, and production and optional test listener information\. 
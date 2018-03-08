# Change Deployment Group Settings with AWS CodeDeploy<a name="deployment-groups-edit"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to change the settings of a deployment group\.

**Warning**  
Do not use these steps if you want the deployment group to use a not\-yet\-created custom deployment group\. Instead, follow the instructions in [Create a Deployment Configuration with AWS CodeDeploy](deployment-configurations-create.md), and then return to this topic\. Do not use these steps if you want the deployment group to use a different, not\-yet\-created service role\. The service role must trust AWS CodeDeploy with, at minimum, the permissions described in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. To create and configure a service role with the correct permissions, follow the instructions in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md), and then return to this topic\.


+ [Change Deployment Group Settings \(Console\)](#deployment-groups-edit-console)
+ [Change Deployment Group Settings \(CLI\)](#deployment-groups-edit-cli)

## Change Deployment Group Settings \(Console\)<a name="deployment-groups-edit-console"></a>

To use the AWS CodeDeploy console to change deployment group settings:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. Choose **Applications**\.

1. In the list of applications, choose the application that is associated with the deployment group you want to change\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy is supported in these regions only\.

1. On the **Application details** page, in **Deployment groups**, choose the button next to the deployment group you want to change\.

1. On the **Actions** menu, choose **Edit**\.

1. Revise the deployment group options as needed\.

   For information about deployment group components, see [Create a Deployment Group with AWS CodeDeploy](deployment-groups-create.md)\.

1. If you want to deploy the last successful revision to the deployment group, select **Deploy changes made to *deployment group name***, and then choose **Save**\. When prompted, choose **Deploy**\. AWS CodeDeploy updates the deployment group's information, starts a deployment of the last successful revision to the deployment group based on changes you specified, and displays the **Deployments** page\.
**Note**  
The **Deploy changes made to *deployment group name*** check box appears only if there was a successful deployment to this deployment group\.

1. If you want to update the deployment group's information with your changes, but do not want to deploy any applications to the deployment group at this time, clear **Deploy changes made to *deployment group name***, and then choose **Save**\. AWS CodeDeploy updates the deployment group's information, but does not deploy any applications to the deployment group\.

## Change Deployment Group Settings \(CLI\)<a name="deployment-groups-edit-cli"></a>

To use the AWS CLI to change deployment group settings, call the [update\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, specifying:

+ For EC2/On\-Premises and AWS Lambda deployments:

  + The application name\. To view a list of application names, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.

  + The current deployment group name\. To view a list of deployment group names, call the [list\-deployment\-groups](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.

  + \(Optional\) A different deployment group name\.

  + \(Optional\) A different Amazon Resource Name \(ARN\) that corresponds to a service role that allows AWS CodeDeploy to act on your AWS account's behalf when interacting with other AWS services\. To get the service role ARN, see [Get the Service Role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\. For more information about service roles, see [Roles Terms and Concepts](http://docs.aws.amazon.com/IAM/latest/UserGuide/cross-acct-access.html) in *IAM User Guide*\.

  + \(Optional\) The name of the deployment configuration\. To view a list of deployment configurations, see [View Deployment Configuration Details with AWS CodeDeploy](deployment-configurations-view-details.md)\. \(If not specified, AWS CodeDeploy uses a default deployment configuration\.\)

  + \(Optional\) Commands to add one or more existing CloudWatch alarms to the deployment group that are activated if a metric specified in an alarm falls below or exceeds a defined threshold\.

  + \(Optional\) Commands for a deployment to roll back to the last known good revision when a deployment fails or a CloudWatch alarm is activated\.

  + \(Optional\) Commands to create or update a trigger that publishes to a topic in Amazon Simple Notification Service, so that subscribers to that topic receive notifications about deployment and instance events in this deployment group\. For information, see [Monitoring Deployments with Amazon SNS Event Notifications](monitoring-sns-event-notifications.md)\.

+ For EC2/On\-Premises deployments only:

  + \(Optional\) Replacement tags or tag groups that uniquely identify the instances to be included in the deployment group\.

  + \(Optional\) The names of replacement Auto Scaling groups to be added to the deployment group\.
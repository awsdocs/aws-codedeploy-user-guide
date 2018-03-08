# View Instance Details with AWS CodeDeploy<a name="instances-view-details"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to view details about instances used in a deployment\.

For information about using AWS CodeDeploy API actions to view instances, see [GetDeploymentInstance](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_GetDeploymentInstance.html), [ListDeploymentInstances](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ListDeploymentInstances.html), and [ListOnPremisesInstances](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_ListOnPremisesInstances.html)\.


+ [View Instance Details \(Console\)](#instances-view-details-console)
+ [View Instance Details \(CLI\)](#instances-view-details-cli)

## View Instance Details \(Console\)<a name="instances-view-details-console"></a>

To view instance details:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Deployments**\. 
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy is supported in these regions only\.

1. To display deployment details, choose the arrow next to the deployment ID that corresponds to the instance\. 

1. In **Instances**, choose **View all instances**\. 

1. To see information about individual deployment lifecycle events for an instance, on the deployment details page, in the **Events** column, choose **View events**\. 
**Note**  
If **Failed** is displayed for any of the lifecycle events, on the instance details page, choose **View logs**, **View in EC2**, or both\. You can find troubleshooting tips in [Troubleshoot Instance Issues](troubleshooting-ec2-instances.md)\.

1. If you want to see more information about an Amazon EC2 instance, but **View in EC2** is not available on the instance details page, return to the deployment details page, and in the **Instance ID** column, choose the ID of the Amazon EC2 instance\.

## View Instance Details \(CLI\)<a name="instances-view-details-cli"></a>

To use the AWS CLI to view instance details, call either the `get-deployment-instance` command or the `list-deployment-instances` command\.

To view details about a single instance, call the [get\-deployment\-instance](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command, specifying: 

+ The unique deployment ID\. To get the deployment ID, call the [list\-deployments](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.

+ The unique instance ID\. To get the instance ID, call the [list\-deployment\-instances](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-instances.html) command\.

To view a list of IDs for instances used in a deployment, call the [list\-deployment\-instances](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-instances.html) command, specifying:

+ The unique deployment ID\. To get the deployment ID, call the [list\-deployments](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.

+ Optionally, whether to include only specific instance IDs by their deployment status\. \(If not specified, all matching instance IDs will be listed, regardless of their deployment status\.\)
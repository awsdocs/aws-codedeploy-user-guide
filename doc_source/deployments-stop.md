# Stop a Deployment with AWS CodeDeploy<a name="deployments-stop"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to stop deployments associated with your AWS account\.

**Warning**  
Stopping an EC2/On\-Premises deployment can leave some or all of the instances in your deployment groups in an indeterminate deployment state\. For more information, see [Stopped and Failed Deployments](deployment-steps.md#deployment-stop-fail)\. 


+ [Stop a deployment \(console\)](#deployments-stop-console)
+ [Stop a deployment \(CLI\)](#deployments-stop-cli)

## Stop a deployment \(console\)<a name="deployments-stop-console"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Deployments**\. 
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy is supported in these regions only\.

1. In the **Actions** column for the deployment you want to stop, choose **Stop**\.
**Note**  
If a **Stop** button does not appear in the **Actions** column, the deployment has progressed to a point where it cannot be stopped\.

## Stop a deployment \(CLI\)<a name="deployments-stop-cli"></a>

Call the [stop\-deployment](http://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command, specifying the deployment ID\. To view a list of deployment IDs, call the [list\-deployments](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.
# Stop a deployment with CodeDeploy<a name="deployments-stop"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to stop deployments associated with your AWS account\.

**Warning**  
Stopping an EC2/On\-Premises deployment can leave some or all of the instances in your deployment groups in an indeterminate deployment state\. For more information, see [Stopped and failed deployments](deployment-steps-server.md#deployment-stop-fail)\. 

**Topics**
+ [Stop a deployment \(console\)](#deployments-stop-console)
+ [Stop a deployment \(CLI\)](#deployments-stop-cli)

**Note**  
If your deployment is a blue/green deployment through AWS CloudFormation, you cannot perform this task in the CodeDeploy console\. Go to the AWS CloudFormation console to perform this task\. 

## Stop a deployment \(console\)<a name="deployments-stop-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Deployments**\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. CodeDeploy is supported in these regions only\.

1. Choose the deployment you want to stop do one of the following:

   1. Choose **Stop deployment** to stop the deployment without a rollback\.

   1. Choose **Stop and roll back deployment** to stop and roll back the deployment

   For more information, see [Redeploy and roll back a deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.
**Note**  
If **Stop deployment** and **Stop and roll back deployment** are unavailable, the deployment has progressed to a point where it cannot be stopped\.

## Stop a deployment \(CLI\)<a name="deployments-stop-cli"></a>

Call the [stop\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/stop-deployment.html) command, specifying the deployment ID\. To view a list of deployment IDs, call the [list\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.
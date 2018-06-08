# Delete a Deployment Group with AWS CodeDeploy<a name="deployment-groups-delete"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to delete deployment groups associated with your AWS account\.

**Warning**  
If you delete a deployment group, all details associated with that deployment group will also be deleted from AWS CodeDeploy\. The instances used in the deployment group will remain unchanged\. This action cannot be undone\.

**Topics**
+ [Delete a Deployment Group \(Console\)](#deployment-groups-delete-console)
+ [Delete a Deployment Group \(CLI\)](#deployment-groups-delete-cli)

## Delete a Deployment Group \(Console\)<a name="deployment-groups-delete-console"></a>

To use the AWS CodeDeploy console to delete a deployment group:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Applications**\.

1. In the list of applications, choose the name of the application associated with the deployment group\.

1. On the **Application details** page, in **Deployment groups**, choose the button next to the deployment group you want to delete\.

1. On the **Actions** menu, choose **Delete**\. 

1. When prompted, type the name of the deployment group to confirm you want to delete it, and then choose **Delete**\.

## Delete a Deployment Group \(CLI\)<a name="deployment-groups-delete-cli"></a>

To use the AWS CLI to delete a deployment group, call the [delete\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command, specifying:
+ The name of the application associated with the deployment group\. To view a list of application names, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The name of the deployment group associated with the application\. To view a list of deployment group names, call the [list\-deployment\-groups](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
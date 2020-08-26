# Delete a deployment group with CodeDeploy<a name="deployment-groups-delete"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to delete deployment groups associated with your AWS account\.

**Warning**  
If you delete a deployment group, all details associated with that deployment group will also be deleted from CodeDeploy\. The instances used in the deployment group will remain unchanged\. This action cannot be undone\.

**Topics**
+ [Delete a deployment group \(console\)](#deployment-groups-delete-console)
+ [Delete a deployment group \(CLI\)](#deployment-groups-delete-cli)

## Delete a deployment group \(console\)<a name="deployment-groups-delete-console"></a>

To use the CodeDeploy console to delete a deployment group:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. In the list of applications, choose the name of the application associated with the deployment group\.

1. On the **Application details** page, on the **Deployment groups** tab, choose the name of the deployment group you want to delete\.

1. On the **Deployment details** page, choose **Delete**\. 

1. When prompted, type the name of the deployment group to confirm you want to delete it, and then choose **Delete**\.

## Delete a deployment group \(CLI\)<a name="deployment-groups-delete-cli"></a>

To use the AWS CLI to delete a deployment group, call the [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command, specifying:
+ The name of the application associated with the deployment group\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The name of the deployment group associated with the application\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
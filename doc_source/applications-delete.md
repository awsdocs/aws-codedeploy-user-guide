# Delete an application in CodeDeploy<a name="applications-delete"></a>

You can use the CodeDeploy console, the AWS CLI, or a CodeDeploy API action to delete applications\. For information about using the CodeDeploy API action, see [DeleteApplication](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteApplication.html)\.

**Warning**  
Deleting an application removes information about the application from the CodeDeploy system, including all related deployment group information and deployment details\. Deleting an application created for an EC2/On\-Premises deployment does not remove any application revisions from instances nor does it delete revisions from Amazon S3 buckets\. Deleting an application created for an EC2/On\-Premises deployment does not terminate any Amazon EC2 instances or deregister any on\-premises instances\. This action cannot be undone\.

**Topics**
+ [Delete an application \(console\)](#applications-delete-console)
+ [Delete an application \(AWS CLI\)](#applications-delete-cli)

## Delete an application \(console\)<a name="applications-delete-console"></a>

To use the CodeDeploy console to delete an application:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. In the list of applications, choose the button next to the application you want to delete, and then choose **Delete**\.

1. When prompted, enter the name of the application to confirm you want to delete it, and then choose **Delete**\. 

## Delete an application \(AWS CLI\)<a name="applications-delete-cli"></a>

To use the AWS CLI to delete an application, call the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command, specifying the application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
# Delete an Application in AWS CodeDeploy<a name="applications-delete"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or an AWS CodeDeploy API action to delete applications\. For information about using the AWS CodeDeploy API action, see [DeleteApplication](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteApplication.html)\.

**Warning**  
Deleting an application removes information about the application from the AWS CodeDeploy system, including all related deployment group information and deployment details\. Deleting an application created for an EC2/On\-Premises deployment does not remove any application revisions from instances nor does it delete revisions from Amazon S3 buckets\. Deleting an application created for an EC2/On\-Premises deployment does not terminate any Amazon EC2 instances or deregister any on\-premises instances\. This action cannot be undone\.


+ [Delete an Application \(Console\)](#applications-delete-console)
+ [Delete an Application \(AWS CLI\)](#applications-delete-cli)

## Delete an Application \(Console\)<a name="applications-delete-console"></a>

To use the AWS CodeDeploy console to delete an application:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page does not appear, on the AWS CodeDeploy menu, choose **Applications**\. 

1. In the list of applications, choose the name of the application you want to delete\.

1. On the **Application details** page, in **Deployment groups**, choose the button next to the deployment group\. On the **Actions** menu, choose **Delete**\. When prompted, type the name of the deployment group to confirm you want to delete it, and then choose **Delete**\. Repeat for any additional deployment groups\.

1. At the bottom of the **Application details** page, choose **Delete application**\.

1. When prompted, type the name of the application to confirm you want to delete it, and then choose **Delete**\. 

## Delete an Application \(AWS CLI\)<a name="applications-delete-cli"></a>

To use the AWS CLI to delete an application, call the [delete\-application](http://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command, specifying the application name\. To view a list of application names, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
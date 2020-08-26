# Step 6: Clean up your "hello, world\!" application and related resources<a name="tutorials-windows-clean-up"></a>

You've now successfully made an update to the "Hello, World\!" code and redeployed the site\. To avoid ongoing charges for resources you created to complete this tutorial, you should delete:
+ Any AWS CloudFormation stacks \(or terminate any Amazon EC2 instances, if you created them outside of AWS CloudFormation\)\.
+ Any Amazon S3 buckets\.
+ The `HelloWorld_App` application in CodeDeploy\.
+ The AWS Systems Manager State Manager association for the CodeDeploy agent\.

You can use the AWS CLI, the AWS CloudFormation, Amazon S3, Amazon EC2, and CodeDeploy consoles, or the AWS APIs to perform the cleanup\.

**Topics**
+ [To use clean up resources \(CLI\)](#tutorials-windows-clean-up-cli)
+ [To clean up resources \(console\)](#tutorials-windows-clean-up-console)
+ [What's next?](#tutorials-windows-clean-up-whats-next)

## To use clean up resources \(CLI\)<a name="tutorials-windows-clean-up-cli"></a>

1. If you used the AWS CloudFormation stack for this tutorial, delete the stack by calling the delete\-stack command against the stack named **CodeDeployDemoStack**\. This terminates all accompanying Amazon EC2 instances and delete all accompanying IAM roles originally created by the stack\.

   ```
   aws cloudformation delete-stack --stack-name CodeDeployDemoStack
   ```

1. To delete the Amazon S3 bucket, call the rm command with the \-\-recursive switch against the bucket named **codedeploydemobucket**\. This deletes the bucket and all objects in the bucket\.

   ```
   aws s3 rm s3://codedeploydemobucket --recursive
   ```

1. To delete the `HelloWorld_App` application from CodeDeploy, call the delete\-application command\. This deletes all associated deployment group records and deployment records for the application\.

   ```
   aws deploy delete-application --application-name HelloWorld_App
   ```

1. To delete the Systems Manager State Manager association, call the delete\-association command\.

   ```
   aws ssm delete-association --assocation-id association-id
   ```

   You can get the *association\-id* by calling the describe\-association command\.

   ```
   aws ssm describe-association --name AWS-ConfigureAWSPackage --targets Key=tag:Name,Values=CodeDeployDemo
   ```

1. If you did not use the AWS CloudFormation stack for this tutorial, call the terminate\-instances command to terminate Amazon EC2 instances you manually created\. Supply the ID of the Amazon EC2 instance to terminate\.

   ```
   aws ec2 terminate-instances --instance-ids instanceId
   ```

## To clean up resources \(console\)<a name="tutorials-windows-clean-up-console"></a>

If you used our AWS CloudFormation template for this tutorial, delete the associated AWS CloudFormation stack\.

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the search box, type the AWS CloudFormation stack name \(for example, **CodeDeployDemoStack**\)\.

1. Select the box beside the stack name\.

1. In the **Actions** menu, choose **Delete Stack**\. This deletes the stack, terminate all accompanying Amazon EC2 instances, and delete all accompanying IAM roles\.

To terminate Amazon EC2 instances you created outside of an AWS CloudFormation stack:

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the **Instances** area, choose **Instances**\.

1. In the search box, type the name of Amazon EC2 instance you want to terminate, and then press **Enter**\.

1. Choose the Amazon EC2 instance\.

1. Choose **Actions**, point to **Instance State**, and then choose **Terminate**\. When prompted, choose **Yes, Terminate**\. Repeat these steps for any additional Amazon EC2 instances\.

To delete the Amazon S3 bucket:

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, browse to and choose the name of the Amazon S3 bucket \(for example, **codedeploydemobucket**\)\.

1. Before you can delete a bucket, you must first delete its contents\. Select all of the files in the bucket, such as **HelloWorld\_App\.zip**\. In the **Actions** menu, choose **Delete**\. When prompted to confirm the deletion, choose **OK**\. 

1. After the bucket is empty, you can delete the bucket\. In the list of buckets, choose the row of the bucket \(but not the bucket name\)\. Choose **Delete bucket**, and when prompted to confirm, choose **OK**\. 

To delete the `HelloWorld_App` application from CodeDeploy:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. Choose **`HelloWorld_App`**\.

1. Choose **Delete application**\.

1. When prompted, enter **Delete**, and then choose **Delete**\. 

To delete the Systems Manager State Manager association:

1. Open the AWS Systems Manager console at https://console\.aws\.amazon\.com/systems\-manager\.

1. In the navigation pane, choose **State Manager**\.

1. Choose the association you created and choose **Delete**\.

## What's next?<a name="tutorials-windows-clean-up-whats-next"></a>

If you've arrived here, you have successfully completed a deployment with CodeDeploy\. Congratulations\!
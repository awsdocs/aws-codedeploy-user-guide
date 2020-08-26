# Step 5: Clean up<a name="tutorial-lambda-clean-up"></a>

To avoid further charges for resources you used during this tutorial, delete the resources created by your AWS SAM template and the CloudWatch logs created by your Lambda validation functions\.

**To delete your AWS CloudFormation stack**

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the **Stacks** column, choose your `my-date-time-app` stack, and then choose **Delete**\.

1. When prompted, choose **Delete stack**\. The Lambda functions, CodeDeploy application and deployment group, and IAM roles created by AWS SAM are deleted\.

**To delete your logs in CloudWatch Logs**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  From the navigation pane, choose **Logs**\. 

1.  From the list of log groups, choose the button next to **/aws/lambda/CodeDeployHook\_beforeAllowTraffic**\. 

1.  From **Actions**, choose **Delete log group**, and then choose **Yes, Delete**\. 

1.  From the list of log groups, choose the button next to **/aws/lambda/CodeDeployHook\_afterAllowTraffic**\. 

1.  From **Actions**, choose **Delete log group**, and then choose **Yes, Delete**\. 
# Step 6: Clean up resources<a name="tutorials-on-premises-instance-6-clean-up-resources"></a>

To avoid ongoing charges for resources you created for this tutorial, delete the Amazon S3 bucket if you'll no longer be using it\. You can also clean up associated resources, such as the application and deployment group records in CodeDeploy and the on\-premises instance\.

You can use the AWS CLI or a combination of the CodeDeploy and Amazon S3 consoles and the AWS CLI to clean up resources\. 

## Clean up resources \(CLI\)<a name="tutorials-on-premises-instance-6-clean-up-resources-cli"></a>

**To delete the Amazon S3 bucket**
+ Call the [rm](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) command along with the `--recursive` switch against the bucket \(for example, `codedeploydemobucket`\)\. The bucket and all objects in the bucket will be deleted\. 

  ```
  aws s3 rm s3://your-bucket-name --recursive
  ```

**To delete the application and deployment group records in CodeDeploy**
+ Call the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command against the application \(for example, `CodeDeploy-OnPrem-App`\)\. The records for the deployment and deployment group will be deleted\. 

  ```
  aws deploy delete-application --application-name your-application-name
  ```<a name="tutorials-on-premises-instance-6-clean-up-resources-deregister-cli"></a>

**To deregister the on\-premises instance and delete the IAM user**
+ Call the [deregister](https://docs.aws.amazon.com/cli/latest/reference/deploy/deregister.html) command against the on\-premises instance and region:

  ```
  aws deploy deregister --instance-name your-instance-name --delete-iam-user --region your-region
  ```
**Note**  
If you do not want to delete the IAM user associated with this on\-premises instance, use the `--no-delete-iam-user` option instead\.

**To uninstall the CodeDeploy agent and remove the configuration file from the on\-premises instance**
+ From the on\-premises instance, call the [uninstall](https://docs.aws.amazon.com/cli/latest/reference/deploy/uninstall.html) command:

  ```
  aws deploy uninstall
  ```

You have now completed all of the steps to clean up the resources used for this tutorial\.

## Clean up resources \(console\)<a name="tutorials-on-premises-instance-6-clean-up-resources-console"></a>

**To delete the Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the icon next to the bucket you want to delete \(for example, `codedeploydemobucket`\), but do not choose the bucket itself\.

1. Choose **Actions**, and then choose **Delete**\. 

1. When prompted to delete the bucket, choose **OK**\. 

**To delete the application and deployment group records in CodeDeploy**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane choose **Applications**\.

1. Choose the name of the application you want to delete \(for example, `CodeDeploy-OnPrem-App`\) and then choose **Delete application**\.

1. When prompted, enter the name of the application to confirm you want to delete it, and then choose **Delete**\. 

You cannot use the AWS CodeDeploy console to deregister the on\-premises instance or uninstall the CodeDeploy agent\. Follow the instructions in [To deregister the on\-premises instance and delete the IAM user ](#tutorials-on-premises-instance-6-clean-up-resources-deregister-cli)\.
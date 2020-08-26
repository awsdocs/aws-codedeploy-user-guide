# Step 4: Deploy your WordPress application<a name="tutorials-wordpress-deploy-application"></a>

Now you deploy the sample WordPress application revision you uploaded to Amazon S3\. You can use the AWS CLI or the CodeDeploy console to deploy the revision and monitor the deployment's progress\. After the application revision is successfully deployed, you check the results\.

**Topics**
+ [Deploy your application revision with CodeDeploy](#tutorials-wordpress-deploy-application-create-deployment)
+ [Monitor and troubleshoot your deployment](#tutorials-wordpress-deploy-application-monitor)
+ [Verify your deployment](#tutorials-wordpress-deploy-application-verify-deployment)

## Deploy your application revision with CodeDeploy<a name="tutorials-wordpress-deploy-application-create-deployment"></a>

Use the AWS CLI or the console to deploy your application revision\.

**Topics**
+ [To deploy your application revision \(CLI\)](#tutorials-wordpress-deploy-application-create-deployment-cli)
+ [To deploy your application revision \(console\)](#tutorials-wordpress-deploy-application-create-deployment-console)

### To deploy your application revision \(CLI\)<a name="tutorials-wordpress-deploy-application-create-deployment-cli"></a>

1. The deployment needs a deployment group\. However, before you create the deployment group, you need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a service role \(CLI\) ](getting-started-create-service-role.md#getting-started-create-service-role-cli) to create a service role\. To get the ARN of the service role, see [Get the service role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\.

1. Now that you have the service role ARN, call the create\-deployment\-group command to create a deployment group named **WordPress\_DepGroup**, associated with the application named **WordPress\_App**, using the Amazon EC2 tag named **CodeDeployDemo** and deployment configuration named **CodeDeployDefault\.OneAtATime**:

   ```
   aws deploy create-deployment-group \
     --application-name WordPress_App \
     --deployment-group-name WordPress_DepGroup \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --ec2-tag-filters Key=Name,Value=CodeDeployDemo,Type=KEY_AND_VALUE \
     --service-role-arn serviceRoleARN
   ```
**Note**  
The [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command provides support for creating triggers that result in the sending of Amazon SNS notifications to topic subscribers about specified events in deployments and instances\. The command also supports options for automatically rolling back deployments and setting up alarms to stop deployments when monitoring thresholds in Amazon CloudWatch alarms are met\. Commands for these actions are not included in this tutorial\.

1. Before you create a deployment, the instances in your deployment group must have the CodeDeploy agent installed\. You can install the agent from the command line with AWS Systems Manager with the following command:

   ```
   aws ssm create-association \
     --name AWS-ConfigureAWSPackage \
     --targets Key=tag:Name,Values=CodeDeployDemo \
      --parameters action=Install, name=AWSCodeDeployAgent \
     --schedule-expression "cron(0 2 ? * SUN *)"
   ```

   This command creates an association in Systems Manager State Manager that will install the CodeDeploy agent and then attempt to update it at 2:00 every Sunday morning\. For more information about the CodeDeploy agent, see [ Working with the CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent.html)\. For more information about Systems Manager, see [What is AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)\.

1. Now call the create\-deployment command to create a deployment associated with the application named **WordPress\_App**, the deployment configuration named **CodeDeployDefault\.OneAtATime**, and the deployment group named **WordPress\_DepGroup**, using the application revision named **WordPressApp\.zip** in the bucket named **codedeploydemobucket**:

   ```
   aws deploy create-deployment \
     --application-name WordPress_App \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --deployment-group-name WordPress_DepGroup \
     --s3-location bucket=codedeploydemobucket,bundleType=zip,key=WordPressApp.zip
   ```

### To deploy your application revision \(console\)<a name="tutorials-wordpress-deploy-application-create-deployment-console"></a>

1. Before you use the CodeDeploy console to deploy your application revision, you need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a service role \(console\) ](getting-started-create-service-role.md#getting-started-create-service-role-console) to create a service role\. To get the ARN of the service role, see [Get the service role ARN \(console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Now that you have the ARN, use the CodeDeploy console to deploy your application revision:

   Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. In the list of applications, choose **WordPress\_App**\.

1. On the **Deployment groups** tab, choose **Create deployment group**\.

1. In **Deployment group name**, enter **WordPress\_DepGroup**\.

1. Under **Deployment type**, choose **In\-place deployment**\.

1. In **Environment configuration**, select **Amazon EC2 instances**\.

1. In **Agent configuration with AWS Systems Manager**, keep the defaults\.

1. In **Key**, enter **Name**\.

1. In **Value**, enter **CodeDeployDemo**\.
**Note**  
After you type **CodeDeployDemo**, a **1** should appear under **Matching instances** to confirm CodeDeploy found one matching Amazon EC2 instance\.

1. In **Deployment configuration**, choose **CodeDeployDefault\.OneAtATime**\.

1. In **Service role ARN**, choose the service role ARN, and then choose **Create deployment group**\.

1. Choose **Create deployment**\.

1. In **Deployment group** choose **WordPress\_DepGroup**\.

1. Next to **Repository type**, choose **My application is stored in Amazon S3**\. In **Revision location**, enter the location of the sample WordPress application revision you previously uploaded to Amazon S3\. To get the location:

   1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. In the list of buckets, choose **codedeploydemobucket** \(or the name of the bucket where you uploaded your application revision\)\. 

   1. In the list of objects, choose **WordPressApp\.zip**\.

   1. On the **Overview** tab, copy the value of the **Link** field to your clipboard\.

      It might look something like this:

      **https://s3\.amazonaws\.com/codedeploydemobucket/WordPressApp\.zip**

   1. Return to the CodeDeploy console, and in **Revision location**, paste the **Link** field value\.

1. If a message appears in the **File type** list stating the file type could not be detected, choose **\.zip**\.

1. \(Optional\) Type a comment in the **Deployment description** box\. 

1. Expand **Deployment group overrides**, and from **Deployment configuration**, choose **CodeDeployDefault\.OneAtATime**\.

1. Choose **Start deployment**\. Information about your newly created deployment appears on the **Deployments** page\.

## Monitor and troubleshoot your deployment<a name="tutorials-wordpress-deploy-application-monitor"></a>

Use the AWS CLI or the console to monitor and troubleshoot your deployment\.

**Topics**
+ [To monitor and troubleshoot your deployment \(CLI\)](#tutorials-wordpress-deploy-application-monitor-cli)
+ [To monitor and troubleshoot your deployment \(console\)](#tutorials-wordpress-deploy-application-monitor-console)

### To monitor and troubleshoot your deployment \(CLI\)<a name="tutorials-wordpress-deploy-application-monitor-cli"></a>

1. Get the deployment's ID by calling the list\-deployments command against the application named **WordPress\_App** and the deployment group named **WordPress\_DepGroup**:

   ```
   aws deploy list-deployments --application-name WordPress_App --deployment-group-name WordPress_DepGroup --query 'deployments' --output text
   ```

1. Call the get\-deployment command with the deployment ID:

   ```
   aws deploy get-deployment --deployment-id deploymentID --query 'deploymentInfo.status' --output text
   ```

1. The command returns the deployment's overall status\. If successful, the value is `Succeeded`\.

   If the overall status is `Failed`, you can call commands such as [list\-deployment\-instances](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-instances.html) and [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) to troubleshoot\. For more troubleshooting options, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

### To monitor and troubleshoot your deployment \(console\)<a name="tutorials-wordpress-deploy-application-monitor-console"></a>

On the **Deployments** page in the CodeDeploy console, you can monitor your deployment's status in the **Status** column\.

To get more information about your deployment, especially if the **Status** column value has any value other than **Succeeded**:

1. In the **Deployments** table, choose the name of the deployment\. After a deployment fails, a message that describes the reason for the failure is displayed\.

1. In **Instance activity**, more information about the deployment is displayed\. After a deployment fails, you might be able to determine on which Amazon EC2 instances and at which step the deployment failed\.

1. If you want to do more troubleshooting, you can use a technique like the one described in [View instance details with CodeDeploy](instances-view-details.md)\. You can also analyze the deployment log files on an Amazon EC2 instance\. For more information, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

## Verify your deployment<a name="tutorials-wordpress-deploy-application-verify-deployment"></a>

After your deployment is successful, verify your WordPress installation is working\. Use the public DNS address of the Amazon EC2 instance, followed by `/WordPress`, to view your site in a web browser\. \(To get the public DNS value, in the Amazon EC2 console, choose the Amazon EC2 instance, and on the **Description** tab, look for the value of **Public DNS**\.\)

For example, if the public DNS address of your Amazon EC2 instance is **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**, you would use the following URL:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com/WordPress
```

When you view the site in your browser, you should see a WordPress welcome page that looks similar to the following:

![\[WordPress welcome page\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/WordPress-Welcome-Page-013118.png)

 If your Amazon EC2 instance does not have an HTTP inbound rule added to its security group, then the WordPress welcome page does not appear\. If you see a message that says the remote server is not responding, make sure the security group for your Amazon EC2 instance has the inbound rule\. For more information, see [ Add an inbound rule that allows HTTP traffic to your Amazon Linux or RHEL Amazon EC2 instance Add an inbound rule that allows HTTP traffic to your Windows Server Amazon EC2 instance](tutorials-wordpress-launch-instance.md#tutorials-wordpress-launch-instance-add-inbound-rule)\. 
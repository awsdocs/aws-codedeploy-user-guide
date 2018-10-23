# Step 4: Deploy Your WordPress Application<a name="tutorials-wordpress-deploy-application"></a>

Now you deploy the sample WordPress application revision you uploaded to Amazon S3\. You can use the AWS CLI or the AWS CodeDeploy console to deploy the revision and monitor the deployment's progress\. After the application revision is successfully deployed, you check the results\.

**Topics**
+ [Deploy Your Application Revision with AWS CodeDeploy](#tutorials-wordpress-deploy-application-create-deployment)
+ [Monitor and Troubleshoot Your Deployment](#tutorials-wordpress-deploy-application-monitor)
+ [Verify Your Deployment](#tutorials-wordpress-deploy-application-verify-deployment)

## Deploy Your Application Revision with AWS CodeDeploy<a name="tutorials-wordpress-deploy-application-create-deployment"></a>

**Topics**
+ [To deploy your application revision \(CLI\)](#tutorials-wordpress-deploy-application-create-deployment-cli)
+ [To deploy your application revision \(console\)](#tutorials-wordpress-deploy-application-create-deployment-console)

### To deploy your application revision \(CLI\)<a name="tutorials-wordpress-deploy-application-create-deployment-cli"></a>

1. The deployment needs a deployment group\. However, before you create the deployment group, you need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives AWS CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a Service Role \(CLI\) ](getting-started-create-service-role.md#getting-started-create-service-role-cli) to create a service role\. To get the ARN of the service role, see [Get the Service Role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\.

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

1. Now call the create\-deployment command to create a deployment associated with the application named **WordPress\_App**, the deployment configuration named **CodeDeployDefault\.OneAtATime**, and the deployment group named **WordPress\_DepGroup**, using the application revision named **WordPressApp\.zip** in the bucket named **codedeploydemobucket**:

   ```
   aws deploy create-deployment \
     --application-name WordPress_App \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --deployment-group-name WordPress_DepGroup \
     --s3-location bucket=codedeploydemobucket,bundleType=zip,key=WordPressApp.zip
   ```

### To deploy your application revision \(console\)<a name="tutorials-wordpress-deploy-application-create-deployment-console"></a>

1. Before you use the AWS CodeDeploy console to deploy your application revision, you need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives AWS CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a Service Role \(Console\) ](getting-started-create-service-role.md#getting-started-create-service-role-console) to create a service role\. To get the ARN of the service role, see [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Now that you have the ARN, use the AWS CodeDeploy console to deploy your application revision:

   Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page is not displayed, on the AWS CodeDeploy menu, choose **Applications**\.

1. In the list of applications, choose **WordPress\_App**\.

1. Under **Deployment groups**, choose **Create deployment group**\.

1. In **Deployment group name**, type **WordPress\_DepGroup**\.

1. Under **Deployment type**, choose **In\-place deployment**\.

1. In **Environment configuration**, choose the **Amazon EC2 instances** tab\.

1. In the **Key** box, type **Name**\.

1. In the **Value** box, type **CodeDeployDemo**\.
**Note**  
After you type **CodeDeployDemo**, a **1** should appear under **Instances** to confirm AWS CodeDeploy found one matching Amazon EC2 instance\.

1. In the **Deployment configuration** drop\-down list, choose **CodeDeployDefault\.OneAtATime**\.

1. In the **Service role ARN** drop\-down list, choose the service role ARN, and then choose **Create deployment group**\.

1. On the **Application details** page, select the button next to the new deployment group\. From the **Actions** menu, choose **Deploy new revision**\.

1. In the **Application** drop\-down list, choose **WordPress\_App**\.

1. In the **Deployment group** drop\-down list, choose **WordPress\_DepGroup**\.

1. Next to **Repository type**, choose **My application is stored in Amazon S3**\. In **Revision location**, type the location of the sample WordPress application revision you previously uploaded to Amazon S3\. To get the location:

   1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. In the list of buckets, choose **codedeploydemobucket** \(or the name of the bucket where you uploaded your application revision\)\. 

   1. In the list of objects, choose **WordPressApp\.zip**\.

   1. On the **Overview** tab, copy the value of the **Link** field to your clipboard\.

      It might look something like this:

      **https://s3\.amazonaws\.com/codedeploydemobucket/WordPressApp\.zip**

   1. Return to the AWS CodeDeploy console, and in **Revision location**, paste the **Link** field value\.

1. If a message appears in the **File type** list stating the file type could not be detected, choose **\.zip**\.

1. \(Optional\) Type a comment in the **Deployment description** box\. 

1. From the **Deployment configuration** drop\-down list, choose **CodeDeployDefault\.OneAtATime**\.

1. Choose **Deploy**\. Information about your newly created deployment appears on the **Deployments** page\.
**Note**  
To get the current status of the deployment, choose the **Refresh** button next to the table\.

## Monitor and Troubleshoot Your Deployment<a name="tutorials-wordpress-deploy-application-monitor"></a>

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

On the **Deployments** page in the AWS CodeDeploy console, you can monitor your deployment's status in the **Status** column\.

**Note**  
To get the current status of the deployment, choose the **Refresh** button above the table\.

To get more information about your deployment, especially if the **Status** column value has any value other than **Succeeded**:

1. In the **Deployments** table, choose the arrow next to the deployment ID\. After a deployment fails, a message that describes the reason for the failure appears in **Details**\.

1. In **Instances**, choose **View all instances**\. More information about the deployment is displayed\. After a deployment fails, you might be able to determine on which Amazon EC2 instances and at which step the deployment failed\.
**Note**  
If you don't see **Instances**, choose the **Refresh** button above the table\. After the **Status** column changes from **In progress** to **Created**, **Instances** should appear\.

1. If you want to do more troubleshooting, you can use a technique like the one described in [View Instance Details with AWS CodeDeploy](instances-view-details.md)\. You can also analyze the deployment log files on an Amazon EC2 instance\. For more information, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

## Verify Your Deployment<a name="tutorials-wordpress-deploy-application-verify-deployment"></a>

After your deployment is successful, verify your WordPress installation is working\. Use the public DNS address of the Amazon EC2 instance, followed by `/WordPress`, to view your site in a web browser\. \(To get the public DNS value, in the Amazon EC2 console, choose the Amazon EC2 instance, and on the **Description** tab, look for the value of **Public DNS**\.\)

For example, if the public DNS address of your Amazon EC2 instance is **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**, you would use the following URL:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com/WordPress
```

When you view the site in your browser, you should see a WordPress welcome page that looks similar to the following:

![\[WordPress welcome page\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/WordPress-Welcome-Page-013118.png)

 If your Amazon EC2 instance does not have an HTTP inbound rule added to its security group, then the WordPress welcome page does not appear\. If you see a message that says the remote server is not responding, make sure the security group for your Amazon EC2 instance has the inbound rule\. For more information, see [ Add Inbound Rule Allowing HTTP Traffic to Your Amazon Linux or RHEL Amazon EC2 Instance](tutorials-wordpress-launch-instance.md#tutorials-wordpress-launch-instance-add-inbound-rule)\. 
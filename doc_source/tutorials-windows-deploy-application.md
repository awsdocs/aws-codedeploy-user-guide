# Step 4: Deploy Your "Hello, World\!" Application<a name="tutorials-windows-deploy-application"></a>

Now you will deploy the sample "Hello, World\!" application revision you uploaded to Amazon S3\. You will use the AWS CLI or the AWS CodeDeploy console to deploy the revision and monitor the deployment's progress\. After the application revision is successfully deployed, you will check the results\.


+ [Deploy Your Application Revision with AWS CodeDeploy](#tutorials-windows-deploy-application-create-deployment)
+ [Monitor and Troubleshoot Your Deployment](#tutorials-windows-deploy-application-monitor)
+ [Verify Your Deployment](#tutorials-windows-deploy-application-verify)

## Deploy Your Application Revision with AWS CodeDeploy<a name="tutorials-windows-deploy-application-create-deployment"></a>


+ [To deploy your application revision \(CLI\)](#tutorials-windows-deploy-application-create-deployment-cli)
+ [To deploy your application revision \(console\)](#tutorials-windows-deploy-application-create-deployment-console)

### To deploy your application revision \(CLI\)<a name="tutorials-windows-deploy-application-create-deployment-cli"></a>

1. First, the deployment needs a deployment group\. However, before you create the deployment group, you need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives AWS CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a Service Role \(CLI\) ](getting-started-create-service-role.md#getting-started-create-service-role-cli) to create a service role\. To get the ARN of the service role, see [Get the Service Role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\.

1. Now that you have the ARN, call the create\-deployment\-group command to create a deployment group named **HelloWorld\_DepGroup**, associated with the application named **HelloWorld\_App**, using the Amazon EC2 instance tag named **CodeDeployDemo** and deployment configuration named **CodeDeployDefault\.OneAtATime**, with the service role ARN:

   ```
   aws deploy create-deployment-group --application-name HelloWorld_App --deployment-group-name HelloWorld_DepGroup --deployment-config-name CodeDeployDefault.OneAtATime --ec2-tag-filters Key=Name,Value=CodeDeployDemo,Type=KEY_AND_VALUE --service-role-arn serviceRoleARN
   ```
**Note**  
The [create\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command provides support for creating triggers that result in the sending of Amazon SNS notifications to topic subscribers about specified events in deployments and instances\. The command also supports options for automatically rolling back deployments and setting up alarms to stop deployments when monitoring thresholds in Amazon CloudWatch Alarms are met\. Commands for these actions are not included in this tutorial\.

1. Now call the create\-deployment command to create a deployment associated with the application named **HelloWorld\_App**, the deployment configuration named **CodeDeployDefault\.OneAtATime**, and the deployment group named **HelloWorld\_DepGroup**, using the application revision named **HelloWorld\_App\.zip** in the bucket named **codedeploydemobucket**:

   ```
   aws deploy create-deployment --application-name HelloWorld_App --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name HelloWorld_DepGroup --s3-location bucket=codedeploydemobucket,bundleType=zip,key=HelloWorld_App.zip
   ```

### To deploy your application revision \(console\)<a name="tutorials-windows-deploy-application-create-deployment-console"></a>

1. Before you use the AWS CodeDeploy console to deploy your application revision, you will need a service role ARN\. A service role is an IAM role that gives a service permission to act on your behalf\. In this case, the service role gives AWS CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their Amazon EC2 instance tags\.

   You should have already followed the instructions in [Create a Service Role \(Console\) ](getting-started-create-service-role.md#getting-started-create-service-role-console) to create a service role\. To get the ARN of the service role, see [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Now that you have the ARN, you can use the AWS CodeDeploy console to deploy your application revision\.

   Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page is not displayed, on the AWS CodeDeploy menu, choose **Applications**\.

1. In the list of applications, choose **HelloWorld\_App**\.

1. Under **Deployment groups**, choose **Create deployment group**\.

1. In **Deployment group name**, type **HelloWorld\_DepGroup**\.

1. In **Environment configuration**, choose the **Amazon EC2 instances** tab\.

1. In the **Key** box, type **Name**\.

1. In **Value**, type **CodeDeployDemo**\.
**Note**  
After you type **CodeDeployDemo**, a **1** should appear under **Matching instances** to confirm AWS CodeDeploy found one matching Amazon EC2 instance\.

1. In the **Deployment configuration** drop\-down list, choose **CodeDeployDefault\.OneAtATime**\.

1. In the **Service role ARN** drop\-down list, choose the service role ARN, and then choose **Create deployment group**\.

1. On the AWS CodeDeploy menu, choose **Deployments**\.

1. Choose **Create deployment**\.

1. In the **Application** drop\-down list, choose **HelloWorld\_App**\.

1. In the **Deployment group** drop\-down list, choose **HelloWorld\_DepGroup**\.

1. In **Repository type**, choose **My application is stored in Amazon S3**, and then in **Revision location**, type the location of the sample "Hello, World\!" application revision you previously uploaded to Amazon S3\. To get the location:

   1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. In the list of buckets, choose **codedeploydemobucket** \(or the name of the bucket where you uploaded your application revision\)\. 

   1. In the list of objects, choose **HelloWorld\_App\.zip**\.

   1. If the **Properties** pane is not displayed, choose the **Properties** button\.

   1. In the **Properties** pane, copy the value of the **Link** field to your clipboard\.

      It might look something like this:

      **https://s3\.amazonaws\.com/codedeploydemobucket/HelloWorld\_App\.zip**

   1. Return to the AWS CodeDeploy console, and in **Revision Location**, paste the **Link** field value\.

1. If a message appears in the **File type** list stating the file type could not be detected, choose **\.zip** in the list of file types\.

1. \(Optional\) Type a comment in **Deployment description**\.

1. From the **Deployment configuration** drop\-down list, choose **CodeDeployDefault\.OneAtATime**\.

1. Choose **Deploy**\. Information about your newly created deployment appears on the **Deployments** page\.
**Note**  
To update the deployment's current status, choose the **Refresh** button next to the table\.

## Monitor and Troubleshoot Your Deployment<a name="tutorials-windows-deploy-application-monitor"></a>


+ [To monitor and troubleshoot your deployment \(CLI\)](#tutorials-windows-deploy-application-monitor-cli)
+ [To monitor and troubleshoot your deployment \(console\)](#tutorials-windows-deploy-application-monitor-console)

### To monitor and troubleshoot your deployment \(CLI\)<a name="tutorials-windows-deploy-application-monitor-cli"></a>

1. Get the deployment's ID by calling the list\-deployments command against the application named **HelloWorld\_App** and the deployment group named **HelloWorld\_DepGroup**:

   ```
   aws deploy list-deployments --application-name HelloWorld_App --deployment-group-name HelloWorld_DepGroup --query "deployments" --output text
   ```

1. Call the get\-deployment command with the deployment ID:

   ```
   aws deploy get-deployment --deployment-id deploymentID --query "deploymentInfo.status" --output text
   ```

1. The command returns the deployment's overall status\. If successful, the value is `Succeeded`\.

   If the overall status is `Failed`, you can call commands such as [list\-deployment\-instances](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-instances.html) and [get\-deployment\-instance](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) to troubleshoot\. For more troubleshooting options, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

### To monitor and troubleshoot your deployment \(console\)<a name="tutorials-windows-deploy-application-monitor-console"></a>

On the **Deployments** page in the AWS CodeDeploy console, you can monitor your deployment's status in the **Status** column\.

**Note**  
To update the deployment's current status, choose the **Refresh** button\.

To get more information about your deployment, especially if the **Status** column value has any value other than **Succeeded**:

1. In the **Deployments** table, choose the arrow next to the deployment ID\. After a deployment fails, a message that describes the reason for the failure appears in **Details**\.

1. In **Instances**, choose **View all instances**\. More information about the deployment is displayed\. After a deployment fails, you might be able to determine on which Amazon EC2 instances and at which step the deployment failed\.
**Note**  
If you don't see **Instances**, choose the **Refresh** button above the table\. After the **Status** column changes from **In progress** to **Created**, **Instances** should appear\.

1. If you want to do more troubleshooting, you can use a technique like [View Instance Details with AWS CodeDeploy](instances-view-details.md)\. You can also analyze the deployment log files on an Amazon EC2 instance\. For more information, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

## Verify Your Deployment<a name="tutorials-windows-deploy-application-verify"></a>

After your deployment is successful, verify your installation is working\. Use the public DNS address of the Amazon EC2 instance to view the web page in a web browser\. \(To get the public DNS value, in the Amazon EC2 console, choose the Amazon EC2 instance, and on the **Description** tab, look for the value in **Public DNS**\.\)

For example, if the public DNS address of your Amazon EC2 instance is **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**, you would use the following URL:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com/WordPress
```

If successful, you should see a "Hello, World\!" web page\.
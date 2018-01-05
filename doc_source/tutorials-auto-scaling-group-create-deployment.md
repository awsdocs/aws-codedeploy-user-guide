# Step 2: Deploy the Application to the Auto Scaling Group<a name="tutorials-auto-scaling-group-create-deployment"></a>

In this step, you'll deploy the revision to the single Amazon EC2 instance in the Auto Scaling group\.


+ [To create the deployment \(CLI\)](#tutorials-auto-scaling-group-create-deployment-cli)
+ [To create the deployment \(console\)](#tutorials-auto-scaling-group-create-deployment-console)

## To create the deployment \(CLI\)<a name="tutorials-auto-scaling-group-create-deployment-cli"></a>

1. Call the create\-application command to create an application named **SimpleDemoApp**:

   ```
   aws deploy create-application --application-name SimpleDemoApp
   ```

1. You should have already created a service role by following the instructions in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. The service role will give AWS CodeDeploy permission to access your Amazon EC2 instances to expand \(read\) their tags\. You will need the service role ARN\. To get the service role ARN, follow the instructions in [Get the Service Role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\.

1. Now that you have a service role ARN, call the create\-deployment\-group command to create a deployment group named **SimpleDemoDG**, associated with the application named **SimpleDemoApp**, using the Auto Scaling group named **CodeDeployDemo\-AS\-Group** and deployment configuration named **CodeDeployDefault\.OneAtATime**, with the specified service role ARN\.
**Note**  
The [create\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command provides support for creating triggers that result in the sending of Amazon SNS notifications to topic subscribers about specified events in deployments and instances\. The command also supports options for automatically rolling back deployments and setting up alarms to stop deployments when monitoring thresholds in Amazon CloudWatch Alarms are met\. Commands for these actions are excluded from the sample in this tutorial\.

   On local Linux, macOS, or Unix machines:

   ```
   aws deploy create-deployment-group \
     --application-name SimpleDemoApp \
     --auto-scaling-groups CodeDeployDemo-AS-Group \
     --deployment-group-name SimpleDemoDG \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --service-role-arn service-role-arn
   ```

   On local Windows machines:

   ```
   aws deploy create-deployment-group --application-name SimpleDemoApp --auto-scaling-groups CodeDeployDemo-AS-Group --deployment-group-name SimpleDemoDG --deployment-config-name CodeDeployDefault.OneAtATime --service-role-arn service-role-arn
   ```

1. Call the create\-deployment command to create a deployment associated with the application named **SimpleDemoApp**, the deployment configuration named **CodeDeployDefault\.OneAtATime**, the deployment group named **SimpleDemoDG**, using the revision at the specified location\.

   **For Amazon Linux and RHEL Amazon EC2 instances, calling from local Linux, macOS, or Unix machines**

   ```
   aws deploy create-deployment \
     --application-name SimpleDemoApp \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --deployment-group-name SimpleDemoDG \
     --s3-location bucket=bucket-name,bundleType=zip,key=samples/latest/SampleApp_Linux.zip
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Amazon Linux and RHEL Amazon EC2 instances, calling from local Windows machines**

   ```
   aws deploy create-deployment --application-name SimpleDemoApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name SimpleDemoDG --s3-location bucket=bucket-name,bundleType=zip,key=samples/latest/SampleApp_Linux.zip
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Windows Server Amazon EC2 instances, calling from local Linux, macOS, or Unix machines**

   ```
   aws deploy create-deployment \
     --application-name SimpleDemoApp \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --deployment-group-name SimpleDemoDG \
     --s3-location bucket=bucket-name,bundleType=zip,key=samples/latest/SampleApp_Windows.zip
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Windows Server Amazon EC2 instances, calling from local Windows machines**

   ```
   aws deploy create-deployment --application-name SimpleDemoApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name SimpleDemoDG --s3-location bucket=bucket-name,bundleType=zip,key=samples/latest/SampleApp_Windows.zip
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.
**Note**  
Currently, AWS CodeDeploy does not provide a sample revision to deploy to Ubuntu Server Amazon EC2 instances\. To create a revision on your own, see [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\.

1. Call the get\-deployment command to make sure the deployment was successful\.

   Before you call this command, you will need the ID of the deployment, which should have been returned by the call to the create\-deployment command\. If you need to get the deployment ID again, call the list\-deployments command against the application named **SimpleDemoApp** and the deployment group named **SimpleDemoDG**:

   ```
   aws deploy list-deployments --application-name SimpleDemoApp --deployment-group-name SimpleDemoDG --query "deployments" --output text
   ```

   Now, call the get\-deployment command using the deployment ID:

   ```
   aws deploy get-deployment --deployment-id deployment-id --query "deploymentInfo.status" --output text
   ```

   Do not continue until the returned value is `Succeeded`\.

## To create the deployment \(console\)<a name="tutorials-auto-scaling-group-create-deployment-console"></a>

1. You should have already created a service role by following the instructions in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. The service role will give AWS CodeDeploy permission to access your instances to expand \(read\) their tags\. Before you use the AWS CodeDeploy console to deploy your application revision, you will need the service role ARN\. To get the service role ARN, follow the instructions in [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\. 

1. Now that you have the service role ARN, you can use the AWS CodeDeploy console to deploy your application revision\.

   Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page does not appear, on the AWS CodeDeploy menu, choose **Applications**\.

1. Choose **Create application**\.

1. In the **Application name** box, type **SimpleDemoApp**\.

1. In the **Deployment group name** box, type **SimpleDemoDG**\.

1. In **Environment configuration**, on the **Auto Scaling groups** tab, type **CodeDeployDemo\-AS\-Group**\.

1. From the **Deployment configuration** drop\-down list, choose **CodeDeployDefault\.OneAtATime**\.

1. From the **Service role ARN** drop\-down list, choose the service role ARN\.

1. Choose **Create application**\. 

1. In the **Application details** page, in the **Deployment groups** area, next to **SimpleDemoDG**, choose the arrow to see the deployment group details\.

1. Select the button next to **SimpleDemoDG**\. In the **Actions** menu, choose **Deploy new revision**\.

1. In the **Repository type** area, choose **My application is stored in Amazon S3**, and then in the **Revision location** box, type the location of the sample application for your operating system and region\.

   **For Amazon Linux and RHEL Amazon EC2 instances**  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/tutorials-auto-scaling-group-create-deployment.html)

   **For Windows Server Amazon EC2 instances**  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/tutorials-auto-scaling-group-create-deployment.html)

    **For Ubuntu Server Amazon EC2 instances**

   Type the location of your custom application revision stored in Amazon S3\.

1. Leave the **Deployment description** box blank\.

1. With **CodeDeployDefault\.OneAtATime** selected in the **Deployment configuration** drop\-down list, choose **Deploy**\. 
**Note**  
To update the deployment's current status, refresh the page in your browser\.  
If **Failed** appears instead of **Succeeded**, you may want to try some of the techniques in [Monitor and Troubleshoot Your Deployment](tutorials-wordpress-deploy-application.md#tutorials-wordpress-deploy-application-monitor) \(using the application name of **SimpleDemoApp** and the deployment group name of **SimpleDemoDG**\)\.
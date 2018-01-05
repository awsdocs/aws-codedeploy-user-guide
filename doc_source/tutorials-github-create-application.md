# Step 5: Create an Application and Deployment Group<a name="tutorials-github-create-application"></a>

In this step, you will use the AWS CodeDeploy console or the AWS CLI to create an application and deployment group to use to deploy the sample revision from your GitHub repository\.

## Create an application and deployment group \(console\)<a name="tutorials-github-create-application-console"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the **Applications** page, choose **Create application**\.
**Note**  
If you haven't created any applications yet and the AWS CodeDeploy start page is displayed, choose **Get Started Now**, complete a deployment using the Sample deployment wizard, and then return to this topic\.

1. In the **Application name** box, type **CodeDeployGitHubDemo\-App**\.

1. In the **Deployment group name** box, type **CodeDeployGitHubDemo\-DepGrp**\.

1. In **Deployment type**, choose **In\-place deployment**\.

1. In **Environment configuration**, depending on the type of instance you are using, choose the **Amazon EC2 instances** tab or the **On\-premises instances** tab\. In the **Key** and **Value** boxes, type the instance tag key and value that was applied to your instance as part of [Step 4: Provision an Instance](tutorials-github-provision-instance.md)\.

1. In the **Service role ARN** drop\-down list, choose your service role ARN\. \(Follow the instructions in [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console) if you need to find your service role ARN\.\)

1. Choose **Create application**, and continue to the next step\. 

## Create an application and deployment group \(CLI\)<a name="tutorials-github-create-application-cli"></a>

1. Call the create\-application command to create an application in AWS CodeDeploy named `CodeDeployGitHubDemo-App`:

   ```
   aws deploy create-application --application-name CodeDeployGitHubDemo-App
   ```

1. Call the create\-deployment\-group command to create a deployment group named `CodeDeployGitHubDemo-DepGrp`:

   + If you're deploying to an Amazon EC2 instance, *ec2\-tag\-key* is the Amazon EC2 instance tag key that was applied to your Amazon EC2 instance as part of [Step 4: Provision an Instance](tutorials-github-provision-instance.md)\.

   + If you're deploying to an Amazon EC2 instance, *ec2\-tag\-value* is the Amazon EC2 instance tag value that was applied to your Amazon EC2 instance as part of [Step 4: Provision an Instance](tutorials-github-provision-instance.md)\.

   + If you're deploying to an on\-premises instance, *on\-premises\-tag\-key* is the on\-premises instance tag key that was applied to your on\-premises instance as part of [Step 4: Provision an Instance](tutorials-github-provision-instance.md)\.

   + If you're deploying to an on\-premises instance, *on\-premises\-tag\-value* is the on\-premises instance tag value that was applied to your on\-premises instance as part of [Step 4: Provision an Instance](tutorials-github-provision-instance.md)\.

   + *service\-role\-arn* is a service role ARN\. \(Follow the instructions in [Get the Service Role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli) to find the service role ARN\.\)

   ```
   aws deploy create-deployment-group --application-name CodeDeployGitHubDemo-App --ec2-tag-filters Key=ec2-tag-key,Type=KEY_AND_VALUE,Value=ec2-tag-value --on-premises-tag-filters Key=on-premises-tag-key,Type=KEY_AND_VALUE,Value=on-premises-tag-value --deployment-group-name CodeDeployGitHubDemo-DepGrp --service-role-arn service-role-arn
   ```
**Note**  
The [create\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command provides support for creating triggers that result in the sending of Amazon SNS notifications to topic subscribers about specified events in deployments and instances\. The command also supports options for automatically rolling back deployments and setting up alarms to stop deployments when monitoring thresholds in Amazon CloudWatch Alarms are met\. Commands for these actions are excluded from the sample in this tutorial\.
# Step 5: Create an application and deployment group<a name="tutorials-github-create-application"></a>

In this step, you will use the CodeDeploy console or the AWS CLI to create an application and deployment group to use to deploy the sample revision from your GitHub repository\.

## Create an application and deployment group \(console\)<a name="tutorials-github-create-application-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. Choose **Create application**, and then select **Custom application**\.

1. In **Application name**, enter **CodeDeployGitHubDemo\-App**\.

1. In **Compute Platform**, choose **EC2/On\-premises**\.

1. Choose **Create application**\.

1. On the **Deployment groups** tab, choose **Create deployment group**\.

1. In **Deployment group name**, enter **CodeDeployGitHubDemo\-DepGrp**\.

1. In **Service role**, choose the name of your CodeDeploy service role that you created in [Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

1. In **Deployment type**, choose **In\-place**\.

1. In **Environment configuration**, depending on the type of instance you are using, choose **Amazon EC2 instances** or **On\-premises instances**\. For **Key** and **Value**, enter the instance tag key and value that was applied to your instance as part of [Step 4: Provision an instance](tutorials-github-provision-instance.md)\.

1. In **Deployment configuration**, choose **CodeDeployDefault\.AllatOnce**\.

1. In **Load Balancer**, clear **Enable load balancing**\.

1. Expand **Advanced**\.

1. In **Alarms**, select **Ignore alarm configuration**\.

1. Choose **Create deployment group**, and continue to the next step\. 

## Create an application and deployment group \(CLI\)<a name="tutorials-github-create-application-cli"></a>

1. Call the create\-application command to create an application in CodeDeploy named `CodeDeployGitHubDemo-App`:

   ```
   aws deploy create-application --application-name CodeDeployGitHubDemo-App
   ```

1. Call the create\-deployment\-group command to create a deployment group named `CodeDeployGitHubDemo-DepGrp`:
   + If you're deploying to an Amazon EC2 instance, *ec2\-tag\-key* is the Amazon EC2 instance tag key that was applied to your Amazon EC2 instance as part of [Step 4: Provision an instance](tutorials-github-provision-instance.md)\.
   + If you're deploying to an Amazon EC2 instance, *ec2\-tag\-value* is the Amazon EC2 instance tag value that was applied to your Amazon EC2 instance as part of [Step 4: Provision an instance](tutorials-github-provision-instance.md)\.
   + If you're deploying to an on\-premises instance, *on\-premises\-tag\-key* is the on\-premises instance tag key that was applied to your on\-premises instance as part of [Step 4: Provision an instance](tutorials-github-provision-instance.md)\.
   + If you're deploying to an on\-premises instance, *on\-premises\-tag\-value* is the on\-premises instance tag value that was applied to your on\-premises instance as part of [Step 4: Provision an instance](tutorials-github-provision-instance.md)\.
   + *service\-role\-arn* is the service role ARN for the service role that you created in [Create a service role for CodeDeploy](getting-started-create-service-role.md)\. \(Follow the instructions in [Get the service role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli) to find the service role ARN\.\)

   ```
   aws deploy create-deployment-group --application-name CodeDeployGitHubDemo-App --ec2-tag-filters Key=ec2-tag-key,Type=KEY_AND_VALUE,Value=ec2-tag-value --on-premises-tag-filters Key=on-premises-tag-key,Type=KEY_AND_VALUE,Value=on-premises-tag-value --deployment-group-name CodeDeployGitHubDemo-DepGrp --service-role-arn service-role-arn
   ```
**Note**  
The [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command provides support for creating triggers that result in the sending of Amazon SNS notifications to topic subscribers about specified events in deployments and instances\. The command also supports options for automatically rolling back deployments and setting up alarms to stop deployments when monitoring thresholds in Amazon CloudWatch alarms are met\. Commands for these actions are not included in this tutorial\.
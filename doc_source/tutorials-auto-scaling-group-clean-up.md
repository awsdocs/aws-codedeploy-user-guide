[Back to contents](index.md)

# Step 6: Clean Up<a name="tutorials-auto-scaling-group-clean-up"></a>

In this step, you'll delete the Amazon EC2 Auto Scaling group to avoid ongoing charges for resources you used during this tutorial\. Optionally, you can delete the Amazon EC2 Auto Scaling configuration and CodeDeploy deployment component records\.

**Topics**
+ [To clean up resources \(CLI\)](#tutorials-auto-scaling-group-clean-up-cli)
+ [To clean up resources \(console\)](#tutorials-auto-scaling-group-clean-up-console)

## To clean up resources \(CLI\)<a name="tutorials-auto-scaling-group-clean-up-cli"></a>

1. Delete the Amazon EC2 Auto Scaling group by calling the delete\-auto\-scaling\-group command against **CodeDeployDemo\-AS\-Group**\. This will also terminate the Amazon EC2 instances\. 

   ```
   aws autoscaling delete-auto-scaling-group --auto-scaling-group-name CodeDeployDemo-AS-Group --force-delete
   ```

1. Optionally, delete the Amazon EC2 Auto Scaling launch configuration by calling the delete\-launch\-configuration command against the launch configuration named **CodeDeployDemo\-AS\-Configuration**:

   ```
   aws autoscaling delete-launch-configuration --launch-configuration-name CodeDeployDemo-AS-Configuration
   ```

1. Optionally, delete the application from CodeDeploy by calling the delete\-application command against the application named **SimpleDemoApp**\. This will also delete all associated deployment, deployment group, and revision records\. 

   ```
   aws deploy delete-application --application-name SimpleDemoApp
   ```

## To clean up resources \(console\)<a name="tutorials-auto-scaling-group-clean-up-console"></a>

1. Delete the Amazon EC2 Auto Scaling group\. This also terminates the Amazon EC2 instances:

   Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the Amazon EC2 navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**, and then choose the **CodeDeployDemo\-AS\-Group** entry\.

1. Choose **Actions**, choose **Delete**, and then choose **Yes, Delete**\.

1. \(Optional\) Delete the launch configuration\. In the navigation bar, under **Auto Scaling**, choose **Launch Configurations**, and then choose **CodeDeployDemo\-AS\-Configuration**\.

1. Choose **Actions**, choose **Delete launch configuration**, and then choose **Yes, Delete**\.

1. Optionally, delete the application from CodeDeploy\. This will also delete all associated deployment, deployment group, and revision records\. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting Started with CodeDeploy](getting-started-codedeploy.md)\.

   In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. In the list of applications, choose **SimpleDemoApp**\.

1. On the **Application details** page, choose **Delete application**\.

1. When prompted, enter **Delete**, and then choose **Delete**\. 
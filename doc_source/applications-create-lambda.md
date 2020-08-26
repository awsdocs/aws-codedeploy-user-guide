# Create an application for an AWS Lambda function deployment \(console\)<a name="applications-create-lambda"></a>

You can use the CodeDeploy console to create an application for an AWS Lambda function deployment\.

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and choose **Getting started**\.

1. On the **Create application** page, choose **Use CodeDeploy**\.

1. Enter the name of your application in **Application name**\.

1. From **Compute platform**, choose **AWS Lambda**\.

1. Choose **Create application**\.

1. On your application page, from the **Deployment groups** tab, choose **Create deployment group**\.

1. In **Deployment group name**, enter a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group \(including the deployment group name and the deployment configuration\), choose those settings on this page\. Although this new deployment group and the existing deployment group might have the same name, CodeDeploy treats them as separate deployment groups, because each is associated with a separate application\.

1. In **Service role**, choose a service role that grants CodeDeploy access to AWS Lambda\. For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

1.  If you want to use a predefined deployment configuration, choose one from **Deployment configuration**, and then skip to step 12\. To create a custom configuration, continue to the next step\.

   For more information about deployment configurations, see [ Deployment configurations on an AWS Lambda compute platform ](deployment-configurations.md#deployment-configuration-lambda)\.

1. To create a custom configuration, choose **Create deployment configuration**, and then do the following:

   1. For **Deployment configuration name**, enter a name for the configuration\.

   1. From **Type**, choose a configuration type\. If you choose **Canary**, traffic is shifted in two increments\. If you choose **Linear**, traffic is shifted in equal increments, with an equal number of minutes between each increment\.

   1. For **Step**, enter a percentage of traffic, between 1 and 99, to be shifted\. If your configuration type is **Canary**, this is the percentage of traffic that is shifted in the first increment\. The remaining traffic is shifted after the selected interval in the second increment\. If your configuration type is **Linear**, this is the percentage of traffic that is shifted at the start of each interval\.

   1. In **Interval**, enter the number of minutes\. If your configuration type is **Canary**, this is the number of minutes between the first and second traffic shift\. If your configuration type is **Linear**, this is the number of minutes between each incremental shift\.
**Note**  
The maximum length of an AWS Lambda deployment is two days, or 2,880 minutes\. Therefore, the maximum value specified for **Interval** for a canary configuration is 2,800 minutes\. The maximum value for a linear configuration depends on the value for **Step**\. For example, if the step percentage of a linear traffic shift is 25%, then there are four traffic shifts\. The maximum interval value is 2,880 divided by four, or 720 minutes\.

   1. Choose **Create deployment configuration**\.

1. \(Optional\) In **Advanced**, configure any options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For more information, see [Configure advanced options for a deployment group](deployment-groups-configure-advanced-options.md)\. 

1. Choose **Create deployment group**\. 
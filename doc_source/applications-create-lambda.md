# Create an Application for an AWS Lambda Function Deployment \(Console\)<a name="applications-create-lambda"></a>

To use the AWS CodeDeploy console to create an application for a Lambda function deployment:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the AWS CodeDeploy home page appears, choose **Get Started Now**\.

1. Choose **Create application**\.

1. In **Application name**, type a name for the application\. \(In an AWS account, an AWS CodeDeploy application name can be used only once per region\. You can reuse an application name in different regions\.\)

1. From the **Compute platform** drop\-down list, choose **AWS Lambda**\.

1. In **Deployment group name**, type a name for the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group, specify those settings on this page\. You might want to reuse the deployment triggers, rollbacks, or deployment configuration\. Although the new and existing deployment group have the same name, AWS CodeDeploy treats them as separate deployment groups, because they are associated with separate applications\.

1.  From the **Deployment configuration** drop\-down list, choose one of the predefined deployment configurations, and then skip to step 9\.

   For more information about deployment configurations, see [ Deployment Configurations on an AWS Lambda Compute Platform ](deployment-configurations.md#deployment-configuration-lambda)\.

1. To create a custom configuration, choose **Create deployment configuration** and do the following:

   + For **Deployment configuration name**, type a name for the configuration\.

   + \(Optional\) For **Description**, type a description for the configuration\.

   + From the **Type** drop\-down list, choose a configuration type\. If you choose **Canary**, traffic is shifted in two increments\. If you choose **Linear**, traffic is shifted in equal increments, with an equal number of minutes between each increment\.

   + For **Step**, enter a percentage of traffic, between 1 and 99, to be shifted\. If your configuration type is **Canary**, this is the percentage of traffic that is shifted in the first increment\. The remaining traffic is shifted after the selected interval in the second increment\. If your configuration type is **Linear**, this is the percentage of traffic that is shifted at the start of each interval\.

   + In the **Interval** dialog box, enter the number of minutes\. If your configuration type is **Canary**, this is the number of minutes between the first and second traffic shift\. If your configuration typeis **Linear**, this is the number of minutes between each incremental shift\.
**Note**  
The maximum length of an AWS Lambda deployment is two days, or 2,880 minutes\. Therefore, the maximum value specified for **Interval** for a canary configuration is 2,800 minutes\. The maximum value for a linear configuration depends on the value for **Step**\. For example, if the step percentage of a linear traffic shift is 25%, then there are four traffic shifts\. The maximum interval value would be 2,880 divided by four, or 720 minutes\.

   + Choose **Submit**\.

1. \(Optional\) In **Advanced**, configure any options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For more information, see [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)\. 

1. In **Service role ARN**, choose an Amazon Resource Name \(ARN\) for a service role that trusts AWS CodeDeploy with, at minimum, the trust and permissions described in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. To get the service role ARN, see [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Choose **Create application**\. 
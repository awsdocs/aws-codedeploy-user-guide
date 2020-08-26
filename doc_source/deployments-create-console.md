# Create an EC2/On\-Premises Compute Platform deployment \(console\)<a name="deployments-create-console"></a>

This topic shows you how to deploy an application to an Amazon EC2 or on\-premises server using the console\.

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. Do one of the following:
   +  If you want to deploy an application, in the navigation pane, expand **Deploy**, and then choose **Applications**\. Choose the name of the application you want to deploy\. Make sure the **Compute platform** column for your application is **EC2/On\-Premises**\.
   +  If you want to redeploy a deployment, in the navigation pane, expand **Deploy**, and then choose **Deployments**\. Locate the deployment you want to redeploy, and then choose the name of its application in the **Application** column\. Make sure the **Compute platform** column for your deployment is **EC2/On\-Premises**\.

1. On the **Deployments** tab, choose **Create deployment**\.
**Note**  
Your application must have a deployment group before it can be deployed\. If your application does not have a deployment group, on the **Deployment groups** tab, choose **Create deployment group**\. For more information, see [Create a deployment group with CodeDeploy](deployment-groups-create.md)\. 

1. In **Deployment group**, choose a deployment group to use for this deployment\.

1. Next to **Repository type**, choose the repository type your revision is stored in:
   + **My application is stored in Amazon S3** — For information, see [Specify information about a revision stored in an Amazon S3 bucket](deployments-create-console-s3.md), and then return to step 6\. 
   + **My application is stored in GitHub** — For information, see [Specify information about a revision stored in a GitHub repository](deployments-create-console-github.md), and then return to step 6\.

1. \(Optional\) In **Deployment description**, enter a description for this deployment\.

1. \(Optional\) Expand **Override deployment configuration** to choose a deployment configuration to control how traffic is shifted to the Amazon EC2 or on\-premises server that is different from the one specified in the deployment group\.

   For more information, see [Working with deployment configurations in CodeDeploy](deployment-configurations.md)\.

1. 

   1. Select **Don't fail the deployment if the ApplicationStop lifecycle event fails** if you want a deployment to an instance to succeed if the `ApplicationStop` lifecycle event fails\.

   1. Expand **Additional deployment behavior settings** to specify how CodeDeploy handles files in a deployment target location that weren't part of the previous successful deployment\.

      Choose from the following:
      + **Fail the deployment** — An error is reported and the deployment status is changed to `Failed`\.
      + **Overwrite the content** — If a file of the same name exists in the target location, the version from the application revision replaces it\.
      + **Retain the content** — If a file of the same name exists in the target location, the file is kept and the version in the application revision is not copied to the instance\.

      For more information, see [Rollback behavior with existing content](deployments-rollback-and-redeploy.md#deployments-rollback-and-redeploy-content-options)\. 

1. \(Optional\) In **Rollback configuration overrides**, you can specify different automatic rollback options for this deployment than were specified for the deployment group, if any\.

   For information about rollbacks in CodeDeploy, see [Redeployments and deployment rollbacks](deployment-steps-server.md#deployment-rollback) and [Redeploy and roll back a deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.

   Choose from the following:
   + **Roll back when a deployment fails** — CodeDeploy redeploys the last known good revision as a new deployment\.
   + **Roll back when alarm thresholds are met** — If alarms were added to the deployment group, CodeDeploy deploys the last known good revision when one or more of the specified alarms is activated\.
   + **Disable rollbacks** — Do not perform rollbacks for this deployment\.

1. Choose **Start deployment**\. 

   To track the status of your deployment, see [View CodeDeploydeployment details ](deployments-view-details.md)\.

**Topics**
+ [Specify information about a revision stored in an Amazon S3 bucket](deployments-create-console-s3.md)
+ [Specify information about a revision stored in a GitHub repository](deployments-create-console-github.md)
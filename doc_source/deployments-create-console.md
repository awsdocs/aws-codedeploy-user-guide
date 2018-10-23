# Create an EC2/On\-Premises Compute Platform Deployment \(Console\)<a name="deployments-create-console"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. Do one of the following:
   + On the AWS CodeDeploy menu, choose **Deployments**, and then choose **Create deployment**\.
   + On the AWS CodeDeploy menu, choose **Applications**, and then choose the name of the EC2/On\-Premises application you want to deploy a revision to\. You can look in the **Compute platform** column to identify EC2/On\-Premises applications\. On the **Application details**, page, select the button for the deployment group you want to deploy a revision to\. On the **Actions** menu, choose **Deploy new revision**\.

1. In the **Application** list, choose the name of the application you want to use for this deployment\. Make sure EC2/On\-Premises is displayed for the **Compute platform**\.

1. In the **Deployment group** list, choose the name of the deployment group associated with the application\.

1. Next to **Repository type**, choose the repository type your revision is stored in:
   + **My application is stored in Amazon S3** — For information, see [Specify Information About a Revision Stored in an Amazon S3 Bucket](deployments-create-console-s3.md), and then return to step 6\. 
   + **My application is stored in GitHub** — For information, see [Specify Information About a Revision Stored in a GitHub Repository](deployments-create-console-github.md) below, and then return to step 6\.

1. \(Optional\) In the **Deployment description** box, type a description for this deployment\.

1. In the **Deployment configuration** list, choose a deployment configuration to control the rate at which instances are updated with the new application revisions \(in\-place deployments\) or the rate at which traffic is rerouted to a replacement environment \(blue/green deployments\)\. 

   For more information, see [Working with Deployment Configurations in AWS CodeDeploy](deployment-configurations.md)\.

1. In **Content options**, you can specify how AWS CodeDeploy handles files in a deployment target location that weren't part of the previous successful deployment\.

   Choose from the following:
   + **Fail the deployment** — An error is reported and the deployment status is changed to Failed\.
   + **Overwrite the content** — If a file of the same name exists in the target location, the version from the application revision replaces it\.
   + **Retain the content** — If a file of the same name exists in the target location, it is kept and the version in the application revision is not copied to the instance\.

   For more information, see [Rollback Behavior with Existing Content](deployments-rollback-and-redeploy.md#deployments-rollback-and-redeploy-content-options)\. 

1. \(Optional\) In **Rollback configuration overrides**, you can specify different automatic rollback options for this deployment than were specified for the deployment group, if any\.
**Note**  
For information about rollbacks in AWS CodeDeploy, see [Redeployments and Deployment Rollbacks](deployment-steps.md#deployment-rollback) and [Redeploy and Roll Back a Deployment with AWS CodeDeploy](deployments-rollback-and-redeploy.md)\.

   Choose from the following:
   + **Roll back when a deployment fails** — AWS CodeDeploy will redeploy the last known good revision as a new deployment\.
   + **Roll back when alarm thresholds are met** — If alarms were added to the deployment group, AWS CodeDeploy will redeploy the last known good revision when one or more of the specified alarms is activated\.
   + **Disable rollbacks** — Do not perform rollbacks for this deployment\.

1. Choose **Deploy**\. 

   To track the status of your deployment, see [View Deployment Details with AWS CodeDeploy](deployments-view-details.md)\.

**Topics**
+ [Specify Information About a Revision Stored in an Amazon S3 Bucket](deployments-create-console-s3.md)
+ [Specify Information About a Revision Stored in a GitHub Repository](deployments-create-console-github.md)
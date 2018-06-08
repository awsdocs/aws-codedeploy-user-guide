# Create an AWS Lambda Compute Platform Deployment \(Console\)<a name="deployments-create-console-lambda"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. Do one of the following:
   + On the AWS CodeDeploy menu, choose **Deployments**, and then choose **Create deployment**\.
   + On the AWS CodeDeploy menu, choose **Applications**, and then choose the name of the AWS Lambda application you want to deploy a revision to\. You can look in the Compute platform column to identify AWS Lambda applications\. On the **Application details**, page, select the button for the deployment group you want to deploy a revision to\. On the **Actions** menu, choose **Deploy new revision**\.

1. In the **Application** list, choose the name of the application you want to use for this deployment\. After selecting your application, make sure the Compute platform under the **Application** list says AWS Lambda\.

1. In the **Deployment group** list, choose the name of the deployment group associated with the application\.

1. Notice the **Deployment type** label\. AWS Lambda deployments are all blue/green\.

1. Next to **Revision location** choose where your revision is located:
   + **My revision is stored in Amazon S3** — For information, see [Specify Information About a Revision Stored in an Amazon S3 Bucket](deployments-create-console-s3.md), and then return to step 6\. 
   + **I will use the AppSpec editor** — Select either JSON or YAML, then type your AppSpec file into the provided editor\. You may save the AppSpec file you type in by selecting **Save as text file**\. When you click **Deploy** at the end of these steps you will receive an error if your JSON or YAML is not valid\. For more information about creating an AppSpec file, see [Add an Application Specification File to a Revision for AWS CodeDeploy](application-revisions-appspec-file.md)\. 

1. \(Optional\) In the **Deployment description** box, type a description for this deployment\.

1. In the **Deployment configuration** list, choose a deployment configuration to control how traffic is shifted to the Lambda function version\. 

   For more information, see [ Deployment Configurations on an AWS Lambda Compute Platform ](deployment-configurations.md#deployment-configuration-lambda)

1. \(Optional\) In **Rollback configuration overrides**, you can specify different automatic rollback options for this deployment than were specified for the deployment group, if any\.
**Note**  
For information about rollbacks in AWS CodeDeploy, see [Redeployments and Deployment Rollbacks](deployment-steps.md#deployment-rollback-lambda) and [Redeploy and Roll Back a Deployment with AWS CodeDeploy](deployments-rollback-and-redeploy.md)\.

   Choose from the following:
   + **Roll back when a deployment fails** — AWS CodeDeploy will redeploy the last known good revision as a new deployment\.
   + **Roll back when alarm thresholds are met** — If alarms were added to the deployment group, AWS CodeDeploy will redeploy the last known good revision when one or more of the specified alarms is activated\.
   + **Disable rollbacks** — Do not perform rollbacks for this deployment\.

1. Choose **Deploy**\. 

   To track the status of your deployment, see [View Deployment Details with AWS CodeDeploy](deployments-view-details.md)\.
--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Prerequisites<a name="tutorials-auto-scaling-group-prerequisites"></a>

For this tutorial, we assume you have already completed all of the steps in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md), including setting up and configuring the AWS CLI and creating an IAM instance profile \(**CodeDeployDemo\-EC2\-Instance\-Profile**\) and a service role \(**CodeDeployDemo**\)\. A *service role* is a special type of IAM role that gives a service permission to act on your behalf\.

If you want to deploy an application revision to an Amazon EC2 Auto Scaling group of Ubuntu Server Amazon EC2 instances, you can create and use the sample revision in [Step 2: Create a Sample Application Revision](tutorials-on-premises-instance.md#tutorials-on-premises-instance-2-create-sample-revision) in the [Tutorial: Deploy an Application to an On\-Premises Instance with AWS CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md) tutorial\. Otherwise, you need to create and use a revision that is compatible with an Ubuntu Server instance and AWS CodeDeploy\. We also provide sample revisions for Amazon Linux, Windows Server, and Red Hat Enterprise Linux \(RHEL\) Amazon EC2 instances\. To create a revision on your own, see [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\.
# Prerequisites<a name="tutorials-auto-scaling-group-prerequisites"></a>

For this tutorial, we assume you have already completed all of the steps in [Getting Started with CodeDeploy](getting-started-codedeploy.md), including setting up and configuring the AWS CLI and creating an IAM instance profile \(**CodeDeployDemo\-EC2\-Instance\-Profile**\) and a service role \(**CodeDeployDemo**\)\. A *service role* is a special type of IAM role that gives a service permission to act on your behalf\.

**Note**  
 If you create your Auto Scaling group with a launch template, you must add the following permissions:   
 `ec2:RunInstances` 
 `ec2:CreateTags` 
 `iam:PassRole` 
 For more information, see [Create a Service Role](getting-started-create-service-role.md) and [Creating a Launch Template for an Auto Scaling Group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html)\. 

If you want to deploy an application revision to an Amazon EC2 Auto Scaling group of Ubuntu Server Amazon EC2 instances, you can create and use the sample revision in [Step 2: Create a Sample Application Revision](tutorials-on-premises-instance-2-create-sample-revision.md) in the [Tutorial: Deploy an Application to an On\-Premises Instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md) tutorial\. Otherwise, you need to create and use a revision that is compatible with an Ubuntu Server instance and CodeDeploy\. We also provide sample revisions for Amazon Linux, Windows Server, and Red Hat Enterprise Linux \(RHEL\) Amazon EC2 instances\. To create a revision on your own, see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.
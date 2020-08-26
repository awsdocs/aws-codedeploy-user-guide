# Prerequisites<a name="tutorials-auto-scaling-group-prerequisites"></a>

To follow along in this tutorial:
+ Complete all of the steps in [Getting started with CodeDeploy](getting-started-codedeploy.md), including setting up and configuring the AWS CLI and creating an IAM instance profile \(**CodeDeployDemo\-EC2\-Instance\-Profile**\) and a service role \(**CodeDeployDemo**\)\. A *service role* is a special type of IAM role that gives a service permission to act on your behalf\.
+ If you create your Auto Scaling group with a launch template, you must add the following permissions:
  +  `ec2:RunInstances` 
  +  `ec2:CreateTags` 
  +  `iam:PassRole` 

  For more information, see [Step 3: Create a service role](getting-started-create-service-role.md) and [Creating a launch template for an Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html)\. 
+  Create and use a revision that is compatible with an Ubuntu Server instance and CodeDeploy\. For your revision, you can do one of the following:
  + Create and use the sample revision in [Step 2: Create a sample application revision](tutorials-on-premises-instance-2-create-sample-revision.md) in the [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md) tutorial\. 
  + Create a revision on your own, see [Working with application revisions for CodeDeploy](application-revisions.md)\.
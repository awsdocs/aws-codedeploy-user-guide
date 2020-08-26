# General troubleshooting issues<a name="troubleshooting-general"></a>

**Topics**
+ [General troubleshooting checklist](#troubleshooting-checklist)
+ [CodeDeploy deployment resources are supported in only in some AWS Regions](#troubleshooting-supported-regions)
+ [Procedures in this guide do not match the CodeDeploy console](#troubleshooting-old-console)
+ [Required IAM roles are not available](#troubleshooting-iam-cloudformation)
+ [Using some text editors to create AppSpec files and shell scripts can cause deployments to fail](#troubleshooting-text-editors)
+ [Using Finder in macOS to bundle an application revision can cause deployments to fail](#troubleshooting-bundle-with-finder)

## General troubleshooting checklist<a name="troubleshooting-checklist"></a>

You can use the following checklist to troubleshoot a failed deployment\.

1. See [View CodeDeploydeployment details ](deployments-view-details.md) and [View instance details with CodeDeploy](instances-view-details.md) to determine why the deployment failed\. If you cannot determine the cause, review the items in this checklist\.

1. Check that you have correctly configured the instances:
   + Was the instance launched with an Amazon EC2 key pair specified? For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in *Amazon EC2 User Guide for Linux Instances*\.
   + Is the correct IAM instance profile attached to the instance? For more information, see [Configure an Amazon EC2 instance to work with CodeDeploy](instances-ec2-configure.md) and [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md)\.
   + Was the instance tagged? For more information, see [Working with tags in the console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console) in *Amazon EC2 User Guide for Linux Instances*\.
   + Is the CodeDeploy agent installed and running on the instance? For more information, see [Managing CodeDeploy agent operations](codedeploy-agent-operations.md)\.

1. Check the application and deployment group settings:
   + To check your application settings, see [View application details with CodeDeploy](applications-view-details.md)\.
   + To check your deployment group settings, see [View deployment group details with CodeDeploy](deployment-groups-view-details.md)\.

1. Confirm the application revision is correctly configured:
   + Check the format of your AppSpec file\. For more information, see [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md) and [CodeDeploy AppSpec File reference](reference-appspec-file.md)\.
   + Check your Amazon S3 bucket or GitHub repository to verify your application revision is in the expected location\.
   + Review the details of your CodeDeploy application revision to ensure that it is registered correctly\. For information, see [View application revision details with CodeDeploy](application-revisions-view-details.md)\.
   + If you're deploying from Amazon S3, check your Amazon S3 bucket to verify CodeDeploy has been granted permissions to download the application revision\. For information about bucket policies, see [Deployment prerequisites](deployments-create-prerequisites.md)\.
   + If you're deploying from GitHub, check your GitHub repository to verify CodeDeploy has been granted permissions to download the application revision\. For more information, see [Create a deployment with CodeDeploy](deployments-create.md) and [GitHub authentication with applications in CodeDeploy](integrations-partners-github.md#behaviors-authentication)\.

1. Check that the service role is correctly configured\. For information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

1. Confirm you followed the steps in [Getting started with CodeDeploy](getting-started-codedeploy.md) to: 
   + Attach policies to the IAM user\.
   + Install or upgrade and configure the AWS CLI\.
   + Create an IAM instance profile and a service role\.

   For more information, see [Identity and access management for AWS CodeDeploy](security-iam.md)\.

1. Confirm you are using AWS CLI version 1\.6\.1 or later\. To check the version you have installed, call aws \-\-version\.

If you are still unable to troubleshoot your failed deployment, review the other issues in this topic\.

## CodeDeploy deployment resources are supported in only in some AWS Regions<a name="troubleshooting-supported-regions"></a>

If you do not see or cannot access applications, deployment groups, instances, or other deployment resources from the AWS CLI or the CodeDeploy console, make sure you're referencing one of the AWS Regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\.

Amazon EC2 instances and Amazon EC2 Auto Scaling groups that are used in CodeDeploy deployments must be launched and created in one of these AWS Regions\.

If you're using the AWS CLI, run the aws configure command from the AWS CLI\. Then you can view and set your default AWS Region\.

If you're using the CodeDeploy console, on the navigation bar, from the region selector, choose one of the supported AWS Regions\.

**Important**  
To use services in the China \(Beijing\) Region or China \(Ningxia\) Region, you must have an account and credentials for those regions\. Accounts and credentials for other AWS regions do not work for the Beijing and Ningxia Regions, and vice versa\.  
Information about some resources for the China Regions, such as CodeDeploy Resource Kit bucket names and CodeDeploy agent installation procedures, are not included in this edition of the *CodeDeploy User Guide*\.  
For more information:  
[CodeDeploy](http://docs.amazonaws.cn/en_us/aws/latest/userguide/codedeploy.html) in *[Getting Started with AWS in the China \(Beijing\) Region](http://docs.amazonaws.cn/en_us/aws/latest/userguide/introduction.html) *
*CodeDeploy User Guide for the China Regions* \([English version](http://docs.amazonaws.cn/en_us/codedeploy/latest/userguide/welcome.html) \| [Chinese version](http://docs.amazonaws.cn/codedeploy/latest/userguide/welcome.html)\)

## Procedures in this guide do not match the CodeDeploy console<a name="troubleshooting-old-console"></a>

 The procedures in this guide are written to reflect the new console design\. If you are using the older version of the console, many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

## Required IAM roles are not available<a name="troubleshooting-iam-cloudformation"></a>

If you rely on an IAM instance profile or a service role that was created as part of an AWS CloudFormation stack, if you delete the stack, all IAM roles are deleted, too\. This may be why the IAM role is no longer displayed in the IAM console and CodeDeploy no longer works as expected\. To fix this problem, you must manually re\-create the deleted IAM role\.

## Using some text editors to create AppSpec files and shell scripts can cause deployments to fail<a name="troubleshooting-text-editors"></a>

Some text editors introduce non\-conforming, non\-printing characters into files\. If you use text editors to create or modify AppSpec files or shell script files to run on Amazon Linux, Ubuntu Server, or RHEL instances, then any deployments that rely on these files might fail\. When CodeDeploy uses these files during a deployment, the presence of these characters can lead to hard\-to\-troubleshoot AppSpec file validation failures and script execution failures\. 

In the CodeDeploy console, on the event details page for the deployment, choose **View logs**\. \(Or you use the AWS CLI to call the [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\.\) Look for errors like `invalid character`, `command not found`, or `file not found`\.

To address this issue, we recommend the following:
+ Do not use text editors that introduce non\-printing characters such as carriage returns \(`^M` characters\) into your AppSpec files and shell script files\. 
+ Use text editors that display non\-printing characters such as carriage returns in your AppSpec files and shell script files, so you can find and remove any that might be introduced\. For examples of these types of text editors, search the internet for text editors that show carriage returns\.
+ Use text editors running on Amazon Linux, Ubuntu Server, or RHEL instances to create shell script files that run on Amazon Linux, Ubuntu Server, or RHEL instances\. For examples of these types of text editors, search the internet for Linux shell script editors\.
+ If you must use a text editor in Windows or macOS to create shell script files to run on Amazon Linux, Ubuntu Server, or RHEL instances, use a program or utility that converts text in Windows or macOS format to Unix format\. For examples of these programs and utilities, search the internet for DOS to UNIX or Mac to UNIX\. Be sure to test the converted shell script files on the target operating systems\.

## Using Finder in macOS to bundle an application revision can cause deployments to fail<a name="troubleshooting-bundle-with-finder"></a>

Deployments might fail if you use the Finder graphical user interface \(GUI\) application on a Mac to bundle \(zip\) an AppSpec file and related files and scripts into an application revision archive \(\.zip\) file\. This is because Finder creates an intermediate `__MACOSX` folder in the \.zip file and places component files into it\. CodeDeploy cannot find the component files, so the deployment fails\.

To address this issue, we recommend you use the AWS CLI to call the [push](https://docs.aws.amazon.com/cli/latest/reference/deploy/push.html) command, which zips the component files into the expected structure\. Alternatively, you can use Terminal instead of the GUI to zip the component files\. Terminal does not create an intermediate `__MACOSX` folder\.
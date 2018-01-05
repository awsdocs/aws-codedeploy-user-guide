# General Troubleshooting Issues<a name="troubleshooting-general"></a>


+ [General Troubleshooting Checklist](#troubleshooting-checklist)
+ [AWS CodeDeploy deployment resources are supported in certain regions only](#troubleshooting-supported-regions)
+ [Required IAM roles are not available](#troubleshooting-iam-cloudformation)
+ [Avoid concurrent deployments to the same Amazon EC2 instance](#troubleshooting-concurrent-deployments)
+ [Using some text editors to create AppSpec files and shell scripts can cause deployments to fail](#troubleshooting-text-editors)
+ [Using Finder in Mac OS to bundle an application revision can cause deployments to fail](#troubleshooting-bundle-with-finder)

## General Troubleshooting Checklist<a name="troubleshooting-checklist"></a>

You can use the following checklist to troubleshoot a failed deployment\.

1. See [View Deployment Details with AWS CodeDeploy](deployments-view-details.md) and [View Instance Details with AWS CodeDeploy](instances-view-details.md) to determine why the deployment failed\. If you are unable to determine the cause, continue to the rest of the items in this checklist\.

1. Check whether you have correctly configured the instances:

   + Was the instance launched with an Amazon EC2 key pair specified? For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in *Amazon EC2 User Guide for Linux Instances*\.

   + Is the correct IAM instance profile attached to the instance? For more information, see [Configure an Amazon EC2 Instance to Work with AWS CodeDeploy](instances-ec2-configure.md) and [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\.

   + Was the instance tagged? For more information, see [Working with Tags in the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console) in *Amazon EC2 User Guide for Linux Instances*\.

   + Is the AWS CodeDeploy agent installed and running on the instance? For more information, see [Managing AWS CodeDeploy Agent Operations](codedeploy-agent-operations.md)\.

1. Check the application and deployment group settings:

   + To check your application settings, see [View Application Details with AWS CodeDeploy](applications-view-details.md)\.

   + To check your deployment group settings, see [View Deployment Group Details with AWS CodeDeploy](deployment-groups-view-details.md)\.

1. Confirm the application revision is correctly configured:

   + Check the format of your AppSpec file\. For more information, see [Add an Application Specification File to a Revision for AWS CodeDeploy](application-revisions-appspec-file.md) and [AWS CodeDeploy AppSpec File Reference](reference-appspec-file.md)\.

   + Check your Amazon S3 bucket or GitHub repository to verify your application revision is in the expected location\.

   + Review the details of your AWS CodeDeploy application revision to ensure that it is registered correctly\. For information, see [View Application Revision Details with AWS CodeDeploy](application-revisions-view-details.md)\.

   + If you're deploying from Amazon S3, check your Amazon S3 bucket to verify AWS CodeDeploy has been granted permissions to download the application revision\. For information about bucket policies, see [Deployment Prerequisites](deployments-create-prerequisites.md)\.

   + If you're deploying from GitHub, check your GitHub repository to verify AWS CodeDeploy has been granted permissions to download the application revision\. For more information, see [Create a Deployment with AWS CodeDeploy](deployments-create.md) and [GitHub Authentication with Applications in AWS CodeDeploy](integrations-partners-github.md#behaviors-authentication)\.

1. Check whether the service role is correctly configured\. For information, see [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\.

1. Confirm you followed the steps in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md) to: 

   + Attach policies to the IAM user\.

   + Install or upgrade and configure the AWS CLI\.

   + Create an IAM instance profile and a service role\.

   For more information, see [Authentication and Access Control for AWS CodeDeploy](auth-and-access-control.md)\.

1. Confirm you are using AWS CLI version 1\.6\.1 or later\. To check the version you have installed, call aws \-\-version\.

If you are still unable to troubleshoot your failed deployment, review the other issues in this topic\.

## AWS CodeDeploy deployment resources are supported in certain regions only<a name="troubleshooting-supported-regions"></a>

If you do not see or cannot access applications, deployment groups, instances, or other deployment resources from the AWS CLI or the AWS CodeDeploy console, make sure you're referencing one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\.

Amazon EC2 instances and Auto Scaling groups that will be used in AWS CodeDeploy deployments must be launched and created in one of these regions\.

If you're using the AWS CLI, run the `aws configure` command from the AWS CLI\. Then you can view and set your default region\.

If you're using the AWS CodeDeploy console, on the navigation bar, from the region selector, choose one of the supported regions\.

**Important**  
To use services in the China \(Beijing\) Region or China \(Ningxia\) Region, you must have an account and credentials for those regions\. Accounts and credentials for other AWS regions do not work for the Beijing and Ningxia Regions, and vice versa\.  
Information about some resources for the China Regions, such as AWS CodeDeploy Resource Kit bucket names and AWS CodeDeploy agent installation procedures, are not included in this edition of the *AWS CodeDeploy User Guide*\.  
For more information:  
[AWS CodeDeploy](http://docs.amazonaws.cn/en_us/aws/latest/userguide/codedeploy.html) in *[Getting Started with AWS in the China \(Beijing\) Region](http://docs.amazonaws.cn/en_us/aws/latest/userguide/introduction.html) *
*AWS CodeDeploy User Guide for the China Regions* \([English version](http://docs.amazonaws.cn/en_us/codedeploy/latest/userguide/welcome.html) | [Chinese version](http://docs.amazonaws.cn/codedeploy/latest/userguide/welcome.html)\)

## Required IAM roles are not available<a name="troubleshooting-iam-cloudformation"></a>

If you rely on an IAM instance profile or a service role that was created as part of an AWS CloudFormation stack, if you delete the stack, all IAM roles are deleted, too\. This may be why the IAM role is no longer displayed in the IAM console and AWS CodeDeploy no longer works as expected\. To fix this problem, you must manually re\-create the deleted IAM role\.

## Avoid concurrent deployments to the same Amazon EC2 instance<a name="troubleshooting-concurrent-deployments"></a>

As a best practice, you should avoid situations that would result in more than one attempted deployment to an Amazon EC2 instance at the same time\. In cases where commands from different deployments compete to run on a single instance, the deployments can time out and fail for the following reasons:

+ AWS CodeDeploy's timeout logic expects all of the steps in a deployment process to be completed in five minutes or less\. 

+ The AWS CodeDeploy agent can process only one deployment command at a time\. 

+ It's not possible to control the order in which deployments occur if more than one deployment attempts to run at the same time\. 

AWS CodeDeploy logic considers a deployment to have failed if its steps are not complete within five minutes, even if a deployment process is otherwise running as expected\. The five\-minute limit can be exceeded if commands from multiple deployments are being sent to the AWS CodeDeploy agent at the same time\.

For information about other challenges you might face with concurrent deployments in Auto Scaling groups, see [Avoid associating multiple deployment groups with a single Auto Scaling group](troubleshooting-auto-scaling.md#troubleshooting-multiple-depgroups)\.

## Using some text editors to create AppSpec files and shell scripts can cause deployments to fail<a name="troubleshooting-text-editors"></a>

Some text editors introduce non\-conforming, non\-printing characters into files\. If you use text editors to create or modify AppSpec files or shell script files to run on Amazon Linux, Ubuntu Server, or RHEL instances, then any deployments that rely on these files might fail\. When AWS CodeDeploy uses these files during a deployment, the presence of these characters can lead to hard\-to\-troubleshoot AppSpec file validation failures and script execution failures\. 

In the AWS CodeDeploy console, on the event details page for the deployment, choose **View logs**\. \(Alternatively, you use the AWS CLI to call the [get\-deployment\-instance](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\.\) Look for errors like "invalid character," "command not found," or "file not found\."

To address this issue, we recommend the following:

+ Do not use text editors that automatically introduce non\-printing characters such as carriage returns \(`^M` characters\) into your AppSpec files and shell script files\. 

+ Use text editors that display non\-printing characters such as carriage returns in your AppSpec files and shell script files, so you can find and remove any that may be automatically or randomly introduced\. For examples of these types of text editors, search the Internet for "text editor show carriage returns\."

+ Use text editors running on Amazon Linux, Ubuntu Server, or RHEL instances to create shell script files that run on Amazon Linux, Ubuntu Server, or RHEL instances\. For examples of these types of text editors, search the Internet for "Linux shell script editor\."

+ If you must use a text editor in Windows or Mac OS to create shell script files to run on Amazon Linux, Ubuntu Server, or RHEL instances, use a program or utility that converts text in Windows or Mac OS format to Unix format\. For examples of these programs and utilities, search the Internet for "DOS to UNIX" or "Mac to UNIX\." Be sure to test the converted shell script files on the target operating systems\.

## Using Finder in Mac OS to bundle an application revision can cause deployments to fail<a name="troubleshooting-bundle-with-finder"></a>

Deployments might fail if you use the Finder graphical user interface \(GUI\) application on a Mac to bundle \(zip\) an AppSpec file and related files and scripts into an application revision archive \(\.zip\) file\. This is because Finder creates an intermediate `__MACOSX` folder in the \.zip file and places component files into it\. AWS CodeDeploy cannot find the component files, so the deployment fails\.

To address this issue, we recommend you use the AWS CLI to call the [push](http://docs.aws.amazon.com/cli/latest/reference/deploy/push.html) command, which zips the component files into the expected structure\. Alternatively, you can use Terminal instead of the GUI to zip the component files\. Terminal does not create an intermediate `__MACOSX` folder\.
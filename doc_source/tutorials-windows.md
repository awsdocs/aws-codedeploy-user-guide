[Back to contents](index.md)

# Tutorial: Deploy a "Hello, World\!" Application with CodeDeploy \(Windows Server\)<a name="tutorials-windows"></a>

In this tutorial, you deploy a single webpage to a single Windows Server Amazon EC2 instance running Internet Information Services \(IIS\) as its web server\. This webpage displays a simple "Hello, World\!" message\.

Not what you're looking for?
+ To practice deploying to an Amazon Linux or Red Hat Enterprise Linux \(RHEL\) Amazon EC2 instance instead, see [Tutorial: Deploy WordPress to an Amazon EC2 Instance \(Amazon Linux or Red Hat Enterprise Linux and Linux, macOS, or Unix\)](tutorials-wordpress.md)\.
+ To practice deploying to an on\-premises instance instead, see [Tutorial: Deploy an Application to an On\-Premises Instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

This tutorial's steps are presented from a Windows perspective\. Although you can complete most of these steps on a local machine running Linux, macOS, or Unix, you must adapt those that cover Windows\-based directory paths such as `c:\temp`\. Also, if you want to connect to the Amazon EC2 instance, you need a client application that can connect through Remote Desktop Protocol \(RDP\) to the Amazon EC2 instance running Windows Server\. \(Windows includes an RDP connection client application by default\.\)

Before you start this tutorial, you must complete the prerequisites in [Getting Started with CodeDeploy](getting-started-codedeploy.md), including configuring your IAM user, installing or upgrading the AWS CLI, and creating an IAM instance profile and a service role\.

**Topics**
+ [Step 1: Launch a Windows Server Amazon EC2 Instance](tutorials-windows-launch-instance.md)
+ [Step 2: Configure Your Source Content to Deploy to the Windows Server Amazon EC2 Instance](tutorials-windows-configure-content.md)
+ [Step 3: Upload Your "Hello, World\!" Application to Amazon S3](tutorials-windows-upload-application.md)
+ [Step 4: Deploy Your Hello World Application](tutorials-windows-deploy-application.md)
+ [Step 5: Update and Redeploy Your "Hello, World\!" Application](tutorials-windows-update-and-redeploy-application.md)
+ [Step 6: Clean Up Your "Hello, World\!" Application and Related Resources](tutorials-windows-clean-up.md)
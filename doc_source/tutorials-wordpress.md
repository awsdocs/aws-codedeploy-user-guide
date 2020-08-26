# Tutorial: Deploy WordPress to an Amazon EC2 instance \(Amazon Linux or Red Hat Enterprise Linux and Linux, macOS, or Unix\)<a name="tutorials-wordpress"></a>

In this tutorial, you deploy WordPress, an open source blogging tool and content management system based on PHP and MySQL, to a single Amazon EC2 instance running Amazon Linux or Red Hat Enterprise Linux \(RHEL\)\.

Not what you're looking for?
+ To practice deploying to an Amazon EC2 instance running Windows Server instead, see [Tutorial: Deploy a "hello, world\!" application with CodeDeploy \(Windows Server\)](tutorials-windows.md)\.
+ To practice deploying to an on\-premises instance instead of an Amazon EC2 instance, see [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

This tutorial's steps are presented from the perspective of a local development machine running Linux, macOS, or Unix\. Although you can complete most of these steps on a local machine running Windows, you must adapt the steps that cover commands such as chmod and wget, applications such as sed, and directory paths such as `/tmp`\.

Before you start this tutorial, you must complete the prerequisites in [Getting started with CodeDeploy](getting-started-codedeploy.md)\. These include configuring your IAM user account, installing or upgrading the AWS CLI, and creating an IAM instance profile and a service role\.

**Topics**
+ [Step 1: Launch and configure an Amazon Linux or Red Hat Enterprise Linux Amazon EC2 instance](tutorials-wordpress-launch-instance.md)
+ [Step 2: Configure your source content to be deployed to the Amazon Linux or Red Hat Enterprise Linux Amazon EC2 instance](tutorials-wordpress-configure-content.md)
+ [Step 3: Upload your WordPress application to Amazon S3](tutorials-wordpress-upload-application.md)
+ [Step 4: Deploy your WordPress application](tutorials-wordpress-deploy-application.md)
+ [Step 5: Update and redeploy your WordPress application](tutorials-wordpress-update-and-redeploy-application.md)
+ [Step 6: Clean up your WordPress application and related resources](tutorials-wordpress-clean-up.md)
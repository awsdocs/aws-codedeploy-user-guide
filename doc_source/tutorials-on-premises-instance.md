# Tutorial: Deploy an Application to an On\-Premises Instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)<a name="tutorials-on-premises-instance"></a>

This tutorial helps you gain experience with CodeDeploy by guiding you through the deployment of a sample application revision to a single on\-premises instance—that is, a physical device that is not an Amazon EC2 instance—running Windows Server, Ubuntu Server, or Red Hat Enterprise Linux \(RHEL\)\. For information about on\-premises instances and how they work with CodeDeploy, see [Working with On\-Premises Instances for CodeDeploy](instances-on-premises.md)\.

Not what you're looking for?
+ To practice deploying to an Amazon EC2 instance running Amazon Linux or RHEL, see [Tutorial: Deploy WordPress to an Amazon EC2 Instance \(Amazon Linux or Red Hat Enterprise Linux and Linux, macOS, or Unix\)](tutorials-wordpress.md)\.
+ To practice deploying to an Amazon EC2 instance running Windows Server, see [Tutorial: Deploy a "Hello, World\!" Application with CodeDeploy \(Windows Server\)](tutorials-windows.md)\.

**Topics**
+ [Prerequisites](#tutorials-on-premises-instance-prerequisites)
+ [Step 1: Configure the On\-Premises Instance](#tutorials-on-premises-instance-1-configure-instance)
+ [Step 2: Create a Sample Application Revision](#tutorials-on-premises-instance-2-create-sample-revision)
+ [Step 3: Bundle and Upload Your Application Revision to Amazon S3](#tutorials-on-premises-instance-3-bundle-sample-revision)
+ [Step 4: Deploy Your Application Revision](#tutorials-on-premises-instance-4-deploy-sample-revision)
+ [Step 5: Verify Your Deployment](#tutorials-on-premises-instance-5-verify-deployment)
+ [Step 6: Clean Up Resources](#tutorials-on-premises-instance-6-clean-up-resources)

## Prerequisites<a name="tutorials-on-premises-instance-prerequisites"></a>

Before you start this tutorial, you must complete the prerequisites in [Getting Started with CodeDeploy](getting-started-codedeploy.md), which include configuring your IAM user, installing or upgrading the AWS CLI, and creating a service role\. You do not have to create an IAM instance profile as described in the prerequisites\. On\-premises instances do not use IAM instance profiles\.

The physical device you will configure as an on\-premises instance must be running one of the operating systems listed in [Operating Systems Supported by the CodeDeploy Agent](codedeploy-agent.md#codedeploy-agent-supported-operating-systems)\.

## Step 1: Configure the On\-Premises Instance<a name="tutorials-on-premises-instance-1-configure-instance"></a>

Before you can deploy to your on\-premises instance, you must configure it\. Follow the instructions in [Working with On\-Premises Instances for CodeDeploy](instances-on-premises.md), and then return to this page\.

## Step 2: Create a Sample Application Revision<a name="tutorials-on-premises-instance-2-create-sample-revision"></a>

In this step, you'll create a sample application revision to deploy to your on\-premises instance\. 

Because it is difficult to know which software and features are already installed—or are allowed to be installed by your organization's policies—on your on\-premises instance, the sample application revision we offer here simply uses batch scripts \(for Windows Server\) or shell scripts \(for Ubuntu Server and RHEL\) to write text files to a location on your on\-premises instance\. One file is written for each of several CodeDeploy deployment lifecycle events, including **Install**, **AfterInstall**, **ApplicationStart**, and **ValidateService**\. During the **BeforeInstall** deployment lifecycle event, a script will run to remove old files written during previous deployments of this sample and create a location on your on\-premises instance to which to write the new files\. 

**Note**  
This sample application revision may fail to be deployed if any of the following are true:  
The user account that starts the CodeDeploy agent on the on\-premises instance does not have permission to execute scripts\.
The user account does not have permission to create or delete folders in the locations listed in the scripts\.
The user account does not have permission to create text files in the locations listed in the scripts\.

**Note**  
If you configured a Windows Server instance and want to deploy a different sample, you may want to use the one in [Step 2: Configure Your Source Content to Deploy to the Windows Server Amazon EC2 Instance](tutorials-windows-configure-content.md) in the [Tutorial: Deploy a "Hello, World\!" Application with CodeDeploy \(Windows Server\)](tutorials-windows.md) tutorial\.  
If you configured a RHEL instance and want to deploy a different sample, you may want to use the one in [Step 2: Configure Your Source Content to Be Deployed to the Amazon Linux or Red Hat Enterprise Linux Amazon EC2 Instance](tutorials-wordpress-configure-content.md) in the [Tutorial: Deploy WordPress to an Amazon EC2 Instance \(Amazon Linux or Red Hat Enterprise Linux and Linux, macOS, or Unix\)](tutorials-wordpress.md) tutorial\.  
Currently, there is no alternative sample for Ubuntu Server\.

1. On your development machine, create a subdirectory \(subfolder\) named `CodeDeployDemo-OnPrem` that will store the sample application revision's files, and then switch to the subfolder\. For this example, we assume you'll use the `c:\temp` folder as the root folder for Windows Server or the `/tmp` folder as the root folder for Ubuntu Server and RHEL\. If you use a different folder, be sure to substitute it for ours throughout this tutorial: 

   For Windows:

   ```
   mkdir c:\temp\CodeDeployDemo-OnPrem
   cd c:\temp\CodeDeployDemo-OnPrem
   ```

   For Linux, macOS, or Unix:

   ```
   mkdir /tmp/CodeDeployDemo-OnPrem
   cd /tmp/CodeDeployDemo-OnPrem
   ```

1. In the root of the `CodeDeployDemo-OnPrem` subfolder, use a text editor to create two files named `appspec.yml` and `install.txt`:

   `appspec.yml` for Windows Server:

   ```
   version: 0.0
   os: windows
   files:
     - source: .\install.txt
       destination: c:\temp\CodeDeployExample
   hooks:
     BeforeInstall:
       - location: .\scripts\before-install.bat
         timeout: 900
     AfterInstall:
       - location: .\scripts\after-install.bat     
         timeout: 900
     ApplicationStart:
       - location: .\scripts\application-start.bat  
         timeout: 900
     ValidateService:
       - location: .\scripts\validate-service.bat    
         timeout: 900
   ```

   `appspec.yml` for Ubuntu Server and RHEL:

   ```
   version: 0.0
   os: linux
   files:
     - source: ./install.txt
       destination: /tmp/CodeDeployExample
   hooks:
     BeforeInstall:
       - location: ./scripts/before-install.sh
         timeout: 900
     AfterInstall:
       - location: ./scripts/after-install.sh
         timeout: 900
     ApplicationStart:
       - location: ./scripts/application-start.sh
         timeout: 900
     ValidateService:
       - location: ./scripts/validate-service.sh
         timeout: 900
   ```

   For more information about AppSpec files, see [Add an Application Specification File to a Revision for CodeDeploy](application-revisions-appspec-file.md) and [CodeDeploy AppSpec File Reference](reference-appspec-file.md)\.

   `install.txt`:

   ```
   The Install deployment lifecycle event successfully completed.
   ```

1. Under the root of the `CodeDeployDemo-OnPrem` subfolder, create a `scripts` subfolder, and then switch to it:

   For Windows:

   ```
   mkdir c:\temp\CodeDeployDemo-OnPrem\scripts
   cd c:\temp\CodeDeployDemo-OnPrem\scripts
   ```

   For Linux, macOS, or Unix:

   ```
   mkdir -p /tmp/CodeDeployDemo-OnPrem/scripts
   cd /tmp/CodeDeployDemo-OnPrem/scripts
   ```

1. In the root of the `scripts` subfolder, use a text editor to create four files named `before-install.bat`, `after-install.bat`, `application-start.bat`, and `validate-service.bat` for Windows Server, or `before-install.sh`, `after-install.sh`, `application-start.sh`, and `validate-service.sh` for Ubuntu Server and RHEL:

   For Windows Server:

   `before-install.bat`:

   ```
   set FOLDER=%HOMEDRIVE%\temp\CodeDeployExample
   
   if exist %FOLDER% (
     rd /s /q "%FOLDER%"
   )
   
   mkdir %FOLDER%
   ```

   `after-install.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The AfterInstall deployment lifecycle event successfully completed. > after-install.txt
   ```

   `application-start.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The ApplicationStart deployment lifecycle event successfully completed. > application-start.txt
   ```

   `validate-service.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The ValidateService deployment lifecycle event successfully completed. > validate-service.txt
   ```

   For Ubuntu Server and RHEL:

   `before-install.sh`:

   ```
   #!/bin/bash
   export FOLDER=/tmp/CodeDeployExample
   
   if [ -d $FOLDER ]
   then
    rm -rf $FOLDER
   fi
   
   mkdir -p $FOLDER
   ```

   `after-install.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The AfterInstall deployment lifecycle event successfully completed." > after-install.txt
   ```

   `application-start.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The ApplicationStart deployment lifecycle event successfully completed." > application-start.txt
   ```

   `validate-service.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The ValidateService deployment lifecycle event successfully completed." > validate-service.txt
   
   unset FOLDER
   ```

1. For Ubuntu Server and RHEL only, make sure the four shell scripts have execute permissions:

   ```
   chmod +x ./scripts/*
   ```

## Step 3: Bundle and Upload Your Application Revision to Amazon S3<a name="tutorials-on-premises-instance-3-bundle-sample-revision"></a>

Before you can deploy your application revision, you'll need to bundle the files, and then upload the file bundle to an Amazon S3 bucket\. Follow the instructions in [Create an Application with CodeDeploy](applications-create.md) and [Push a Revision for CodeDeploy to Amazon S3 \(EC2/On\-Premises Deployments Only\)](application-revisions-push.md)\. \(Although you can give the application and deployment group any name, we recommend you use `CodeDeploy-OnPrem-App` for the application name and `CodeDeploy-OnPrem-DG` for the deployment group name\.\) After you have completed those instructions, return to this page\. 

**Note**  
Alternatively, you can upload the file bundle to a GitHub repository and deploy it from there\. For more information, see [Integrating CodeDeploy with GitHub](integrations-partners-github.md)\.

## Step 4: Deploy Your Application Revision<a name="tutorials-on-premises-instance-4-deploy-sample-revision"></a>

After you've uploaded your application revision to an Amazon S3 bucket, try deploying it to your on\-premises instance\. Follow the instructions in [Create a Deployment with CodeDeploy](deployments-create.md), and then return to this page\.

## Step 5: Verify Your Deployment<a name="tutorials-on-premises-instance-5-verify-deployment"></a>

To verify the deployment was successful, follow the instructions in [View CodeDeployDeployment Details ](deployments-view-details.md), and then return to this page\.

If the deployment was successful, you'll find four text files in the `c:\temp\CodeDeployExample` folder \(for Windows Server\) or `/tmp/CodeDeployExample` \(for Ubuntu Server and RHEL\)\. 

If the deployment failed, follow the troubleshooting steps in [View Instance Details with CodeDeploy](instances-view-details.md) and [Troubleshoot instance issues](troubleshooting-ec2-instances.md)\. Make any required fixes, rebundle and upload your application revision, and then try the deployment again\.

## Step 6: Clean Up Resources<a name="tutorials-on-premises-instance-6-clean-up-resources"></a>

To avoid ongoing charges for resources you created for this tutorial, delete the Amazon S3 bucket if you'll no longer be using it\. You can also clean up associated resources, such as the application and deployment group records in CodeDeploy and the on\-premises instance\.

You can use the AWS CLI or a combination of the CodeDeploy and Amazon S3 consoles and the AWS CLI to clean up resources\. 

### Clean Up Resources \(CLI\)<a name="tutorials-on-premises-instance-6-clean-up-resources-cli"></a>

**To delete the Amazon S3 bucket**
+ Call the [rm](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) command along with the `--recursive` switch against the bucket \(for example, `codedeploydemobucket`\)\. The bucket and all objects in the bucket will be deleted\. 

  ```
  aws s3 rm s3://your-bucket-name --recursive
  ```

**To delete the application and deployment group records in CodeDeploy**
+ Call the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command against the application \(for example, `CodeDeploy-OnPrem-App`\)\. The records for the deployment and deployment group will be deleted\. 

  ```
  aws deploy delete-application --application-name your-application-name
  ```<a name="tutorials-on-premises-instance-6-clean-up-resources-deregister-cli"></a>

**To deregister the on\-premises instance and delete the IAM user**
+ Call the [deregister](https://docs.aws.amazon.com/cli/latest/reference/deploy/deregister.html) command against the on\-premises instance and region:

  ```
  aws deploy deregister --instance-name your-instance-name --delete-iam-user --region your-region
  ```
**Note**  
If you do not want to delete the IAM user associated with this on\-premises instance, use the `--no-delete-iam-user` option instead\.

**To uninstall the CodeDeploy agent and remove the configuration file from the on\-premises instance**
+ From the on\-premises instance, call the [uninstall](https://docs.aws.amazon.com/cli/latest/reference/deploy/uninstall.html) command:

  ```
  aws deploy uninstall
  ```

You have now completed all of the steps to clean up the resources used for this tutorial\.

### Clean Up Resources \(Console\)<a name="tutorials-on-premises-instance-6-clean-up-resources-console"></a>

**To delete the Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the icon next to the bucket you want to delete \(for example, `codedeploydemobucket`\), but do not choose the bucket itself\.

1. Choose **Actions**, and then choose **Delete**\. 

1. When prompted to delete the bucket, choose **OK**\. 

**To delete the application and deployment group records in CodeDeploy**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting Started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane choose **Applications**\.

1. Choose the name of the application you want to delete \(for example, `CodeDeploy-OnPrem-App`\) and then choose **Delete application**\.

1. When prompted, enter the name of the application to confirm you want to delete it, and then choose **Delete**\. 

You cannot use the AWS CodeDeploy console to deregister the on\-premises instance or uninstall the CodeDeploy agent\. Follow the instructions in [To deregister the on\-premises instance and delete the IAM user ](#tutorials-on-premises-instance-6-clean-up-resources-deregister-cli)\.
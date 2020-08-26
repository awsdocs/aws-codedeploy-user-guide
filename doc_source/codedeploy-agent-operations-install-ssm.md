# Install the CodeDeploy agent using AWS Systems Manager<a name="codedeploy-agent-operations-install-ssm"></a>

You can use the AWS Management Console or the AWS CLI to install the CodeDeploy agent to your Amazon EC2 or on\-premises instances by using AWS Systems Manager\. You can choose to install a specific version or choose to always install the latest version of the agent\. For more information about AWS Systems Manager, see [What is AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)\. 

 Using AWS Systems Manager is the recommended method for installing and updating the CodeDeploy agent\. You can also install the CodeDeploy agent from an Amazon S3 bucket\. For information about using an Amazon S3 download link, see [Install the CodeDeploy agent using the command line](codedeploy-agent-operations-install-cli.md)\. 

**Topics**
+ [Prerequisites](#install-codedeploy-agent-prereqs)
+ [Install the CodeDeploy agent](#download-codedeploy-agent-on-EC2-Instance)

## Prerequisites<a name="install-codedeploy-agent-prereqs"></a>

Follow the steps in [Getting started with CodeDeploy](getting-started-codedeploy.md) to set up IAM permissions and the AWS CLI\.

If installing the CodeDeploy agent on an on\-premises server with Systems Manager, you must register your on\-premises server with Amazon EC2 Systems Manager\. For more information, see [ Setting up Systems Manager in hybrid environments](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html) in the *AWS Systems Manager User Guide*\.

## Install the CodeDeploy agent<a name="download-codedeploy-agent-on-EC2-Instance"></a>

Before you can use Systems Manager to install the CodeDeploy agent, you must make sure that the instance is configured correctly for Systems Manager\.

### Installing or updating the SSM agent<a name="update-SSM-Agent-EC2instance"></a>

On an Amazon EC2 instance, the CodeDeploy agent requires that the instance is running version 2\.3\.274\.0 or later\. Before you install the CodeDeploy agent, update or install SSM agent on the instance if you haven't already done so\. 

The following Amazon EC2 AMIs come with the SSM agent pre\-installed:
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux and Amazon Linux 2
+ Ubuntu Server 16\.04 and 18\.04
+ Amazon ECS\-Optimized

For information about installing or updating SSM agent on an instance running Linux, see [Installing and configuring the SSM agent on Linux instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html) in the *AWS Systems Manager User Guide*\.

For information about installing or updating SSM agent on an instance running Windows Server, see [ Installing and configuring SSM agent on Windows instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-win.html) in the *AWS Systems Manager User Guide*\.

### \(Optional\) Verify Systems Manager prerequisites<a name="install-codedeploy-agent-minimum-requirements"></a>

Before you use Systems Manager Run Command to install the CodeDeploy agent, verify that your instances meet the minimum Systems Manager requirements\. For more information, see [Setting up AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) in the *AWS Systems Manager User Guide*\.

### Install the CodeDeploy agent<a name="install-codedeploy-agent-EC2"></a>

With SSM, you can install the CodeDeploy once or set up a schedule to install new versions\.

 To install the CodeDeploy agent, choose the `AWSCodeDeployAgent` package while you follow the steps in [Install or update packages with AWS Systems Manager distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor-working-with-packages-deploy.html)\. 
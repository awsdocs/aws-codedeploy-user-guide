# Install or Reinstall the CodeDeploy Agent<a name="codedeploy-agent-operations-install"></a>

If you suspect the CodeDeploy agent is missing or not working, you can run commands on an instance to install or reinstall it\.

**Important**  
An IAM instance profile with permission to access the Amazon S3 bucket that contains the agent installation files for your region must be attached to each Amazon EC2 instance\. Without the permissions provided by the IAM instance profile, the instance will not be able to download CodeDeploy agent installation files\. For information, see [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md) and [Configure an Amazon EC2 Instance to Work with CodeDeploy](instances-ec2-configure.md)\. 

**Topics**
+ [Install or reinstall the CodeDeploy agent for Amazon Linux or RHEL](codedeploy-agent-operations-install-linux.md)
+ [Install or reinstall the CodeDeploy agent for Ubuntu Server](codedeploy-agent-operations-install-ubuntu.md)
+ [Install or reinstall the CodeDeploy agent for Windows Server](codedeploy-agent-operations-install-windows.md)
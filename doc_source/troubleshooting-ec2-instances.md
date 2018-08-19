# Troubleshoot Instance Issues<a name="troubleshooting-ec2-instances"></a>

**Topics**
+ [Tags must be set correctly](#troubleshooting-ec2-tags)
+ [AWS CodeDeploy agent must be installed and running on instances](#troubleshooting-sds-agent)
+ [Deployments do not fail for up to an hour when an instance is terminated during a deployment](#troubleshooting-one-hour-timeout)
+ [Analyzing log files to investigate deployment failures on instances](#troubleshooting-deploy-failures)
+ [Create a new AWS CodeDeploy log file if it was accidentally deleted](#troubleshooting-create-new-log-file)
+ [Troubleshooting “InvalidSignatureException – Signature expired: \[time\] is now earlier than \[time\]” deployment errors](#troubleshooting-instance-time-failures)

## Tags must be set correctly<a name="troubleshooting-ec2-tags"></a>

Use the [list\-deployment\-instances](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-instances.html) command to confirm the instances used for a deployment are tagged correctly\. If an Amazon EC2 instance is missing in the output, use the Amazon EC2 console to confirm the tags have been set on the instance\. For more information, see [Working with Tags in the Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
If you tag an instance and immediately use AWS CodeDeploy to deploy an application to it, the instance might not be included in the deployment\. This is because it can take several minutes before AWS CodeDeploy can read the tags \. We recommend that you wait at least five minutes between the time you tag an instance and attempt to deploy to it\.

## AWS CodeDeploy agent must be installed and running on instances<a name="troubleshooting-sds-agent"></a>

To verify the AWS CodeDeploy agent is installed and running on an instance, see [Verify the AWS CodeDeploy Agent Is Running](codedeploy-agent-operations-verify.md)\.

To install, uninstall, or reinstall the AWS CodeDeploy agent, see [Install or Reinstall the AWS CodeDeploy Agent](codedeploy-agent-operations-install.md)\.

## Deployments do not fail for up to an hour when an instance is terminated during a deployment<a name="troubleshooting-one-hour-timeout"></a>

AWS CodeDeploy provides a one\-hour window for each deployment lifecycle event to run to completion\. This provides ample time for long\-running scripts\. 

If anything occurs that prevents scripts from running to completion while a lifecycle event is in progress \(for example, if an instance is terminated or the AWS CodeDeploy agent is shut down\), it might take up to an hour for the status of the deployment to be displayed as **Failed**\. This is true even if the timeout period specified in the script is shorter than an hour\. This is because when the instance is terminated, the AWS CodeDeploy agent will shut down and will be unable to process any additional scripts\. 

If an instance is terminated between lifecycle events or before the first lifecycle event step starts, however, the timeout occurs after just five minutes\. 

## Analyzing log files to investigate deployment failures on instances<a name="troubleshooting-deploy-failures"></a>

If the status of an instance in the deployment is anything other than `Succeeded`, you can review the deployment log file data to help identify the problem\. For information about accessing deployment log data, see [View Log Data for AWS CodeDeploy Deployments](deployments-view-logs.md)\.

## Create a new AWS CodeDeploy log file if it was accidentally deleted<a name="troubleshooting-create-new-log-file"></a>

If you accidentally delete the deployment log file on an instance, AWS CodeDeploy does not create a replacement log file\. To create a new log file, sign in to the instance, and then run these commands:

**For an Amazon Linux, Ubuntu Server, or RHEL instance**, run these commands in this order, one at a time:

```
sudo service codedeploy-agent stop
```

```
sudo service codedeploy-agent start
```

**For a Windows Server instance**:

```
powershell.exe -Command Restart-Service -Name codedeployagent
```

## Troubleshooting “InvalidSignatureException – Signature expired: \[time\] is now earlier than \[time\]” deployment errors<a name="troubleshooting-instance-time-failures"></a>

AWS CodeDeploy requires accurate time references in order to perform its operations\. If your instance's date and time are not set correctly, they may not match the signature date of your deployment request, which AWS CodeDeploy will therefore reject\.

To avoid deployment failures related to incorrect time settings, see the following topics: 
+  [Setting the Time for Your Linux Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html)
+  [Setting the Time for a Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/windows-set-time.html)

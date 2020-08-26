# Troubleshoot EC2/On\-Premises Deployment Issues<a name="troubleshooting-deployments"></a>

**Topics**
+ [Deployment fails with the message “Validation of PKCS7 signed message failed”](#troubleshooting-deployments-agent-SHA-256)
+ [Deployment or redeployment of the same files to the same instance locations fail with the error "The deployment failed because a specified file already exists at this location"](#troubleshooting-same-files-different-app-name)
+ [Troubleshooting a failed AllowTraffic lifecycle event with no error reported in the deployment logs](#troubleshooting-deployments-allowtraffic-no-logs)
+ [Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events](#troubleshooting-deployments-lifecycle-event-failures)
+ [Troubleshooting a failed DownloadBundle deployment lifecycle event with UnknownError: not opened for reading](#troubleshooting-deployments-downloadbundle)
+ [Windows PowerShell scripts fail to use the 64\-bit version of Windows PowerShell by default](#troubleshooting-deployments-powershell)
+ [Long\-running processes can cause deployments to fail](#troubleshooting-long-running-processes)
+ [Troubleshooting all lifecycle events skipped errors](#troubleshooting-skipped-lifecycle-events)
+ [CodeDeploy plugin CommandPoller missing credentials error](#troubleshooting-agent-commandpoller-error)

**Note**  
The causes of many deployment failures can be identified by reviewing the log files created during the deployment process\. For simplicity, we recommend using Amazon CloudWatch Logs to centrally monitor log files instead of viewing them instance by instance\. For information, see [View CodeDeploy Logs in CloudWatch Logs Console](http://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)\.

## Deployment fails with the message “Validation of PKCS7 signed message failed”<a name="troubleshooting-deployments-agent-SHA-256"></a>

This error message indicates the instance is running a version of the CodeDeploy agent that supports only the SHA\-1 hash algorithm\. Support for the SHA\-2 hash algorithm was introduced in version 1\.0\.1\.854 of the CodeDeploy agent, released in November 2015\. Effective October 17, 2016, deployments fail if a version of the CodeDeploy agent earlier than 1\.0\.1\.854 is installed\. For more information, see [AWS to switch to SHA256 hash algorithm for SSL Certificates](https://aws.amazon.com/security/security-bulletins/aws-to-switch-to-sha256-hash-algorithm-for-ssl-certificates/), [NOTICE: Retiring CodeDeploy host agents older than version 1\.0\.1\.85](https://forums.aws.amazon.com/thread.jspa?threadID=223319), and [Update the CodeDeploy agent](codedeploy-agent-operations-update.md)\.

## Deployment or redeployment of the same files to the same instance locations fail with the error "The deployment failed because a specified file already exists at this location"<a name="troubleshooting-same-files-different-app-name"></a>

When CodeDeploy tries to deploy a file to an instance but a file with the same name already exists in the specified target location, the deployment to that instance may fail\. You may receive the error message "The deployment failed because a specified file already exists at this location: *location\-name*\." This is because, during each deployment, CodeDeploy first deletes all files from the previous deployment, which are listed in a cleanup log file\. If there are files in the target installation folders that aren’t listed in this cleanup file, the CodeDeploy agent by default interprets this as an error and fails the deployment\.

**Note**  
On Amazon Linux, RHEL, and Ubuntu Server instances, the cleanup file is located in `/opt/codedeploy-agent/deployment-root/deployment-instructions/`\. On Windows Server instances, the location is `C:\ProgramData\Amazon\CodeDeploy\deployment-instructions\`\.

The easiest way to avoid this error is to specify an option other than the default behavior to fail the deployment\. For each deployment, you can choose whether to fail the deployment, to overwrite the files not listed in the cleanup file, or to retain the files already on the instance\.

The overwrite option is useful when, for example, you manually placed a file on an instance after the last deployment, but then added a file of the same name to the next application revision\.

You might choose the retain option for files you place on the instance that you want to be part of the next deployment without having to add them to the application revision package\. The retain option is also useful if your application files are already in your production environment and you want to deploy using CodeDeploy for the first time\. For more information, see [Create an EC2/On\-Premises Compute Platform deployment \(console\)](deployments-create-console.md) and [Rollback behavior with existing content](deployments-rollback-and-redeploy.md#deployments-rollback-and-redeploy-content-options)\.

### Troubleshooting `The deployment failed because a specified file already exists at this location` deployment errors<a name="troubleshooting-same-files-different-app-name-failed-deployment"></a>

If you choose not to specify an option to overwrite or retain content that CodeDeploy detects in your target deployment locations \(or if you do not specify any deployment option for handling existing content in a programmatic command\), you can choose to troubleshoot the error\.

The following information applies only if you choose not to retain or overwrite the content\.

If you try to redeploy files with the same names and locations, the redeployment is more likely to succeed if you specify the application name and the deployment group with the same underlying deployment group ID you used before\. CodeDeploy uses the underlying deployment group ID to identify files to remove before a redeployment\. 

Deploying new files or redeploying the same files to the same locations on instances can fail for these reasons:
+ You specified a different application name for a redeployment of the same revision to the same instances\. The redeployment fails because even if the deployment group name is the same, the use of a different application name means a different underlying deployment group ID is being used\.
+ You deleted and re\-created a deployment group for an application and then tried to redeploy the same revision to the deployment group\. The redeployment fails because even if the deployment group name is the same, CodeDeploy references a different underlying deployment group ID\.
+ You deleted an application and deployment group in CodeDeploy, and then created a new application and deployment group with the same names as the ones you deleted\. After that, you tried to redeploy a revision that had been deployed to the previous deployment group to the new one with the same name\. The redeployment fails because even though the application and deployment group names are the same, CodeDeploy still references the ID of the deployment group you deleted\.
+ You deployed a revision to a deployment group and then deployed the same revision to another deployment group to the same instances\. The second deployment fails because CodeDeploy references a different underlying deployment group ID\.
+ You deployed a revision to one deployment group and then deployed another revision to another deployment group to the same instances\. There is at least one file with the same name and in the same location that the second deployment group tries to deploy\. The second deployment fails because CodeDeploy does not remove the existing file before the second deployment starts\. Both deployments >reference different deployment group IDs\.
+ You deployed a revision in CodeDeploy, but there is at least one file with the same name and in the same location\. The deployment fails because, by default, CodeDeploy does not remove the existing file before the deployment starts\. 

To address these situations, do one of the following:
+ Remove the files from the locations and instances to which they were previously deployed, and then try the deployment again\. 
+ In your revision's AppSpec file, in either the ApplicationStop or BeforeInstall deployment lifecycle events, specify a custom script to delete files in any locations that match the files your revision is about to install\.
+ Deploy or redeploy the files to locations or instances that were not part of previous deployments\.
+ Before you delete an application or a deployment group, deploy a revision that contains an AppSpec file that specifies no files to copy to the instances\. For the deployment, specify the application name and deployment group name that use the same underlying application and deployment group IDs as those you are about to delete\. \(You can use the [get\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-group.html) command to retrieve the deployment group ID\.\) CodeDeploy uses the underlying deployment group ID and AppSpec file to remove all of the files it installed in the previous successful deployment\. 

## Troubleshooting a failed AllowTraffic lifecycle event with no error reported in the deployment logs<a name="troubleshooting-deployments-allowtraffic-no-logs"></a>

In some cases, a blue/green deployment fails during the AllowTraffic lifecycle event, but the deployment logs do not indicate the cause for the failure\.

This failure is typically due to incorrectly configured health checks in Elastic Load Balancing for the Classic Load Balancer, Application Load Balancer, or Network Load Balancer used to manage traffic for the deployment group\.

To resolve the issue, review and correct any errors in the health check configuration for the load balancer\.

For Classic Load Balancers, see [Configure Health Checks](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-healthchecks.html) in the *User Guide for Classic Load Balancers* and [ConfigureHealthCheck](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_ConfigureHealthCheck.html) in the *Elastic Load Balancing API Reference version 2012\-06\-01*\.

For Application Load Balancers, see [Health Checks for Your Target Groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) in the *User Guide for Application Load Balancers*\.

For Network Load Balancers, see [Health Checks for Your Target Groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/target-group-health-checks.html) in the *Network Load Balancer User Guide*\.

## Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events<a name="troubleshooting-deployments-lifecycle-event-failures"></a>

During a deployment, the CodeDeploy agent runs the scripts specified for ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic in the AppSpec file from the previous successful deployment\. \(All other scripts are run from the AppSpec file in the current deployment\.\) If one of these scripts contains an error and does not run successfully, the deployment can fail\. 

Possible reasons for these failures include:
+ The CodeDeploy agent finds the `deployment-group-id_last_successful_install` file in the correct location, but the location listed in the `deployment-group-id_last_successful_install` file does not exist\. 

  **On Amazon Linux, Ubuntu Server, and RHEL instances**, this file must exist in `/opt/codedeploy-agent/deployment-root/deployment-instructions`\.

  **On Windows Server instances**, this file must be stored in the `C:\ProgramData\Amazon\CodeDeploy\deployment-instructions` folder\.
+ In the location listed in the `deployment-group-id_last_successful_install` file, either the AppSpec file is invalid or the scripts do not run successfully\.
+ The script contains an error that cannot be corrected, so it never runs successfully\.

Use the CodeDeploy console to investigate why a deployment might have failed during any of these events\. On the details page for the deployment, choose **View events**\. On the details page for the instance, in the **ApplicationStop**, **BeforeBlockTraffic**, or **AfterBlockTraffic** row, choose **View logs**\. Or use the AWS CLI to call the [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\. 

If the cause of the failure is a script from the last successful deployment that never runs successfully, create a deployment and specify that the ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic failures should be ignored\. There are two ways to do this:
+ Use the CodeDeploy console to create a deployment\. On the **Create deployment** page, under **ApplicationStop lifecycle event failure**, choose **Don't fail the deployment to an instance if this lifecycle event on the instance fails**\.
+ Use the AWS CLI to call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command and include the `--ignore-application-stop-failures` option\. 

When you deploy the application revision again, the deployment continues even if any of these three lifecycle events fail\. If the new revision includes fixed scripts for those lifecycle events, future deployments can succeed without applying this fix\.

## Troubleshooting a failed DownloadBundle deployment lifecycle event with UnknownError: not opened for reading<a name="troubleshooting-deployments-downloadbundle"></a>

If you are trying to deploy an application revision from Amazon S3, and the deployment fails during the DownloadBundle deployment lifecycle event with the `UnknownError: not opened for reading` error:
+ There was internal Amazon S3 service error\. Deploy the application revision again\.
+ The IAM instance profile on your Amazon EC2 instance does not have permissions to access the application revision in Amazon S3\. For information about Amazon S3 bucket policies, see [Push a revision for CodeDeploy to Amazon S3 \(EC2/On\-Premises deployments only\)](application-revisions-push.md) and [Deployment prerequisites](deployments-create-prerequisites.md)\.
+ The instances you deploy to are associated with one AWS Region \(for example, US West \(Oregon\)\), but the Amazon S3 bucket that contains the application revision is associated with another AWS Region \(for example, US East \(N\. Virginia\)\)\. Make sure the application revision is in an Amazon S3 bucket associated with the same AWS Region as the instances\.

On the event details page for the deployment, in the **Download bundle** row, choose **View logs**\. Or use the AWS CLI to call the [get\-deployment\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\. If this error occurred, there should be an error in the output with the error code `UnknownError` and the error message `not opened for reading`\.

To determine the reason for this error:

1. Enable wire logging on at least one of the instances, and then deploy the application revision again\.

1. Examine the wire logging file to find the error\. Common error messages for this issue include the phrase "access denied\." 

1. After you have examined the log files, we recommend that you disable wire logging to reduce log file size and the amount of sensitive information that might appear in the output in plain text on the instance in the future\.

For information about how to find the wire logging file and enable and disable wire logging, see `:log_aws_wire:` in [CodeDeploy agent configuration reference](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-agent-configuration.html)\.

## Windows PowerShell scripts fail to use the 64\-bit version of Windows PowerShell by default<a name="troubleshooting-deployments-powershell"></a>

If a Windows PowerShell script running as part of a deployment relies on 64\-bit functionality \(for example, because it consumes more memory than a 32\-bit application allows or calls libraries that are offered only in a 64\-bit version\), the script might crash or not run as expected\. This is because, by default, CodeDeploy uses the 32\-bit version of Windows PowerShell to run Windows PowerShell scripts that are part of an application revision\. 

Add code like the following to the beginning of any script that must run with the 64\-bit version of Windows PowerShell:

```
# Are you running in 32-bit mode?
#   (\SysWOW64\ = 32-bit mode)

if ($PSHOME -like "*SysWOW64*")
{
  Write-Warning "Restarting this script under 64-bit Windows PowerShell."

  # Restart this script under 64-bit Windows PowerShell.
  #   (\SysNative\ redirects to \System32\ for 64-bit mode)

  & (Join-Path ($PSHOME -replace "SysWOW64", "SysNative") powershell.exe) -File `
    (Join-Path $PSScriptRoot $MyInvocation.MyCommand) @args

  # Exit 32-bit script.

  Exit $LastExitCode
}

# Was restart successful?
Write-Warning "Hello from $PSHOME"
Write-Warning "  (\SysWOW64\ = 32-bit mode, \System32\ = 64-bit mode)"
Write-Warning "Original arguments (if any): $args"

# Your 64-bit script code follows here...
# ...
```

Although the file path information in this code might seem counterintuitive, 32\-bit Windows PowerShell uses a path like:

 `c:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

64\-bit Windows PowerShell uses a path like:

 `c:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`

## Long\-running processes can cause deployments to fail<a name="troubleshooting-long-running-processes"></a>

For deployments to Amazon Linux, Ubuntu Server, and RHEL instances, if you have a deployment script that starts a long\-running process, CodeDeploy might spend a long time waiting in the deployment lifecycle event and then fail the deployment\. This is because if the process runs longer than the foreground and background processes in that event are expected to take, CodeDeploy stops and fails the deployment, even if the process is still running as expected\.

For example, an application revision contains two files in its root, `after-install.sh` and `sleep.sh`\. Its AppSpec file contains the following instructions:

```
version: 0.0
os: linux
files:
  - source: ./sleep.sh
    destination: /tmp
hooks:
  AfterInstall:
    - location: after-install.sh
      timeout: 60
```

The `after-install.sh` file runs during the AfterInstall application lifecycle event\. Here are its contents:

```
#!/bin/bash
/tmp/sleep.sh
```

The `sleep.sh` file contains the following, which suspends program execution for three minutes \(180 seconds\), simulating some long\-running process:

```
#!/bin/bash
sleep 180
```

When `after-install.sh` calls `sleep.sh`, `sleep.sh` starts and runs for three minutes \(180 seconds\), which is two minutes \(120 seconds\) past the time CodeDeploy expects `sleep.sh` \(and, by relation, `after-install.sh`\) to stop running\. After the timeout of one minute \(60 seconds\), CodeDeploy stops and fails the deployment at the AfterInstall application lifecycle event, even though `sleep.sh` continues to run as expected\. The following error is displayed:

`Script at specified location: after-install.sh failed to complete in 60 seconds`\.

You cannot simply add an ampersand \(`&`\) in `after-install.sh` to run `sleep.sh` in the background\.

```
#!/bin/bash
# Do not do this.
/tmp/sleep.sh &
```

Doing so can leave the deployment in a pending state for up to the default one\-hour deployment lifecycle event timeout period, after which CodeDeploy stops and fails the deployment at the AfterInstall application lifecycle event as before\.

In `after-install.sh`, call `sleep.sh` as follows, which enables CodeDeploy to continue after the process starts running:

```
#!/bin/bash
/tmp/sleep.sh > /dev/null 2> /dev/null < /dev/null &
```

In the preceding call, `sleep.sh` is the name of the process you want to start running in the background, redirecting stdout, stderr, and stdin to `/dev/null`\.

## Troubleshooting all lifecycle events skipped errors<a name="troubleshooting-skipped-lifecycle-events"></a>

 If all lifecycle events of an Amazon EC2 or on\-premises deployment are skipped, you might receive an error similar to `The overall deployment failed because too many individual instances failed deployment, too few healthy instances are available for deployment, or some instances in your deployment group are experiencing problems. (Error code: HEALTH_CONSTRAINTS)`\. Here are some possible causes and solutions: 
+ The CodeDeploy agent might not be installed or running on the instance\. To determine if the CodeDeploy agent is running:
  + For Amazon Linux RHEL or Ubuntu server, run the following:

    ```
    sudo service codedeploy-agent status
    ```
  + For Windows, run the following:

    ```
    powershell.exe -Command Get-Service -Name codedeployagent
    ```

  If the CodeDeploy agent is not installed or running, see [Verify the CodeDeploy agent is running](codedeploy-agent-operations-verify.md)\. 

  Your instance might not be able to reach the CodeDeploy or S3 public endpoint using port 443\. Try one of the following: 
  + Assign a public IP address to the instance and use its route table to allow internet access\. Make sure the security group associated with the instance allows outbound access over port 443 \(HTTPS\)\. For more information, see [Communication protocol and port for the CodeDeploy agent](codedeploy-agent.md#codedeploy-agent-outbound-port)\. 
  + If an instance is provisioned in a private subnet, use a NAT gateway instead of an internet gateway in the route table\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\. 
+ The service role for CodeDeploy might not have required permissions\. To configure a CodeDeploy service role, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\. 
+ If you use an HTTP proxy, make sure it is specified in the `:proxy_uri:` setting in the CodeDeploy agent configuration file\. For more information, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\. 
+ The date and time signature of your deployment instance might not match the date and time signature of your deployment request\. Look for an error similar to `Cannot reach InstanceService: Aws::CodeDeployCommand::Errors::InvalidSignatureException - Signature expired` in your CodeDeploy agent log file\. If you see this error, follow the steps in [Troubleshooting “InvalidSignatureException – Signature expired: \[time\] is now earlier than \[time\]” deployment errors](troubleshooting-ec2-instances.md#troubleshooting-instance-time-failures)\. For more information, see [View log data for CodeDeploy EC2/On\-Premises deployments](deployments-view-logs.md)\. 
+ The CodeDeploy agent might stop running because an instance is running low on memory or hard disk space\. Try to lower the number of archived deployments on your instance by updating the `max_revisions` setting in the CodeDeploy agent configuration\. If you do this for an Amazon EC2 instance and the issue persists, consider using a larger instance\. For example, if your instance type is `t2.small`, try using a `t2.medium`\. For more information, see [Files installed by the CodeDeploy agent ](codedeploy-agent.md#codedeploy-agent-install-files), [CodeDeploy agent configuration reference](reference-agent-configuration.md), and [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\. 
+  The instance you're deploying to might not have an IAM instance profile attached, or it might have an IAM instance profile attached that does not have the required permissions\. 
  +  If an IAM instance profile is not attached to your instance, create one with the required permissions and then attach it\. 
  +  If an IAM instance profile is already attached to your instance, make sure it has the required permissions\. 

  After you confirm your attached instance profile is configured with the required permissions, restart your instance\. For more information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md) and [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the *Amazon EC2 User Guide*\. 

## CodeDeploy plugin CommandPoller missing credentials error<a name="troubleshooting-agent-commandpoller-error"></a>

 If you receive an error similar to `InstanceAgent::Plugins::CodeDeployPlugin::CommandPoller: Missing credentials - please check if this instance was started with an IAM instance profile`, it might be caused by one of the following: 
+  The instance you are deploying to does not have an IAM instance profile associated with it\. 
+  Your IAM instance profile does not have the correct permissions configured\. 

 An IAM instance profile grants the CodeDeploy agent permission to communicate with CodeDeploy and to download your revision from Amazon S3\. For Amazon EC2 instances, see [Identity and access management for AWS CodeDeploy](security-iam.md)\. For on\-premises instances, see [Working with on\-premises instances for CodeDeploy](instances-on-premises.md)\. 
# Troubleshoot EC2/On\-PremisesDeployment Issues<a name="troubleshooting-deployments"></a>

**Topics**
+ [Deployment fails with the message “Validation of PKCS7 signed message failed”](#troubleshooting-deployments-agent-SHA-256)
+ [Deployment or redeployment of the same files to the same instance locations fail with the error "The deployment failed because a specified file already exists at this location"](#troubleshooting-same-files-different-app-name)
+ [Troubleshooting a failed AllowTraffic lifecycle event with no error reported in the deployment logs](#troubleshooting-deployments-allowtraffic-no-logs)
+ [Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events](#troubleshooting-deployments-lifecycle-event-failures)
+ [Troubleshooting a failed DownloadBundle deployment lifecycle event with "UnknownError: not opened for reading"](#troubleshooting-deployments-downloadbundle)
+ [Windows PowerShell scripts fail to use the 64\-bit version of Windows PowerShell by default](#troubleshooting-deployments-powershell)
+ [Long\-running processes can cause deployments to fail](#troubleshooting-long-running-processes)

**Note**  
The causes of many deployment failures can be identified by reviewing the log files created during the deployment process\. For simplicity, we recommend using Amazon CloudWatch Logs to centrally monitor log files instead of viewing them instance by instance\. For information, see [View AWS CodeDeploy Logs in CloudWatch Logs Console](http://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)\.

## Deployment fails with the message “Validation of PKCS7 signed message failed”<a name="troubleshooting-deployments-agent-SHA-256"></a>

This error message indicates the instance is running a version of the AWS CodeDeploy agent that supports only the SHA\-1 hash algorithm\. Support for the SHA\-2 hash algorithm was introduced in version 1\.0\.1\.854 of the AWS CodeDeploy agent, released in November 2015\. Effective October 17, 2016, deployments will fail if a version of the AWS CodeDeploy agent earlier than 1\.0\.1\.854 is installed\. For more information, see [AWS to Switch to SHA256 Hash Algorithm for SSL Certificates](https://aws.amazon.com/security/security-bulletins/aws-to-switch-to-sha256-hash-algorithm-for-ssl-certificates/), [NOTICE: Retiring AWS CodeDeploy host agents older than version 1\.0\.1\.85](https://forums.aws.amazon.com/thread.jspa?threadID=223319), and [Update the AWS CodeDeploy Agent](codedeploy-agent-operations-update.md)\.

## Deployment or redeployment of the same files to the same instance locations fail with the error "The deployment failed because a specified file already exists at this location"<a name="troubleshooting-same-files-different-app-name"></a>

When AWS CodeDeploy tries to deploy a file to an instance but a file with the same name already exists in the specified target location, the deployment to that instance may fail\. You may receive the error message "The deployment failed because a specified file already exists at this location: *location\-name*\." This is because, during each deployment, AWS CodeDeploy first deletes all files from the previous deployment, which are listed in a cleanup log file\. If there are files in the target installation folders that aren’t listed in this cleanup file, the AWS CodeDeploy agent by default interprets this as an error and fails the deployment\.

**Note**  
On Amazon Linux, RHEL, and Ubuntu Server instances, the cleanup file is located in `/opt/codedeploy-agent/deployment-root/deployment-instructions/`\. On Windows Server instances, the location is `C:\ProgramData\Amazon\CodeDeploy\deployment-instructions\`\.

The easiest way to avoid this error is to specify an option other than the default behavior to fail the deployment\. For each deployment, you can choose whether to fail the deployment, to overwrite the files not listed in the cleanup file, or to retain the files already on the instance\.

The overwrite option is useful when, for example, you manually placed a file on an instance after the last deployment, but then added a file of the same name to the next application revision\.

You might choose the retain option for files you place on the instance that you want to be part of the next deployment without having to add them to the application revision package\. The retain option is also useful if your application files are already in your production environment and you want to deploy using AWS CodeDeploy for the first time\. For more information, see [Create an EC2/On\-Premises Compute Platform Deployment \(Console\)](deployments-create-console.md) and [Rollback Behavior with Existing Content](deployments-rollback-and-redeploy.md#deployments-rollback-and-redeploy-content-options)\.

### Troubleshooting "The deployment failed because a specified file already exists at this location" deployment errors<a name="troubleshooting-same-files-different-app-name-failed-deployment"></a>

If you choose not to specify an option to overwrite or retain content that AWS CodeDeploy detects in your target deployment locations \(or if you do not specify any deployment option for handling existing content in a programmatic command\), you can choose to troubleshoot the error\.

The following information applies only if you choose not to retain or overwrite the content\.

If you try to redeploy files with the same names and locations, the redeployment will have a better chance of succeeding if you specify the application name and the deployment group with the same underlying deployment group ID you used before\. AWS CodeDeploy uses the underlying deployment group ID to identify files to remove before a redeployment\. 

Deploying new files or redeploying the same files to the same locations on instances can fail for these reasons:
+ You specified a different application name for a redeployment of the same revision to the same instances\. The redeployment will fail because even if the deployment group name is the same, the use of a different application name means a different underlying deployment group ID will be used\.
+ You deleted and re\-created a deployment group for an application and then tried to redeploy the same revision to the deployment group\. The redeployment will fail because even if the deployment group name is the same, AWS CodeDeploy will reference a different underlying deployment group ID\.
+ You deleted an application and deployment group in AWS CodeDeploy, then created a new application and deployment group with the same names as the ones you deleted\. After that, you tried to redeploy a revision that had been deployed to the previous deployment group to the new one with the same name\. The redeployment will fail because even though the application and deployment group names are the same, AWS CodeDeploy still references the ID of the deployment group you deleted\.
+ You deployed a revision to a deployment group and then deployed the same revision to another deployment group to the same instances\. The second deployment will fail because AWS CodeDeploy will reference a different underlying deployment group ID\.
+ You deployed a revision to one deployment group and then deployed another revision to another deployment group to the same instances\. There is at least one file with the same name and in the same location that the second deployment group tries to deploy\. The second deployment will fail because AWS CodeDeploy will not remove the existing file before the second deployment starts\. Both deployments will reference different deployment group IDs\.
+ You deployed a revision in AWS CodeDeploy, but there is at least one file with the same name and in the same location\. The deployment will fail because, by default, AWS CodeDeploy will not remove the existing file before the deployment starts\. 

To address these situations, do one of the following:
+ Remove the files from the locations and instances to which they were previously deployed, and then try the deployment again\. 
+ In your revision's AppSpec file, in either the **ApplicationStop** or **BeforeInstall** deployment lifecycle events, specify a custom script to delete files in any locations that match the files your revision is about to install\.
+ Deploy or redeploy the files to locations or instances that were not part of previous deployments\.
+ Before you delete an application or a deployment group, deploy a revision that contains an AppSpec file that specifies no files to copy to the instances\. For the deployment, specify the application name and deployment group name that use the same underlying application and deployment group IDs as those you are about to delete\. \(You can use the [get\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-group.html) command to retrieve the deployment group ID\.\) AWS CodeDeploy will use the underlying deployment group ID and AppSpec file to remove all of the files it installed in the previous successful deployment\. 

## Troubleshooting a failed AllowTraffic lifecycle event with no error reported in the deployment logs<a name="troubleshooting-deployments-allowtraffic-no-logs"></a>

In some cases, a blue/green deployment fails during the AllowTraffic lifecycle event, but no cause for the failure is indicated in the deployment logs\.

This failure is typically due to the health checks being configured incorrectly in Elastic Load Balancing for the Classic Load Balancer, Application Load Balancer, or Network Load Balancer used to manage traffic for the deployment group\.

To resolve the issue, review and correct any errors in the the health check configuration for the load balancer\.

For Classic Load Balancers, see [Configure Health Checks](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-healthchecks.html) in the *User Guide for Classic Load Balancers* and [ConfigureHealthCheck](http://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_ConfigureHealthCheck.html) in the *Elastic Load Balancing API Reference version 2012\-06\-01*\.

For Application Load Balancers, see [Health Checks for Your Target Groups](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) in the *User Guide for Application Load Balancers*\.

For Network Load Balancers, see [Health Checks for Your Target Groups](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/target-group-health-checks.html) in the *Network Load Balancer User Guide*\.

## Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events<a name="troubleshooting-deployments-lifecycle-event-failures"></a>

During a deployment, the AWS CodeDeploy agent runs the scripts specified for ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic in the AppSpec file from the *previous* successful deployment\. \(All other scripts are run from the AppSpec file in the current deployment\.\) If one of these scripts contains an error and does not run successfully, the deployment can fail\. 

Possible reasons for these failures include:
+ The AWS CodeDeploy agent finds the `deployment-group-id_last_successful_install` file in the correct location, but the location listed in the `deployment-group-id_last_successful_install` file does not exist\. 

  **On Amazon Linux, Ubuntu Server, and RHEL instances**, this file must exist in `/opt/codedeploy-agent/deployment-root/deployment-instructions`\.

  **On Windows Server instances**, this file must be stored in the `C:\ProgramData\Amazon\CodeDeploy\deployment-instructions` folder\.
+ In the location listed in the `deployment-group-id_last_successful_install` file, either the AppSpec file is invalid or the scripts do not run successfully\.
+ The script contains an error that cannot be corrected, so it will never run successfully\.

Use the AWS CodeDeploy console to investigate why a deployment might have failed during any of these events\. On the details page for the deployment, choose **View events**\. On the details page for the instance, in the **ApplicationStop**, **BeforeBlockTraffic**, or **AfterBlockTraffic** row, choose **View logs**\. Alternatively, use the AWS CLI to call the [get\-deployment\-instance](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\. 

If the cause of the failure is a script from the last successful deployment that will never run successfully, create a new deployment and specify that the **ApplicationStop**, **BeforeBlockTraffic**, and **AfterBlockTraffic** failures should be ignored\. You can do this in two ways:
+ Use the AWS CodeDeploy console to create a deployment\. On the **Create deployment** page, under **ApplicationStop lifecycle event failure**, choose **Don't fail the deployment to an instance if this lifecycle event on the instance fails**\.
+ Use the AWS CLI to call the `[create\-deployment](http://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html)` command and include the `--ignore-application-stop-failures` option\. 

When you deploy the application revision again, the deployment will continue even if any of these three lifecycle events fail\. If the new revision includes fixed scripts for those lifecycle events, future deployments can succeed without applying this fix\.

## Troubleshooting a failed DownloadBundle deployment lifecycle event with "UnknownError: not opened for reading"<a name="troubleshooting-deployments-downloadbundle"></a>

If you are trying to deploy an application revision from Amazon S3, and the deployment fails during the **DownloadBundle** deployment lifecycle event with the "UnknownError: not opened for reading" error:
+ There was internal Amazon S3 service error\. Deploy the application revision again\.
+ The IAM instance profile on your Amazon EC2 instance does not have permissions to access the application revision in Amazon S3\. For information about Amazon S3 bucket policies, see [Push a Revision for AWS CodeDeploy to Amazon S3](application-revisions-push.md) and [Deployment Prerequisites](deployments-create-prerequisites.md)\.
+ The instances to which you will deploy are associated with one region \(for example, US West \(Oregon\)\), but the Amazon S3 bucket that contains the application revision is associated with another region \(for example, US East \(N\. Virginia\)\)\. Make sure the application revision is in an Amazon S3 bucket associated with the same region as the instances\.

On the event details page for the deployment, in the **Download bundle** row, choose **View logs**\. Alternatively, use the AWS CLI to call the [get\-deployment\-instance](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-instance.html) command\. If this error occurred, there should be an error in the output with the error code "UnknownError" and the error message "not opened for reading\."

To determine the reason for this error:

1. Enable wire logging on at least one of the instances, and then deploy the application revision again\.

1. Examine the wire logging file to find the error\. Common error messages for this issue include the phrase "access denied\." 

1. After you have examined the log files, we recommend that you disable wire logging to reduce log file size and the amount of sensitive information that may appear in the output in plain text on the instance in the future\.

To learn how to find the wire logging file and enable and disable wire logging, see **:log\_aws\_wire:** in [Working with the AWS CodeDeploy Agent](codedeploy-agent.md)\.

## Windows PowerShell scripts fail to use the 64\-bit version of Windows PowerShell by default<a name="troubleshooting-deployments-powershell"></a>

If a Windows PowerShell script running as part of a deployment relies on 64\-bit functionality \(for example, because it consumes more memory than a 32\-bit application will allow or calls libraries that are offered only in a 64\-bit version\), the script may crash or otherwise not run as expected\. This is because, by default, AWS CodeDeploy uses the 32\-bit version of Windows PowerShell to run Windows PowerShell scripts that are part of an application revision\. 

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

Although the file path information in this code may seem counterintuitive, 32\-bit Windows PowerShell uses a path like:

 `c:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

64\-bit Windows PowerShell uses a path like:

 `c:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`

## Long\-running processes can cause deployments to fail<a name="troubleshooting-long-running-processes"></a>

For deployments to Amazon Linux, Ubuntu Server, and RHEL instances, if you have a deployment script that starts a long\-running process, AWS CodeDeploy may spend a long time waiting in the deployment lifecycle event and then fail the deployment\. This is because if the process runs longer than the foreground and background processes in that event are expected to take, AWS CodeDeploy stops and fails the deployment, even if the process is still running as expected\.

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

The `after-install.sh` file runs during the **AfterInstall** application lifecycle event\. Here are its contents:

```
#!/bin/bash
/tmp/sleep.sh
```

The `sleep.sh` file contains the following, which suspends program execution for three minutes \(180 seconds\), simulating some long\-running process:

```
#!/bin/bash
sleep 180
```

When `after-install.sh` calls `sleep.sh`, `sleep.sh` will start and keep running for three minutes \(180 seconds\), which is two minutes \(120 seconds\) past the time AWS CodeDeploy expects `sleep.sh` \(and, by relation, `after-install.sh`\) to stop running\. After the timeout of one minute \(60 seconds\), AWS CodeDeploy stops and fails the deployment at the **AfterInstall** application lifecycle event, even though `sleep.sh` continues to run as expected\. The following error is displayed:

`Script at specified location: after-install.sh failed to complete in 60 seconds`\.

You cannot simply add an ampersand \(`&`\) in `after-install.sh` to run `sleep.sh` in the background\.

```
#!/bin/bash
# Do not do this.
/tmp/sleep.sh &
```

Doing so can leave the deployment in a pending state for up to the default one\-hour deployment lifecycle event timeout period, after which AWS CodeDeploy stops and fails the deployment at the **AfterInstall** application lifecycle event as before\.

In `after-install.sh`, call `sleep.sh` as follows, which enables AWS CodeDeploy to continue after the process starts running:

```
#!/bin/bash
/tmp/sleep.sh > /dev/null 2> /dev/null < /dev/null &
```

In the preceding call, `sleep.sh` is the name of the process you want to start running in the background, redirecting stdout, stderr, and stdin to `/dev/null`\.
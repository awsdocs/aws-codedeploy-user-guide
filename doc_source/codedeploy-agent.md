# Working with the CodeDeploy agent<a name="codedeploy-agent"></a>

 The CodeDeploy agent is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments\. You must use version 1\.0\.1\.1458 or later\. If you use an earlier version, deployments to your instances might fail\.

**Note**  
 The CodeDeploy agent is required only if you deploy to an EC2/On\-Premises compute platform\. The agent is not required for deployments that use the Amazon ECS or AWS Lambda compute platform\. 

A configuration file is placed on the instance when the agent is installed\. This file is used to specify how the agent works\. This configuration file specifies directory paths and other settings for AWS CodeDeploy to use as it interacts with the instance\. You can change some of the configuration options in the file\. For information about working with the CodeDeploy agent configuration file, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\.

For more information about working with the CodeDeploy agent, such as steps for installing, updating, and verifying versions, see [Managing CodeDeploy agent operations](codedeploy-agent-operations.md)\.

**Topics**
+ [Operating systems supported by the CodeDeploy agent](#codedeploy-agent-supported-operating-systems)
+ [Communication protocol and port for the CodeDeploy agent](#codedeploy-agent-outbound-port)
+ [Version history of the CodeDeploy agent](#codedeploy-agent-version-history)
+ [Application revision and log file cleanup](#codedeploy-agent-revisions-logs-cleanup)
+ [Files installed by the CodeDeploy agent](#codedeploy-agent-install-files)
+ [Managing CodeDeploy agent operations](codedeploy-agent-operations.md)

## Operating systems supported by the CodeDeploy agent<a name="codedeploy-agent-supported-operating-systems"></a>

### Supported Amazon EC2 AMI operating systems<a name="codedeploy-agent-supported-operating-systems-ec2"></a>

The CodeDeploy agent has been tested on the following Amazon EC2 AMI operating systems:
+ Amazon Linux 2018\.03\.x, 2017\.03\.x, 2016\.09\.x, 2016\.03\.x, and 2014\.09\.x
+ Amazon Linux 2 \(ARM, x86\)
+ Ubuntu Server 20\.04 LTS, 19\.10, 18\.04 LTS, 16\.04 LTS, and 14\.04 LTS
+ Microsoft Windows Server 2019, 2016, 2012 R2, and 2008 R2
+ Red Hat Enterprise Linux \(RHEL\) 7\.x

The CodeDeploy agent is available as open source for you to adapt to your needs\. It can be used with other Amazon EC2 AMI operating systems\. For more information, go to the [CodeDeploy agent](https://github.com/aws/aws-codedeploy-agent) repository in GitHub\.

### Supported on\-premises operating systems<a name="codedeploy-agent-supported-operating-systems-on-premises"></a>

The CodeDeploy agent has been tested on the following on\-premises operating systems:
+ Ubuntu Server 20\.04 LTS, 19\.10, 18\.04 LTS, 16\.04 LTS, and 14\.04 LTS
+ Microsoft Windows Server 2019, 2016, 2012 R2, and 2008 R2 
+ Red Hat Enterprise Linux \(RHEL\) 7\.x

The CodeDeploy agent is available as open source for you to adapt to your needs\. It can be used with other on\-premises instance operating systems\. For more information, go to the [CodeDeploy agent](https://github.com/aws/aws-codedeploy-agent) repository in GitHub\.

## Communication protocol and port for the CodeDeploy agent<a name="codedeploy-agent-outbound-port"></a>

The CodeDeploy agent communicates outbound using HTTPS over port 443\.

When the CodeDeploy agent runs on an EC2 instance, it will use the [EC2 metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html) endpoint to retrieve instance related information\. Find out more about [limiting and granting instance metadata service access](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html#instance-metadata-limiting-access)\.

## Version history of the CodeDeploy agent<a name="codedeploy-agent-version-history"></a>

Your instances must be running a supported version of the CodeDeploy agent\. The current minimum supported version is 1\.0\.1\.1458\. If you are running an earlier version, deployments to your instances might fail\. 

The following table lists all releases of the CodeDeploy agent and the features and enhancements included with each version\.


| Version | Release date | Details | 
| --- | --- | --- | 
|  1\.2\.0  |  August 26, 2020  |  **Changed**: Upgraded AWS Ruby SDK dependency from v2 to v3\. **Added**: Support for IMDSv2\. Includes a silent fallback to IMDSv1 if IMDSv2 http requests fail\. **Changed**: Updated Rake and Rubyzip dependencies for security patches\. **Fixed**: Ensure that an empty PID file will return a status of `No CodeDeploy Agent Running` and clean up the PID file on agent start\.  | 
|  1\.1\.2  |  August 4, 2020  |  **Added**: Support for Ubuntu Server 19\.10 and 20\.04\. **Added**: Memory efficiency improvements for Linux and Ubuntu to release reserved memory more timely\. **Added**: Compatibility with Windows Server "silent\-cleanup" which was causing the agent to be unresponsive in some cases\. **Added**: Ignore non\-empty directories during cleanup to avoid failures on deployment\. **Added**: Support for AWS Local Zone in Los Angeles \(LA\)\. **Added**: Extract AZ from instance metadata to provide compatibility for AWS Local Zones\. **Added**: Users can now provide their archive in subdirectories and aren't required to store it in the root directory\. **Added**: Detected an issue with Rubyzip that could result in memory leaks\. Updated the unzip command to first attempt to use a system\-installed unzip utility before using Rubyzip\. **Added**: `:deploy_control_endpoint:`, `:s3_endpoint_override:`, and `:enable_auth_policy:` as configurable variables\. **Changed**: Unzip warnings are now ignored so deployments will continue\.  | 
|  1\.1\.0  |  June 30, 2020  |  **Changed**: Versioning of the CodeDeploy agent now follows the Ruby standard versioning convention\. **Added**: New parameter to the install and update command to allow installation of specific agent version from the command line\. **Removed**: Removed the CodeDeploy agent Auto Updater for Linux and Ubuntu\. To configure automatic updates of the CodeDeploy agent, see [Install the CodeDeploy agent using AWS Systems Manager](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ssm.html)\.  | 
|  1\.0\.1\.1597  |  November 15, 2018  |  **Enhancement**: CodeDeploy supports Ubuntu 18\.04\. **Enhancement**: CodeDeploy supports Ruby 2\.5\. **Enhancement**: CodeDeploy supports FIPS endpoints\. For more information about FIPS endpoints, see [FIPS 140\-2 overview](https://aws.amazon.com/compliance/fips/)\. For endpoints that can be used with CodeBuild, see [CodeDeploy regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region)\.  | 
|  1\.0\.1\.1518  |  June 12, 2018  |  **Enhancement**: Fixed an issue that caused an error when the CodeDeploy agent is closed while it is accepting poll requests\. **Enhancement**: Added a deployment tracking feature that prevents the CodeDeploy agent from being closed when a deployment is in progress\. **Enhancement**: Improved performance when deleting files\.  | 
|  1\.0\.1\.1458  |  March 6, 2018  |   The minimum supported version of the CodeDeploy agent is 1\.0\.1\.1458\. Use of an earlier CodeDeploy agent might cause deployments to fail\.  **Enhancement**: Improved certificate validations to support more trusted authorities\. **Enhancement**: Fixed an issue that caused the local CLI to fail during a deployment that includes a BeforeInstall lifecycle event\. **Enhancement**: Fixed an issue that might cause an active deployment to fail when the CodeDeploy agent is updated\.  | 
|  1\.0\.1\.1352  |  November 16, 2017  |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Feature**: Introduced a new feature for testing and debugging an EC2/On\-Premises deployment on a local machine or instance where the CodeDeploy agent is installed\.  | 
|  1\.0\.1\.1106  |  May 16, 2017  |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Feature**: Introduced new support for handling content in a target location that wasn't part of the application revision from the most recent successful deployment\. Deployments options for existing content now include retaining the content, overwriting the content, or failing the deployment\.  **Enhancement**: Made the CodeDeploy agent compatible with version 2\.9\.2 of the AWS SDK for Ruby \(aws\-sdk\-core 2\.9\.2\)\.  | 
|  1\.0\.1\.1095  |  March 29, 2017  |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Enhancement**: Introduced support for the CodeDeploy agent in the China \(Beijing\) Region\. **Enhancement**: Enabled Puppet to run on Windows Server instances when invoked by a lifecycle event hook\. **Enhancement**: Improved the handling of `untar` operations\.  | 
| 1\.0\.1\.1067 | January 6, 2017 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Enhancement**: Revised many error messages to include more specific causes for deployment failures\. **Enhancement**: Fixed an issue that prevented the CodeDeploy agent from identifying the correct application revision to deploy during some deployments\. **Enhancement**: Reverted the usage of `pushd` and `popd` before and after the `untar` operation\.  | 
| 1\.0\.1\.1045 | November 21, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Enhancement**: Made the CodeDeploy agent compatible with version 2\.6\.11 of the AWS SDK for Ruby \(aws\-sdk\-core 2\.6\.11\)\.   | 
| 1\.0\.1\.1037 | October 19, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. The CodeDeploy agent for Amazon Linux, RHEL, and Ubuntu Server instances has been updated with the following change\. For Windows Server instances, the latest version remains 1\.0\.1\.998\. **Enhancement**: The agent can now determine which version of Ruby is installed on an instance so it can invoke the `codedeploy-agent` script using that version\.  | 
| 1\.0\.1\.1011\.1 | August 17, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Enhancement**: Removed the changes introduced by version 1\.0\.1\.1011 due to issues with shell support\. This version of the agent is functionally equivalent to version 1\.0\.1\.998 released on July 11, 2016\. | 
| 1\.0\.1\.1011 | August 15, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. The CodeDeploy agent for Amazon Linux, RHEL, and Ubuntu Server instances has been updated with the following changes\. For Windows Server instances, the latest version remains 1\.0\.1\.998\.**Feature**: Added support for invoking the CodeDeploy agent using the bash shell on operating systems where the systemd init system is in use\.Enhancement: Enabled support for all versions of Ruby 2\.x in the CodeDeploy agent and the CodeDeploy agent updater\. Updated CodeDeploy agents are no longer dependent on Ruby 2\.0 only\. \(Ruby 2\.0 is still required for deb and rpm versions of the CodeDeploy agent installer\.\) | 
| 1\.0\.1\.998 | July 11, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Enhancement**: Fixed support for running the CodeDeploy agent with user profiles other than *root*\. The variable named `USER` is replaced by `CODEDEPLOY_USER` to avoid conflicts with environmental variables\.  | 
| 1\.0\.1\.966 | June 16, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Feature**: Introduced support for running the CodeDeploy agent with user profiles other than *root*\. **Enhancement**: Fixed support for specifying the number of application revisions you want the CodeDeploy agent to archive for a deployment group\. **Enhancement**: Made the CodeDeploy agent compatible with version 2\.3 of the AWS SDK for Ruby \(aws\-sdk\-core 2\.3\)\.  **Enhancement**: Fixed issues with UTF\-8 encoding during deployments\. **Enhancement**: Improved accuracy when identifying process names\.  | 
| 1\.0\.1\.950 | March 24, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Feature**: Added installation proxy support\. **Enhancement**: Updated the installation script to not download the CodeDeploy agent if the latest version is already installed\.  | 
| 1\.0\.1\.934 | February 11, 2016 |  **Note**: This version is no longer supported\. If you use this version, your deployments might fail\.\. **Feature**: Introduced support for specifying the number of application revisions you want the CodeDeploy agent to archive for a deployment group\.   | 
| 1\.0\.1\.880 | January 11, 2016 | **Note**: This version is no longer supported and might cause deployments to fail\. **Enhancement**: Made the CodeDeploy agent compatible with version 2\.2 of the AWS SDK for Ruby \(aws\-sdk\-core 2\.2\)\. Version 2\.1\.2 is still supported\.  | 
| 1\.0\.1\.854 | November 17, 2015 | **Note**: This version is no longer supported\. If you use this version, your deployments might fail\. **Feature**: Introduced support for the SHA\-256 hash algorithm\.  **Feature**: Introduced version tracking support in `.version` files\. **Feature**: Made the deployment group ID available through the use of an environment variable\. **Enhancement**: Added support for monitoring CodeDeploy agent logs using [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\.  | 

For related information, see the following:
+ [Determine the version of the CodeDeploy agent](codedeploy-agent-operations-version.md)
+ [Install the CodeDeploy agent](codedeploy-agent-operations-install.md)

For a history of CodeDeploy agent versions, see the [Release repository on GitHub](https://github.com/aws/aws-codedeploy-agent/releases)\.

## Application revision and log file cleanup<a name="codedeploy-agent-revisions-logs-cleanup"></a>

The CodeDeploy agent archives revisions and log files on instances\. The CodeDeploy agent cleans up these artifacts to conserve disk space\.

**Application revision deployment logs**: You can use the **:max\_revisions:** option in the agent configuration file to specify the number of application revisions to archive by entering any positive integer\. CodeDeploy also archives the log files for those revisions\. All others are deleted, with the exception of the log file of the last successful deployment\. That log file is always retained, even if the number of failed deployments exceeds the number of retained revisions\. If no value is specified, CodeDeploy retains the five most recent revisions in addition to the currently deployed revision\. 

**CodeDeploy logs**: For Amazon Linux, Ubuntu Server, and RHEL instances, the CodeDeploy agent rotates the log files under the `/var/log/aws/codedeploy-agent` folder\. The log file is rotated at 00:00:00 \(instance time\) daily\. Log files are deleted after seven days\. The naming pattern for rotated log files is `codedeploy-agent.YYYYMMDD.log`\.

## Files installed by the CodeDeploy agent<a name="codedeploy-agent-install-files"></a>

The CodeDeploy agent stores revisions, deployment history, and deployment scripts in its root directory on an instance\. The default name and location of this directory is:

`'/opt/codedeploy-agent/deployment-root'` for Amazon Linux, Ubuntu Server, and RHEL instances\.

`'C:\ProgramData\Amazon\CodeDeploy'` for Windows Server instances\. 

You can use the **root\_dir** setting in the CodeDeploy agent configuration file to configure the directory's name and location\. For more information, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\.

The following is an example of the file and directory structure under the root directory\. The structure assumes there are N number of deployment groups, and each deployment group contains N number of deployments\. 

```
|--deployment-root/
|-- deployment group 1 ID 
|    |-- deployment 1 ID 
|    |    |-- Contents and logs of the deployment's revision
|    |-- deployment 2 ID
|    |    |-- Contents and logs of the deployment's revision
|    |-- deployment N ID
|    |    |-- Contents and logs of the deployment's revision
|-- deployment group 2 ID
|    |-- deployment 1 ID
|    |    |-- bundle.tar
|    |    |-- deployment-archive
|    |    |    | -- contents of the deployment's revision
|    |    |-- logs
|    |    |    | -- scripts.log     
|    |-- deployment 2 ID
|    |    |-- bundle.tar
|    |    |-- deployment-archive
|    |    |    | -- contents of the deployment's revision
|    |    |-- logs
|    |    |    | -- scripts.log     
|    |-- deployment N ID
|    |    |-- bundle.tar
|    |    |-- deployment-archive
|    |    |    | -- contents of the deployment's revision
|    |    |-- logs
|    |    |    | -- scripts.log     
|-- deployment group N ID
|    |-- deployment 1 ID
|    |    |-- Contents and logs of the deployment's revision
|    |-- deployment 2 ID
|    |    |-- Contents and logs of the deployment's revision
|    |-- deployment N ID
|    |    |-- Contents and logs of the deployment's revision
|-- deployment-instructions
|    |-- [deployment group 1 ID]_cleanup
|    |-- [deployment group 2 ID]_cleanup
|    |-- [deployment group N ID]_cleanup
|    |-- [deployment group 1 ID]_install.json
|    |-- [deployment group 2 ID]_install.json
|    |-- [deployment group N ID]_install.json
|    |-- [deployment group 1 ID]_last_successful_install
|    |-- [deployment group 2 ID]_last_successful_install
|    |-- [deployment group N ID]_last_successful_install
|    |-- [deployment group 1 ID]_most_recent_install
|    |-- [deployment group 2 ID]_most_recent_install
|    |-- [deployment group N ID]_most_recent_install
|-- deployment-logs
|    |-- codedeploy-agent-deployments.log
```
+  **Deployment Group ID** folders represent each of your deployment groups\. A deployment group directory's name is its ID \(for example, `acde1916-9099-7caf-fd21-012345abcdef`\)\. Each deployment group directory contains one subdirectory for each attempted deployment in that deployment group\. 

   You can use the [batch\-get\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/batch-get-deployments.html) command to find a deployment group ID\.
+  **Deployment ID** folders represent each deployment in a deployment group\. Each deployment directory's name is its ID\. Each folder contains:
  +  **bundle\.tar**, a compressed file with the contents of the deployment's revision\. Use a zip decompression utility if you want to view the revision\.
  +  **deployment\-archive**, a directory that contains the contents of the deployment's revision\. 
  +  **logs**, a directory that contains a `scripts.log` file\. This file lists the output of all scripts specified in the deployment's AppSpec file\. 

   If you want to find the folder for a deployment but don't know its deployment ID or deployment group ID, you can use the [AWS CodeDeploy console](https://console.aws.amazon.com/codedeploy) or the AWS CLI to find them\. For more information, see [View CodeDeploydeployment details ](deployments-view-details.md)\. 

   The default maximum number of deployments that can be archived in a deployment group is five\. When that number is reached, future deployments are archived and the oldest archive is deleted\. You can use the **max\_revisions** setting in the CodeDeploy agent configuration file to change the default\. For more information, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\. 
**Note**  
 If you want to recover hard disk space used by archived deployments, update the **max\_revisions** setting to a low number, such as 1 or 2\. The next deployment deletes archived deployments so that the number is equal to the you specified\. 
+  **deployment\-instructions** contains four text files for each deployment group: 
  + **\[Deployment Group ID\]\-cleanup**, a text file with an undo verison of each command that is run during a deployment\. An example file name is `acde1916-9099-7caf-fd21-012345abcdef-cleanup`\. 
  + **\[Deployment Group ID\]\-install\.json**, a JSON file created during the most recent deployment\. It contains the commands run during the deployment\. An example file name is `acde1916-9099-7caf-fd21-012345abcdef-install.json`\.
  + **\[Deployment Group ID\]\_last\_successfull\_install**, a text file that lists the archive directory of the last successful deployment\. This file is created when the CodeDeploy agent has copied all files in the deployment application to the instance\. It is used by the CodeDeploy agent during the next deployment to determine which `ApplicationStop` and `BeforeInstall` scripts to run\. An example file name is `acde1916-9099-7caf-fd21-012345abcdef_last_successfull_install`\.
  + **\[Deployment Group ID\]\_most\_recent\_install**, a text file that lists the name of the archive directory of the most recent deployment\. This file is created when the files in the deployment are successfully downloaded\. The \[deployment group ID\]\_last\_successfull\_install file is created after this file, when the downloaded files are copied to their final destination\. An example file name is `acde1916-9099-7caf-fd21-012345abcdef_most_recent_install`\.
+  **deployment\-logs** contains the following log files: 
  +  **codedeploy\-agent\.yyyymmdd\.log** files are created for each day there is a deployment\. Each log file contains information about the day's deployments\. These log files might be useful for debugging problems like a permissions issue\. The log file is initially named `codedeploy-agent.log`\. The next day, the date of its deployments is inserted into the file name\. For example, if today is January 3, 2018, you can see information about all of today's deployments in `codedeploy-agent.log`\. Tomorrow, on January 4, 2018, the log file is renamed `codedeploy-agent.20180103.log`\. 
  +  **codedeploy\-agent\-deployments\.log** compiles the contents of `scripts.log` files for each deployment\. The `scripts.log` files are located in the `logs` subfolder under each `Deployment ID` folder\. The entries in this file are preceded by a deployment ID\. For example, "`[d-ABCDEF123]LifecycleEvent - BeforeInstall`" might be written during a deployment with an ID of `d-ABCDEF123`\. When `codedeploy-agent-deployments.log` reaches its maximum size, the CodeDeploy agent continues to write to it while deleting old content\. 
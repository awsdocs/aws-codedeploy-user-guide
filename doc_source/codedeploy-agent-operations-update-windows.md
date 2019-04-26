# Update the CodeDeploy Agent on Windows Server<a name="codedeploy-agent-operations-update-windows"></a>

To enable automatic updates of the CodeDeploy agent, install the CodeDeploy agent updater for Windows Server on new or existing instances\. The updater checks periodically for new versions\. When a new version is detected, the updater uninstalls the current version of the agent, if one is installed, before installing the newest version\.

If a deployment is already underway when the updater detects a new version, the deployment continues to completion\. If a deployment attempts to start during the update process, the deployment fails\.

If you want to force an update of the CodeDeploy agent, follow the instructions in [Install or reinstall the CodeDeploy agent for Windows Server](codedeploy-agent-operations-install-windows.md)\.

On Windows Server instances, you can download and install the CodeDeploy agent updater by running Windows PowerShell commands, using a direct download link, or running an Amazon S3 copy command\.

**Topics**
+ [Use Windows PowerShell](#codedeploy-agent-operations-update-windows-powershell)
+ [Use a direct link](#codedeploy-agent-operations-update-windows-direct-link)
+ [Use an Amazon S3 copy command](#codedeploy-agent-operations-update-windows-s3-copy)

## Use Windows PowerShell<a name="codedeploy-agent-operations-update-windows-powershell"></a>

Sign in to the instance, and run the following commands in Windows PowerShell, one at a time:

```
Set-ExecutionPolicy RemoteSigned
```

 If you are prompted to change the execution policy, choose **Y** so Windows PowerShell requires all scripts and configuration files downloaded from the internet be signed by a trusted publisher\. 

```
Import-Module AWSPowerShell
```

```
New-Item -Path "c:\temp" -ItemType "directory" -Force
```

```
powershell.exe -Command Read-S3Object -BucketName bucket-name -Key latest/codedeploy-agent-updater.msi -File c:\temp\codedeploy-agent-updater.msi
```

```
c:\temp\codedeploy-agent-updater.msi /quiet /l c:\temp\host-agent-updater-log.txt
```

```
powershell.exe -Command Get-Service -Name codedeployagent
```

*bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

If you need to troubleshoot an update process error, type the following command to open the CodeDeploy agent updater log file:

```
notepad C:\ProgramData\Amazon\CodeDeployUpdater\log\codedeploy-agent.updater.log
```

## Use a direct link<a name="codedeploy-agent-operations-update-windows-direct-link"></a>

If the browser security settings on the Windows Server instance provide the required permissions \(for example, to `http://*.s3.amazonaws.com`\), you can use a direct link to download the CodeDeploy agent updater\. Enter the following in your browser's address bar and then download and run the installer manually\.


| Region name | Download link | 
| --- | --- | 
|  US East \(Ohio\)  |  `[https://aws\-codedeploy\-us\-east\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  US East \(N\. Virginia\)  |  `[https://aws\-codedeploy\-us\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  US West \(N\. California\)  |  `[https://aws\-codedeploy\-us\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-us-west-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  US West \(Oregon\)  |  `[https://aws\-codedeploy\-us\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-us-west-2.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Canada \(Central\)  |  `[https://aws\-codedeploy\-ca\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ca-central-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  EU \(Ireland\)  |  `[https://aws\-codedeploy\-eu\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-eu-west-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  EU \(London\)  |  `[https://aws\-codedeploy\-eu\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-eu-west-2.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  EU \(Paris\)  |  `[https://aws\-codedeploy\-eu\-west\-3\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-eu-west-3.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  EU \(Frankfurt\)  |  `[https://aws\-codedeploy\-eu\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-eu-central-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Hong Kong\)  |  `[https://aws\-codedeploy\-ap\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-east-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Tokyo\)  |  `[https://aws\-codedeploy\-ap\-northeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-northeast-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Seoul\)  |  `[https://aws\-codedeploy\-ap\-northeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Singapore\)  |  `[https://aws\-codedeploy\-ap\-southeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-southeast-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Sydney\)  |  `[https://aws\-codedeploy\-ap\-southeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-southeast-2.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  Asia Pacific \(Mumbai\)  |  `[https://aws\-codedeploy\-ap\-south\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 
|  South America \(São Paulo\)  |  `[https://aws\-codedeploy\-sa\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\-updater\.msi](https://aws-codedeploy-sa-east-1.s3.amazonaws.com/latest/codedeploy-agent-updater.msi)`  | 

## Use an Amazon S3 copy command<a name="codedeploy-agent-operations-update-windows-s3-copy"></a>

If the AWS CLI is installed on the instance, you can use the Amazon S3 [cp](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command to download the CodeDeploy agent updater and then run the installer manually\. For information, see [Install the AWS Command Line Interface on Microsoft Windows](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)\. 


| Region name | Amazon S3 copy command | 
| --- | --- | 
|  US East \(Ohio\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-east-2/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  US East \(N\. Virginia\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-east-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  US West \(N\. California\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-west-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  US West \(Oregon\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-west-2/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Canada \(Central\)  |  <pre>aws s3 cp s3://aws-codedeploy-ca-central-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  EU \(Ireland\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  EU \(London\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-2/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  EU \(Paris\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-3/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  EU \(Frankfurt\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-central-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Hong Kong\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-east-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Tokyo\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-northeast-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Seoul\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Singapore\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-southeast-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Sydney\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-southeast-2/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  Asia Pacific \(Mumbai\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-south-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
|  South America \(São Paulo\)  |  <pre>aws s3 cp s3://aws-codedeploy-sa-east-1/latest/codedeploy-agent-updater.msi codedeploy-agent-updater.msi</pre>  | 
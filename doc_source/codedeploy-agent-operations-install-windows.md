# Install or reinstall the AWS CodeDeploy agent for Windows Server<a name="codedeploy-agent-operations-install-windows"></a>

On Windows Server instances, you can use one of these methods to download and install the AWS CodeDeploy agent:

+ Run a series of Windows PowerShell commands

+ Choose a direct download link

+ Run an Amazon S3 copy command

**Note**  
On both new and existing instances, we recommend installing the AWS CodeDeploy agent updater for Windows Server\. The updater checks periodically for new versions of the agent and installs it when a new version is available\. On new instances, you can install the updater instead of the agent, and the current version of the agent will be installed immediately after the updater\. For more information, see [Update the AWS CodeDeploy Agent on Windows Server](codedeploy-agent-operations-update-windows.md)\.


+ [Use Windows PowerShell](#codedeploy-agent-operations-install-windows-powershell)
+ [Use a Direct Link](#codedeploy-agent-operations-install-windows-direct-link)
+ [Use an Amazon S3 Copy Command](#codedeploy-agent-operations-install-windows-s3-copy)

## Use Windows PowerShell<a name="codedeploy-agent-operations-install-windows-powershell"></a>

Sign in to the instance, and run the following commands in Windows PowerShell, one at a time:

```
Set-ExecutionPolicy RemoteSigned
```

```
Import-Module AWSPowerShell
```

```
New-Item –Path "c:\temp" –ItemType "directory" -Force
```

```
powershell.exe -Command Read-S3Object -BucketName bucket-name -Key latest/codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
```

+ 

+ 

```
c:\temp\codedeploy-agent.msi /quiet /l c:\temp\host-agent-install-log.txt
```

*bucket\-name* is the name of the Amazon S3 bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

To check that the service is running, run the following command:

```
powershell.exe -Command Get-Service -Name codedeployagent
```

If the AWS CodeDeploy agent is installed and running, after the Get\-Service command call, you should see output similar to the following:

```
Status   Name                DisplayName
------   ----                -----------
Running  codedeployagent    CodeDeploy Host Agent Service
```

## Use a Direct Link<a name="codedeploy-agent-operations-install-windows-direct-link"></a>

If the browser security settings on the Windows Server instance provide the permissions \(for example, to `http://*.s3.amazonaws.com`\), you can use a direct link for your region to download the AWS CodeDeploy agent and then run the installer manually\.


| Region name | Download link | 
| --- | --- | 
| US East \(Ohio\) | `[https://aws\-codedeploy\-us\-east\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| US East \(N\. Virginia\) | `[https://aws\-codedeploy\-us\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| US West \(N\. California\) | `[https://aws\-codedeploy\-us\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-west-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| US West \(Oregon\) | `[https://aws\-codedeploy\-us\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-west-2.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Canada \(Central\) | `[https://aws\-codedeploy\-ca\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ca-central-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| EU \(Ireland\) | `[https://aws\-codedeploy\-eu\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| EU \(London\) | `[https://aws\-codedeploy\-eu\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-2.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| EU \(Paris\) | `[https://aws\-codedeploy\-eu\-west\-3\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-3.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| EU \(Frankfurt\) | `[https://aws\-codedeploy\-eu\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-central-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Asia Pacific \(Tokyo\) | `[https://aws\-codedeploy\-ap\-northeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-northeast-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Asia Pacific \(Seoul\) | `[https://aws\-codedeploy\-ap\-northeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Asia Pacific \(Singapore\) | `[https://aws\-codedeploy\-ap\-southeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-southeast-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Asia Pacific \(Sydney\) | `[https://aws\-codedeploy\-ap\-southeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-southeast-2.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| Asia Pacific \(Mumbai\) | `[https://aws\-codedeploy\-ap\-south\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 
| South America \(São Paulo\) | `[https://aws\-codedeploy\-sa\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-sa-east-1.s3.amazonaws.com/latest/codedeploy-agent.msi)` | 

## Use an Amazon S3 Copy Command<a name="codedeploy-agent-operations-install-windows-s3-copy"></a>

If the AWS CLI is installed on the instance, you can use the Amazon S3 [cp](http://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command to download the AWS CodeDeploy agent and then run the installer manually\. For information, see [Install the AWS Command Line Interface on Microsoft Windows](http://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)\. 


| Region name | Amazon S3 copy command | 
| --- | --- | 
| US East \(Ohio\) |  

```
aws s3 cp s3://aws-codedeploy-us-east-2/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| US East \(N\. Virginia\) | 

```
aws s3 cp s3://aws-codedeploy-us-east-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| US West \(N\. California\) | 

```
aws s3 cp s3://aws-codedeploy-us-west-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| US West \(Oregon\) | 

```
aws s3 cp s3://aws-codedeploy-us-west-2/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Canada \(Central\) | 

```
aws s3 cp s3://aws-codedeploy-ca-central-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| EU \(Ireland\) | 

```
aws s3 cp s3://aws-codedeploy-eu-west-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| EU \(London\) | 

```
aws s3 cp s3://aws-codedeploy-eu-west-2/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| EU \(Paris\) | 

```
aws s3 cp s3://aws-codedeploy-eu-west-3/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| EU \(Frankfurt\) | 

```
aws s3 cp s3://aws-codedeploy-eu-central-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Asia Pacific \(Tokyo\) | 

```
aws s3 cp s3://aws-codedeploy-ap-northeast-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Asia Pacific \(Seoul\) | 

```
aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Asia Pacific \(Singapore\) | 

```
aws s3 cp s3://aws-codedeploy-ap-southeast-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Asia Pacific \(Sydney\) | 

```
aws s3 cp s3://aws-codedeploy-ap-southeast-2/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| Asia Pacific \(Mumbai\) | 

```
aws s3 cp s3://aws-codedeploy-ap-south-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
| South America \(São Paulo\) | 

```
aws s3 cp s3://aws-codedeploy-sa-east-1/latest/codedeploy-agent.msi codedeploy-agent.msi
``` | 
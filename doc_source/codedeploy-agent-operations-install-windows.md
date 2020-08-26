# Install the CodeDeploy agent for Windows Server<a name="codedeploy-agent-operations-install-windows"></a>

On Windows Server instances, you can use one of these methods to download and install the CodeDeploy agent:
+ Run a series of Windows PowerShell commands\.
+ Choose a direct download link\.
+ Run an Amazon S3 copy command\.

**Note**  
We recommend installing the CodeDeploy agent with AWS Systems Manager to be able to configure scheduled updates of the agent\. For more information, see [Install the CodeDeploy agent using AWS Systems Manager](codedeploy-agent-operations-install-ssm.md)\.

**Topics**
+ [Use Windows PowerShell](#codedeploy-agent-operations-install-windows-powershell)
+ [Use a direct link](#codedeploy-agent-operations-install-windows-direct-link)
+ [Use an Amazon S3 copy command](#codedeploy-agent-operations-install-windows-s3-copy)

## Use Windows PowerShell<a name="codedeploy-agent-operations-install-windows-powershell"></a>

Sign in to the instance, and run the following commands in Windows PowerShell:

1.  Require that all scripts and configuration files downloaded from the Internet be signed by a trusted publisher\. If you are prompted to change the execution policy, type "**Y**\." 

   ```
    Set-ExecutionPolicy RemoteSigned
   ```

1.  Load the AWS Tools for Windows PowerShell\. 

   ```
   Import-Module AWSPowerShell
   ```

1.  Create a directory into where the CodeDeploy agent installation file is downloaded\. 

   ```
   New-Item -Path "c:\temp" -ItemType "directory" -Force
   ```

1.  Download the CodeDeploy agent installation file\. 

   To install the latest version of the CodeDeploy agent:
   + 

     ```
     powershell.exe -Command Read-S3Object -BucketName bucket-name -Key latest/codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
     ```

   To install a specific version of the CodeDeploy agent:
   + 

     ```
     powershell.exe -Command Read-S3Object -BucketName bucket-name -Key releases/codedeploy-agent-###.msi -File c:\temp\codedeploy-agent.msi
     ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource kit bucket names by Region](resource-kit.md#resource-kit-bucket-names)\.

1.  Run the CodeDeploy agent installation file\. 

   ```
   c:\temp\codedeploy-agent.msi /quiet /l c:\temp\host-agent-install-log.txt
   ```

To check that the service is running, run the following command:

```
powershell.exe -Command Get-Service -Name codedeployagent
```

 If the CodeDeploy agent was just installed and has not been started, then after running the Get\-Service command, under **Status**, you should see **Start\.\.\.**:

```
Status     Name                DisplayName
------     ----                -----------
Start...   codedeployagent    CodeDeploy Host Agent Service
```

If the CodeDeploy agent is already running, after running the Get\-Service command, under **Status**, you should see **Running**:

```
Status     Name                DisplayName
------     ----                -----------
Running    codedeployagent    CodeDeploy Host Agent Service
```

## Use a direct link<a name="codedeploy-agent-operations-install-windows-direct-link"></a>

If the browser security settings on the Windows Server instance provide the permissions \(for example, to `http://*.s3.amazonaws.com`\), you can use a direct link for your region to download the CodeDeploy agent and then run the installer manually\.


| Region name | Download link | 
| --- | --- | 
|  US East \(Ohio\)  |  `[https://aws\-codedeploy\-us\-east\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-east-2.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  US East \(N\. Virginia\)  |  `[https://aws\-codedeploy\-us\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  US West \(N\. California\)  |  `[https://aws\-codedeploy\-us\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-west-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  US West \(Oregon\)  |  `[https://aws\-codedeploy\-us\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-us-west-2.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Canada \(Central\)  |  `[https://aws\-codedeploy\-ca\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ca-central-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Europe \(Ireland\)  |  `[https://aws\-codedeploy\-eu\-west\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Europe \(London\)  |  `[https://aws\-codedeploy\-eu\-west\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-2.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Europe \(Paris\)  |  `[https://aws\-codedeploy\-eu\-west\-3\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-west-3.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Europe \(Frankfurt\)  |  `[https://aws\-codedeploy\-eu\-central\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-eu-central-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Hong Kong\)  |  `[https://aws\-codedeploy\-ap\-east\-1\.s3\.ap\-east\-1\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-east-1.s3.ap-east-1.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Tokyo\)  |  `[https://aws\-codedeploy\-ap\-northeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-northeast-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Seoul\)  |  `[https://aws\-codedeploy\-ap\-northeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Singapore\)  |  `[https://aws\-codedeploy\-ap\-southeast\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-southeast-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Sydney\)  |  `[https://aws\-codedeploy\-ap\-southeast\-2\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-southeast-2.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  Asia Pacific \(Mumbai\)  |  `[https://aws\-codedeploy\-ap\-south\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 
|  South America \(São Paulo\)  |  `[https://aws\-codedeploy\-sa\-east\-1\.s3\.amazonaws\.com/latest/codedeploy\-agent\.msi](https://aws-codedeploy-sa-east-1.s3.amazonaws.com/latest/codedeploy-agent.msi)`  | 

## Use an Amazon S3 copy command<a name="codedeploy-agent-operations-install-windows-s3-copy"></a>

If the AWS CLI is installed on the instance, you can use the Amazon S3 [cp](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command to download the CodeDeploy agent and then run the installer manually\. For information, see [Install the AWS Command Line Interface on Microsoft Windows](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)\. 


| Region name | Amazon S3 copy command | 
| --- | --- | 
|  US East \(Ohio\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-east-2/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  US East \(N\. Virginia\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-east-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  US West \(N\. California\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-west-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  US West \(Oregon\)  |  <pre>aws s3 cp s3://aws-codedeploy-us-west-2/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Canada \(Central\)  |  <pre>aws s3 cp s3://aws-codedeploy-ca-central-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Europe \(Ireland\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Europe \(London\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-2/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Europe \(Paris\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-west-3/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Europe \(Frankfurt\)  |  <pre>aws s3 cp s3://aws-codedeploy-eu-central-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Hong Kong\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-east-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Tokyo\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-northeast-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Seoul\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Singapore\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-southeast-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Sydney\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-southeast-2/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  Asia Pacific \(Mumbai\)  |  <pre>aws s3 cp s3://aws-codedeploy-ap-south-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
|  South America \(São Paulo\)  |  <pre>aws s3 cp s3://aws-codedeploy-sa-east-1/latest/codedeploy-agent.msi codedeploy-agent.msi</pre>  | 
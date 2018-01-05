# AppSpec File Structure<a name="reference-appspec-file-structure"></a>

The following is the high\-level structure for an AppSpec file used for deployments to AWS Lambda and EC2/On\-Premises compute platforms\.

## AppSpec File Structure for AWS LambdaDeployments<a name="lambda-appspec-structure"></a>

**Note**  
This structure is written in YAML\. An AppSpec file for a Lambda deployment can also be written in JSON using the same structure\.

```
version: 0.0
resources: 
  lambda-function-specifications
hooks: 
  deployment-lifecycle-event-mappings
```

In this structure:

** **version** **  
This section specifies the version of the AppSpec file\. Do not change this value\. It is required\. Currently, the only allowed value is **0\.0**\. It is reserved by AWS CodeDeploy for future use\.

** **resources** **  
This section specifies information about the Lambda function to deploy\.  
For more information, see [AppSpec 'resources' Section \(AWS Lambda Deployments Only\)](reference-appspec-file-structure-resources.md)\.

** **hooks** **  
This section specifies Lambda fuctions to run at specific deployment lifecycle events to validate the deployment\.  
For more information, see [AppSpec 'hooks' Section](reference-appspec-file-structure-hooks.md)\.

## AppSpec File Structure for EC2/On\-Premises Delployments<a name="server-appspec-structure"></a>

```
version: 0.0
os: operating-system-name
files:
  source-destination-files-mappings
permissions:high
  permissions-specifications
hooks:
  deployment-lifecycle-event-mappings
```

In this structure:

** **version** **  
This section specifies the version of the AppSpec file\. Do not change this value\. It is required\. Currently, the only allowed value is **0\.0**\. It is reserved by AWS CodeDeploy for future use\.

** **os** **  
This section specifies the operating system value of the instance to which you will deploy\. It is required\. The following values can be specified:  

+ **linux** – The instance is an Amazon Linux, Ubuntu Server, or RHEL instance\.

+ **windows** – The instance is a Windows Server instance\.

** **files** **  
This section specifies the names of files that should be copied to the instance during the deployment's **Install** event\.  
For more information, see [AppSpec 'files' Section \(EC2/On\-Premises Deployments Only\)](reference-appspec-file-structure-files.md)\.

** **permissions** **  
This section specifies how special permissions, if any, should be applied to the files in the **files** section as they are being copied over to the instance\. This section applies to Amazon Linux, Ubuntu Server, and Red Hat Enterprise Linux \(RHEL\) instances only\.  
For more information see, [AppSpec 'permissions' Section \(EC2/On\-Premises Deployments Only\)](reference-appspec-file-structure-permissions.md)\.

** **hooks** **  
This section specifies scripts to run at specific deployment lifecycle events during the deployment\.  
For more information, see [AppSpec 'hooks' Section](reference-appspec-file-structure-hooks.md)\.


+ [AppSpec File Structure for AWS LambdaDeployments](#lambda-appspec-structure)
+ [AppSpec File Structure for EC2/On\-Premises Delployments](#server-appspec-structure)
+ [AppSpec 'files' Section \(EC2/On\-Premises Deployments Only\)](reference-appspec-file-structure-files.md)
+ [AppSpec 'resources' Section \(AWS Lambda Deployments Only\)](reference-appspec-file-structure-resources.md)
+ [AppSpec 'permissions' Section \(EC2/On\-Premises Deployments Only\)](reference-appspec-file-structure-permissions.md)
+ [AppSpec 'hooks' Section](reference-appspec-file-structure-hooks.md)
# AppSpec File structure<a name="reference-appspec-file-structure"></a>

The following is the high\-level structure for an AppSpec file used for deployments to AWS Lambda and EC2/On\-Premises compute platforms\.

A value in a YAML\-formatted AppSpec file that is a string must not be wrapped in quotation marks \(""\) unless otherwise specified\.

## AppSpec file structure for Amazon ECS deployments<a name="ecs-appspec-structure"></a>

**Note**  
This AppSpec file is written in YAML, but you can use the same structure to write one in JSON\. A string in a JSON\-formatted AppSpec file is always wrapped in quotation marks \(""\)\.

```
version: 0.0
resources: 
  ecs-service-specifications
hooks: 
  deployment-lifecycle-event-mappings
```

In this structure:

** **version** **  
This section specifies the version of the AppSpec file\. Do not change this value\. It is required\. Currently, the only allowed value is **0\.0**\. It is reserved by CodeDeploy for future use\.  
Specify **version** with a string\.

** **resources** **  
This section specifies information about Amazon ECS application to deploy\.  
For more information, see [ AppSpec 'resources' section for Amazon ECS deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs)\.

** **hooks** **  
This section specifies Lambda functions to run at specific deployment lifecycle event hooks to validate the deployment\.  
For more information, see [List of lifecycle event hooks for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#reference-appspec-file-structure-hooks-list-ecs)\.

## AppSpec file structure for AWS Lambda deployments<a name="lambda-appspec-structure"></a>

**Note**  
This AppSpec file is written in YAML, but you can use the same structure to write an AppSpec file for a Lambda deployment in JSON\. A string in a JSON\-formatted AppSpec file is always wrapped in quotation marks \(""\)\.

```
version: 0.0
resources: 
  lambda-function-specifications
hooks: 
  deployment-lifecycle-event-mappings
```

In this structure:

** **version** **  
This section specifies the version of the AppSpec file\. Do not change this value\. It is required\. Currently, the only allowed value is **0\.0**\. It is reserved by CodeDeploy for future use\.  
Specify **version** with a string\.

** **resources** **  
This section specifies information about the Lambda function to deploy\.  
For more information, see [ AppSpec 'resources' section \(Amazon ECS and AWS Lambda deployments only\) ](reference-appspec-file-structure-resources.md)\.

** **hooks** **  
This section specifies Lambda functions to run at specific deployment lifecycle events to validate the deployment\.  
For more information, see [AppSpec 'hooks' section](reference-appspec-file-structure-hooks.md)\.

## AppSpec file structure for EC2/On\-Premises deployments<a name="server-appspec-structure"></a>

```
version: 0.0
os: operating-system-name
files:
  source-destination-files-mappings
permissions:
  permissions-specifications
hooks:
  deployment-lifecycle-event-mappings
```

In this structure:

** **version** **  
This section specifies the version of the AppSpec file\. Do not change this value\. It is required\. Currently, the only allowed value is **0\.0**\. It is reserved by CodeDeploy for future use\.  
Specify **version** with a string\.

** **os** **  
This section specifies the operating system value of the instance to which you deploy\. It is required\. The following values can be specified:  
+ **linux** – The instance is an Amazon Linux, Ubuntu Server, or RHEL instance\.
+ **windows** – The instance is a Windows Server instance\.
Specify **os** with a string\.

** **files** **  
This section specifies the names of files that should be copied to the instance during the deployment's **Install** event\.  
For more information, see [AppSpec 'files' section \(EC2/On\-Premises deployments only\)](reference-appspec-file-structure-files.md)\.

** **permissions** **  
This section specifies how special permissions, if any, should be applied to the files in the `files` section as they are being copied over to the instance\. This section applies to Amazon Linux, Ubuntu Server, and Red Hat Enterprise Linux \(RHEL\) instances only\.  
For more information see, [AppSpec 'permissions' section \(EC2/On\-Premises deployments only\)](reference-appspec-file-structure-permissions.md)\.

** **hooks** **  
This section specifies scripts to run at specific deployment lifecycle events during the deployment\.  
For more information, see [AppSpec 'hooks' section](reference-appspec-file-structure-hooks.md)\.

**Topics**
+ [AppSpec file structure for Amazon ECS deployments](#ecs-appspec-structure)
+ [AppSpec file structure for AWS Lambda deployments](#lambda-appspec-structure)
+ [AppSpec file structure for EC2/On\-Premises deployments](#server-appspec-structure)
+ [AppSpec 'files' section \(EC2/On\-Premises deployments only\)](reference-appspec-file-structure-files.md)
+ [AppSpec 'resources' section \(Amazon ECS and AWS Lambda deployments only\)](reference-appspec-file-structure-resources.md)
+ [AppSpec 'permissions' section \(EC2/On\-Premises deployments only\)](reference-appspec-file-structure-permissions.md)
+ [AppSpec 'hooks' section](reference-appspec-file-structure-hooks.md)
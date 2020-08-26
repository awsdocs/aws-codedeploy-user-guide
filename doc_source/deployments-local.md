# Use the CodeDeploy agent to validate a deployment package on a local machine<a name="deployments-local"></a>

Using the CodeDeploy agent, you can deploy content on an instance you are logged in to\. This allows you to test the integrity of an application specification file \(AppSpec file\) that you intend to use in a deployment and the content you intend to deploy\. 

You do not need to create an application and deployment group\. If you want to deploy content stored on the local instance, you do not even need an AWS account\. For the simplest testing, you can run the codedeploy\-local command, without specifying any options, in a directory that contains the AppSpec file and the content to be deployed\. There are options for other test cases in the tool\. 

By validating a deployment package on a local machine you can:
+ Test the integrity of an application revision\.
+ Test the contents of an AppSpec file\.
+ Try out CodeDeploy for the first time with your existing application code\.
+ Deploy content rapidly when you are already logged in to an instance\.

You can use deploy content that is stored on the local instance or in a supported remote repository type \(Amazon S3 buckets or public GitHub repositories\)\.

## Prerequisites<a name="deployments-local-prerequisites"></a>

Before you start a local deployment, complete the following steps: 
+ Create or use an instance type supported by the CodeDeploy agent\. For information, see [Operating systems supported by the CodeDeploy agent](codedeploy-agent.md#codedeploy-agent-supported-operating-systems)\. 
+ Install version 1\.0\.1\.1352 or later of the CodeDeploy agent\. For information, see [Install the CodeDeploy agent](codedeploy-agent-operations-install.md)\.
+ If you are deploying your content from an Amazon S3 bucket or GitHub repository, provision an IAM user to use with CodeDeploy\. For information, see [Step 1: Provision an IAM user](getting-started-provision-user.md)\.
+ If you are deploying your application revision from an Amazon S3 bucket, create an Amazon S3 bucket in the region you are working in and apply an Amazon S3 bucket policy to the bucket\. This policy grants your instances the permissions required to download the application revision\.

  For example, the following Amazon S3 bucket policy allows any Amazon EC2 instance with an attached IAM instance profile containing the ARN `arn:aws:iam::80398EXAMPLE:role/CodeDeployDemo` to download from anywhere in the Amazon S3 bucket named `codedeploydemobucket`:

  ```
  {
      "Statement": [
          {
              "Action": [
                  "s3:Get*",
                  "s3:List*"
              ],
              "Effect": "Allow",
              "Resource": "arn:aws:s3:::codedeploydemobucket/*",
              "Principal": {
                  "AWS": [
                      "arn:aws:iam::80398EXAMPLE:role/CodeDeployDemo"
                  ]
              }
          }
      ]
  }
  ```

  The following Amazon S3 bucket policy allows any on\-premises instance with an associated IAM user containing the ARN `arn:aws:iam::80398EXAMPLE:user/CodeDeployUser` to download from anywhere in the Amazon S3 bucket named `codedeploydemobucket`:

  ```
  {
      "Statement": [
          {
              "Action": [
                  "s3:Get*",
                  "s3:List*"
              ],
              "Effect": "Allow",
              "Resource": "arn:aws:s3:::codedeploydemobucket/*",
              "Principal": {
                  "AWS": [
                      "arn:aws:iam::80398EXAMPLE:user/CodeDeployUser"
                  ]
              }
          }
      ]
  }
  ```

  For information about how to generate and attach an Amazon S3 bucket policy, see [Bucket policy examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)\.
+ If you are deploying your application revision from an Amazon S3 bucket or GitHub repository, set up an IAM instance profile and attach it to the instance\. For information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md), [Create an Amazon EC2 instance for CodeDeploy \(AWS CLI or Amazon EC2 console\)](instances-ec2-create.md), and [Create an Amazon EC2 instance for CodeDeploy \(AWS CloudFormation template\)](instances-ec2-create-cloudformation-template.md)\.
+ If you are deploying your content from GitHub, create a GitHub account and a public repository\. To create a GitHub account, see [Join GitHub](https://github.com/join)\. To create a GitHub repository, see [Create a repo](https://help.github.com/articles/create-a-repo/)\.
**Note**  
 Private repositories are not currently supported\. If your content is stored in a private GitHub repository, you can download it to the instance and use the `--bundle-location` option to specify its local path\.
+ Prepare the content \(including an AppSpec file\) that you want to deploy to the instance and place it on the local instance, in your Amazon S3 bucket, or in your GitHub repository\. For information, see [Working with application revisions for CodeDeploy](application-revisions.md)\.
+ If you want to use values other than the defaults for other configuration options, create the configuration file and place it on the instance \(`/etc/codedeploy-agent/conf/codedeployagent.yml` for Amazon Linux, RHEL, or Ubuntu Server instances or `C:\ProgramData\Amazon\CodeDeploy\conf.yml` for Windows Server instances\)\. For information, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\.
**Note**  
If you use a configuration file on Amazon Linux, RHEL, or Ubuntu Server instances, you must either:  
Use the `:root_dir:` and `:log_dir:` variables to specify locations other than the defaults for the deployment root and log directory folders\. 
Use `sudo` to run CodeDeploy agent commands\.

## Create a local deployment<a name="deployments-local-deploy"></a>

On the instance where you want to create the local deployment, open a terminal session \(Amazon Linux, RHEL, or Ubuntu Server instances\) or a command prompt \(Windows Server\) to run the tool commands\.

**Note**  
 The codedeploy\-local command is installed in the following locations:   
 On Amazon Linux, RHEL, or Ubuntu Server: `/opt/codedeploy-agent/bin`\. 
 On Windows Server: `C:\ProgramData\Amazon\CodeDeploy\bin`\. 

** Basic Command Syntax **

```
codedeploy-local [options]
```

**Synopsis**

```
codedeploy-local
[--bundle-location <value>]
[--type <value>]
[--file-exists-behavior <value>]
[--deployment-group <value>]
[--events <comma-separated values>]
[--agent-configuration-file <value>]
```

** Options**

**\-l**, **\-\-bundle\-location**

The location of the application revision bundle\. If you do not specify a location, the tool uses the directory you are currently working in by default\. If you specify a value for `--bundle-location`, you must also specify a value for `--type`\.

Bundle location format examples:
+ Local Amazon Linux, RHEL, or Ubuntu Server instance: `/path/to/local/bundle.tgz`
+ Local Windows Server instance: `C:/path/to/local/bundle`
+ Amazon S3 bucket: `s3://mybucket/bundle.tar`
+ GitHub repository: `https://github.com/account-name/repository-name/`

**\-t**, **\-\-type**

The format of the application revision bundle\. Supported types include `tgz`, `tar`, `zip`, and `directory`\. If you do not specify a type, the tool uses `directory` by default\. If you specify a value for `--type`, you must also specify a value for `--bundle-location`\.

**\-b**, **\-\-file\-exists\-behavior**

Indicates how files are handled that already exist in a deployment target location but weren't part of a previous successful deployment\. Options include DISALLOW, OVERWRITE, RETAIN\. For more information, see [fileExistsBehavior](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_CreateDeployment.html#CodeDeploy-CreateDeployment-request-fileExistsBehavior) in *[AWS CodeDeploy API Reference](https://docs.aws.amazon.com/codedeploy/latest/APIReference/)*\.

**\-g**, **\-\-deployment\-group**

The path to the folder that is the target location for the content to be deployed\. If you do not specify a folder, the tool creates one named *default\-local\-deployment\-group* inside your deployment root directory\. For each local deployment you create, the tool creates a subdirectory inside this folder with names such as *d\-98761234\-local*\.

**\-e**, **\-\-events**

A set of override lifecycle event hooks you want to run, in order, instead of the events you listed in the AppSpec file\. Multiple hooks can be specified, separated by commas\. You can use this option if:
+ You want to run a different set of events without having to update the AppSpec file\. 
+ You want to run a single event hook as an exception to what's in the AppSpec file, such as `ApplicationStop`\.

If you don't specify **DownloadBundle** and **Install** events in the override list, they will run before all the event hooks you do specify\. If you include **DownloadBundle** and **Install** in the list of `--events` options, they must be preceded only by events that normally run before them in CodeDeploy deployments\. For information, see [AppSpec 'hooks' section](reference-appspec-file-structure-hooks.md)\.

**\-c**, **\-\-agent\-configuration\-file**

The location of a configuration file to use for the deployment, if you store it in a location other than the default\. A configuration file specifies alternatives to other default values and behaviors for a deployment\. 

By default, configuration files are stored in `/etc/codedeploy-agent/conf/codedeployagent.yml` \(Amazon Linux, RHEL, or Ubuntu Server instances\) or `C:/ProgramData/Amazon/CodeDeploy/conf.yml` \(Windows Server\)\. For more information, see [CodeDeploy agent configuration reference](reference-agent-configuration.md)\.

**\-h**, **\-\-help**

Displays a summary of help content\.

**\-v**, **\-\-version**

Displays the tool's version number\.

## Examples<a name="deployments-local-examples"></a>

The following are examples of valid command formats\.

```
codedeploy-local
```

```
codedeploy-local --bundle-location /path/to/local/bundle/directory
```

```
codedeploy-local --bundle-location C:/path/to/local/bundle.zip --type zip --deployment-group my-deployment-group
```

```
codedeploy-local --bundle-location /path/to/local/directory --type directory --deployment-group my-deployment-group
```

Deploy a bundle from Amazon S3:

```
codedeploy-local --bundle-location s3://mybucket/bundle.tgz --type tgz
```

```
codedeploy-local --bundle-location s3://mybucket/bundle.zip?versionId=1234&etag=47e8 --type zip --deployment-group my-deployment-group
```

Deploy a bundle from a public GitHub repository:

```
codedeploy-local --bundle-location https://github.com/awslabs/aws-codedeploy-sample-tomcat --type zip
```

```
codedeploy-local --bundle-location https://api.github.com/repos/awslabs/aws-codedeploy-sample-tomcat/zipball/master --type zip
```

```
codedeploy-local --bundle-location https://api.github.com/repos/awslabs/aws-codedeploy-sample-tomcat/zipball/HEAD --type zip
```

```
codedeploy-local --bundle-location https://api.github.com/repos/awslabs/aws-codedeploy-sample-tomcat/zipball/1a2b3c4d --type zip
```

Deploy a bundle specifying multiple lifecycle events:

```
codedeploy-local --bundle-location /path/to/local/bundle.tar --type tar --application-folder my-deployment --events DownloadBundle,Install,ApplicationStart,HealthCheck
```

Stop a previously deployed application using the ApplicationStop lifecycle event:

```
codedeploy-local --bundle-location /path/to/local/bundle.tgz --type tgz --deployment-group --events ApplicationStop
```

Deploy using a specific deployment group ID:

```
codedeploy-local --bundle-location C:/path/to/local/bundle/directory --deployment-group 1234abcd-5dd1-4774-89c6-30b107ac5dca
```

```
codedeploy-local --bundle-location C:/path/to/local/bundle.zip --type zip --deployment-group 1234abcd-5dd1-4774-89c6-30b107ac5dca
```
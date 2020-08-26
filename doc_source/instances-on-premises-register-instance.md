# Use the register command \(IAM user ARN\) to register an on\-premises instance<a name="instances-on-premises-register-instance"></a>

This section describes how to configure an on\-premises instance and register and tag it with CodeDeploy with the least amount of effort\. The register command is most useful when you are working with single or small fleets of on\-premises instances\. You can use the register command only when you are using an IAM user ARN to authenticate an instance\. You cannot use the register command with an IAM session ARN for authentication\.

When you use the register command, you can let CodeDeploy do the following:
+ Create an IAM user in AWS Identity and Access Management for the on\-premises instance, if you do not specify one with the command\.
+ Save the IAM user's credentials to an on\-premises instance configuration file\.
+ Register the on\-premises instance with CodeDeploy\.
+ Add tags to the on\-premises instance, if you specify them as part of the command\.

**Note**  
The [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command is an alternative to the [register](https://docs.aws.amazon.com/cli/latest/reference/deploy/register.html) command\. You use the register\-on\-premises\-instance command if you want to configure an on\-premises instance and register and tag it with CodeDeploy mostly on your own\. The register\-on\-premises\-instance command also gives you the option to use an IAM session ARN to register instances instead of an IAM user ARN\. This approach provides a major advantage if you have large fleets of on\-premises instances\. Specifically, you can use a single IAM session ARN to authenticate multiple instances instead of having to create an IAM user for each on\-premises instance one by one\. For more information, see [Use the register\-on\-premises\-instance command \(IAM user ARN\) to register an on\-premises instance](register-on-premises-instance-iam-user-arn.md) and [Use the register\-on\-premises\-instance command \(IAM Session ARN\) to register an on\-premises instance ](register-on-premises-instance-iam-session-arn.md)\.

**Topics**
+ [Step 1: Install and configure the AWS CLI on the on\-premises instance](#instances-on-premises-register-instance-1-install-cli)
+ [Step 2: Call the register command](#instances-on-premises-register-instance-2-register-command)
+ [Step 3: Call the install command](#instances-on-premises-register-instance-3-install-command)
+ [Step 4: Deploy application revisions to the on\-premises instance](#instances-on-premises-register-instance-4-deploy-revision)
+ [Step 5: Track deployments to the on\-premises instance](#instances-on-premises-register-instance-5-track-deployment)

## Step 1: Install and configure the AWS CLI on the on\-premises instance<a name="instances-on-premises-register-instance-1-install-cli"></a>

1. Install the AWS CLI on the on\-premises instance\. Follow the instructions in [Getting set up with the AWS command line interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.
**Note**  
CodeDeploy commands for working with on\-premises instances are available in AWS CLI version 1\.7\.19 and later\. If you have the AWS CLI already installed, call aws \-\-version to check its version\.

1. Configure the AWS CLI on the on\-premises instance\. Follow the instructions in [Configuring the AWS command line interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in *AWS Command Line Interface User Guide*\.
**Important**  
As you configure the AWS CLI \(for example, by calling the aws configure command\), be sure to specify the secret key ID and secret access key of an IAM user who has, at minimum, the following AWS access permissions in addition to the permissions specified in [Prerequisites for configuring an on\-premises instance](instances-on-premises-prerequisites.md)\. This makes it possible to download and install the CodeDeploy agent on the on\-premises instance\. The access permissions might look similar to this:  

   ```
   {
     "Version": "2012-10-17",
     "Statement" : [
       {
         "Effect" : "Allow",
         "Action" : [
           "codedeploy:*",
           "iam:CreateAccessKey",
           "iam:CreateUser",
           "iam:DeleteAccessKey",
           "iam:DeleteUser",
           "iam:DeleteUserPolicy",
           "iam:ListAccessKeys",
           "iam:ListUserPolicies",
           "iam:PutUserPolicy",
           "iam:GetUser",
           "tag:GetTags",
           "tag:GetResources"
         ],
         "Resource" : "*"
       },
       {
         "Effect" : "Allow",
         "Action" : [
           "s3:Get*",
           "s3:List*"
         ],
         "Resource" : [
           "arn:aws:s3:::aws-codedeploy-us-east-2/*",
           "arn:aws:s3:::aws-codedeploy-us-east-1/*",
           "arn:aws:s3:::aws-codedeploy-us-west-1/*",
           "arn:aws:s3:::aws-codedeploy-us-west-2/*",
           "arn:aws:s3:::aws-codedeploy-ca-central-1/*",
           "arn:aws:s3:::aws-codedeploy-eu-west-1/*",
           "arn:aws:s3:::aws-codedeploy-eu-west-2/*",
           "arn:aws:s3:::aws-codedeploy-eu-west-3/*",
           "arn:aws:s3:::aws-codedeploy-eu-central-1/*",
           "arn:aws:s3:::aws-codedeploy-ap-east-1/*",
           "arn:aws:s3:::aws-codedeploy-ap-northeast-1/*",
           "arn:aws:s3:::aws-codedeploy-ap-northeast-2/*",
           "arn:aws:s3:::aws-codedeploy-ap-southeast-1/*",
           "arn:aws:s3:::aws-codedeploy-ap-southeast-2/*",  
           "arn:aws:s3:::aws-codedeploy-ap-south-1/*",
           "arn:aws:s3:::aws-codedeploy-sa-east-1/*"
         ]
       }     
     ]
   }
   ```

## Step 2: Call the register command<a name="instances-on-premises-register-instance-2-register-command"></a>

For this step, we assume you are registering the on\-premises instance from the on\-premises instance itself\. You can also register an on\-premises instance from a separate device or instance that has the AWS CLI installed and configured as described in the preceding step\.

Use the AWS CLI to call the [register](https://docs.aws.amazon.com/cli/latest/reference/deploy/register.html) command, specifying:
+ A name that uniquely identifies the on\-premises instance to CodeDeploy \(with the `--instance-name` option\)\. 
**Important**  
To help identify the on\-premises instance later, especially for debugging purposes, we strongly recommend that you use a name that maps to some unique characteristic of the on\-premises instance \(for example, the serial number or some unique internal asset identifier, if applicable\)\. If you specify a MAC address for a name, be aware that MAC addresses contain characters that CodeDeploy does not allow, such as colon \(`:`\)\. For a list of allowed characters, see [CodeDeploy limits](reference.md#limits)\.
+ Optionally, the ARN of an existing IAM user that you want to associate with this on\-premises instance \(with the `--iam-user-arn` option\)\. To get the ARN of an IAM user, call the [get\-user](https://docs.aws.amazon.com/cli/latest/reference/iam/get-user.html) command, or choose the IAM user name in the **Users** section of the IAM console and then find the **User ARN** value in the **Summary** section\. If this option is not specified, CodeDeploy will create an IAM user on your behalf in your AWS account and associate it with the on\-premises instance\.
**Important**  
If you specify the `--iam-user-arn` option, you must also manually create the on\-premises instance configuration file, as described in [Step 4: Add a configuration file to the on\-premises instance](register-on-premises-instance-iam-user-arn.md#register-on-premises-instance-iam-user-arn-4)\.  
 You can associate only one IAM user with only one on\-premises instance\. Trying to associate a single IAM user with multiple on\-premises instances can result in errors, failed deployments to those on\-premises instances, or deployments to those on\-premises instances that are stuck in a perpetual pending state\. 
+ Optionally, a set of on\-premises instance tags \(with the `--tags` option\) that CodeDeploy will use to identify the set of Amazon EC2 instances to which to deploy\. Specify each tag with `Key=tag-key,Value=tag-value` \(for example, `Key=Name,Value=Beta Key=Name,Value=WestRegion`\)\. If this option is not specified, no tags will be registered\. To register tags later, call the [add\-tags\-to\-on\-premises\-instances](https://docs.aws.amazon.com/cli/latest/reference/deploy/add-tags-to-on-premises-instances.html) command\.
+ Optionally, the AWS region where the on\-premises instance will be registered with CodeDeploy \(with the `--region` option\)\. This must be one of the supported regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference* \(for example, `us-west-2`\)\. If this option is not specified, the default AWS region associated with the calling IAM user will be used\.

For example:

```
aws deploy register --instance-name AssetTag12010298EX --iam-user-arn arn:aws:iam::80398EXAMPLE:user/CodeDeployUser-OnPrem --tags Key=Name,Value=CodeDeployDemo-OnPrem --region us-west-2
```

The register command does the following:

1. If no existing IAM user is specified, creates an IAM user, attaches the required permissions to it, and generates a corresponding secret key and secret access key\. The on\-premises instance will use this IAM user and its permissions and credentials to authenticate and interact with CodeDeploy\. 

1. Registers the on\-premises instance with CodeDeploy\.

1. If specified, associates in CodeDeploy the tags that are specified with the `--tags` option with the registered on\-premises instance name\. 

1. If an IAM user was created, also creates the required configuration file in the same directory from which the register command was called\.

If this command encounters any errors, an error message appears, describing how you can manually complete the remaining steps\. Otherwise, a success message appears, describing how to call the install command as listed in the next step\.

## Step 3: Call the install command<a name="instances-on-premises-register-instance-3-install-command"></a>

From the on\-premises instance, use the AWS CLI to call the [install](https://docs.aws.amazon.com/cli/latest/reference/deploy/install.html) command, specifying:
+ The path to the configuration file \(with the `--config-file` option\)\.
+ Optionally, whether to replace the configuration file that already exists on the on\-premises instance \(with the `--override-config` option\)\. If not specified, the existing configuration file will not be replaced\.
+ Optionally, the AWS region where the on\-premises instance will be registered with CodeDeploy \(with the `--region` option\)\. This must be one of the supported regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference* \(for example, `us-west-2`\)\. If this option is not specified, the default AWS region associated with the calling IAM user will be used\.
+ Optionally, a custom location from which to install the CodeDeploy agent \(with the `--agent-installer` option\)\. This option is useful for installing a custom version of the CodeDeploy agent that CodeDeploy does not officially support \(such as a custom version based on the [CodeDeploy agent](https://github.com/aws/aws-codedeploy-agent) repository in GitHub\)\. The value must be the path to an Amazon S3 bucket that contains either: 
  + A CodeDeploy agent installation script \(for Linux\- or Unix\-based operating systems, similar to the install file in the [CodeDeploy agent](https://github.com/aws/aws-codedeploy-agent/blob/master/bin/install) repository in GitHub\)\.
  + A CodeDeploy agent installer package \(\.msi\) file \(for Windows\-based operating systems\)\.

   If this option is not specified, CodeDeploy will make its best attempt to install from its own location an officially supported version of the CodeDeploy agent that is compatible with the operating system on the on\-premises instance\.

For example:

```
aws deploy install --override-config --config-file /tmp/codedeploy.onpremises.yml --region us-west-2 --agent-installer s3://aws-codedeploy-us-west-2/latest/codedeploy-agent.msi
```

The install command does the following:

1. Checks whether the on\-premises instance is an Amazon EC2 instance\. If it is, an error message appears\.

1. Copies the on\-premises instances configuration file from the specified location on the instance to the location where the CodeDeploy agent expects to find it, provided that the file is not already in that location\.

   For Ubuntu Server and Red Hat Enterprise Linux \(RHEL\)\), this is `/etc/codedeploy-agent/conf`/`codedeploy.onpremises.yml`\.

   For Windows Server, this is `C:\ProgramData\Amazon\CodeDeploy`\\`conf.onpremises.yml`\.

   If the `--override-config` option was specified, creates or overwrites the file\.

1. Installs the CodeDeploy agent on the on\-premises instance and then starts it\. 

## Step 4: Deploy application revisions to the on\-premises instance<a name="instances-on-premises-register-instance-4-deploy-revision"></a>

You are now ready to deploy application revisions to the registered and tagged on\-premises instance\. 

You deploy application revisions to on\-premises instances in a way that's similar to deploying application revisions to Amazon EC2 instances\. For instructions, see [Create a deployment with CodeDeploy](deployments-create.md)\. These instructions link to prerequisites, including creating an application, creating a deployment group, and preparing an application revision\. If you need a simple sample application revision to deploy, you can create the one described in [Step 2: Create a sample application revision](tutorials-on-premises-instance-2-create-sample-revision.md) in the [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

**Important**  
If you reuse an existing CodeDeploy service role as part of creating a deployment group that targets on\-premises instances, you must include `Tag:get*` to the `Action` portion of the service role's policy statement\. For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

## Step 5: Track deployments to the on\-premises instance<a name="instances-on-premises-register-instance-5-track-deployment"></a>

After you deploy an application revision to registered and tagged on\-premises instances, you can track the deployment's progress\.

You track deployments to on\-premises instances in a way that's similar to tracking deployments to Amazon EC2 instances\. For instructions, see [View CodeDeploydeployment details ](deployments-view-details.md)\.

For more options, see [Managing on\-premises instances operations in CodeDeploy](on-premises-instances-operations.md)\.
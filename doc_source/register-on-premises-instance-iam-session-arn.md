# Use the register\-on\-premises\-instance command \(IAM Session ARN\) to register an on\-premises instance<a name="register-on-premises-instance-iam-session-arn"></a>

For maximum control over the authentication and registration of your on\-premises instances, you can use the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command and periodically refreshed temporary credentials generated with the AWS Security Token Service \(AWS STS\)\. A static IAM role for the instance assumes the role of these refreshed AWS STS credentials to perform CodeDeploy deployment operations\. 

This method is most useful when you need to register a large number of instances\. It allows you to automate the registration process with CodeDeploy\. You can use your own identity and authentication system to authenticate on\-premises instances and distribute IAM session credentials from the service to the instances for use with CodeDeploy\. 

**Note**  
Alternatively, you can use a shared IAM user distributed to all on\-premises instances to call the AWS STS [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API to retrieve session credentials for on\-premises instances\. This method is less secure and not recommended for production or mission\-critical environments\.

Use the information in the following topics to configure an on\-premises instance using temporary security credentials generated with AWS STS\.

**Topics**
+ [IAM session ARN registration prerequisites](#register-on-premises-instance-iam-session-arn-prerequisites)
+ [Step 1: Create the IAM role that on\-premises instances will assume](#register-on-premises-instance-iam-session-arn-1)
+ [Step 2: Generate temporary credentials for an individual instance using AWS STS](#register-on-premises-instance-iam-session-arn-2)
+ [Step 3: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-session-arn-3)
+ [Step 4: Prepare an on\-premises instance for CodeDeploy deployments](#register-on-premises-instance-iam-session-arn-4)
+ [Step 5: Register the on\-premises instance with CodeDeploy](#register-on-premises-instance-iam-session-arn-5)
+ [Step 6: Tag the on\-premises instance](#register-on-premises-instance-iam-session-arn-6)
+ [Step 7: Deploy application revisions to the on\-premises instance](#register-on-premises-instance-iam-session-arn-7)
+ [Step 8: Track deployments to the on\-premises instance](#register-on-premises-instance-iam-session-arn-8)

## IAM session ARN registration prerequisites<a name="register-on-premises-instance-iam-session-arn-prerequisites"></a>

In addition to the prerequisites listed in [Prerequisites for configuring an on\-premises instance](instances-on-premises-prerequisites.md), the following additional requirements must be met:

**IAM permissions**

The IAM identity you use to register an on\-premises instance must be granted permissions to perform CodeDeploy operations\. Make sure the **AWSCodeDeployFullAccess** managed policy is attached to the IAM identity\. For information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

**System to refresh temporary credentials**

If you use an IAM session ARN to register on\-premises instances, you must have a system in place to periodically refresh the temporary credentials\. Temporary credentials expire after one hour or sooner if a shorter period is specified when the credentials are generated\. There are two methods for refreshing the credentials:
+ **Method 1**: Use the identity and authentication system in place in your corporate network with a CRON script that periodically polls the identity and authentication system and copies the latest session credentials to the instance\. This enables you to integrate your authentication and identity structure with AWS without needing to make changes to the CodeDeploy agent or service to support authentication types you use in your organization\.
+ **Method 2**: Periodically run a CRON job on the instance to call the AWS STS [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) action and write the session credentials to a file that the CodeDeploy agent can access\. This method still requires using an IAM user and copying credentials to the on\-premises instance, but you can re\-use the same IAM user and credentials across your fleet of on\-premises instances\. 

For information about creating and working with AWS STS credentials, see [AWS Security Token Service API Reference](https://docs.aws.amazon.com/STS/latest/APIReference/) and [Using temporary security credentials to request access to AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html)\.

## Step 1: Create the IAM role that on\-premises instances will assume<a name="register-on-premises-instance-iam-session-arn-1"></a>

You can use the AWS CLI or the IAM console to create an IAM role that will be used by your on\-premises instances to authenticate and interact with CodeDeploy\. 

You only need to create a single IAM role\. Each one of your on\-premises instances can assume this role to retrieve the temporary security credentials that provide the permissions granted to this role\. 

The role you create will require the following permissions to access the files required to install the CodeDeploy agent: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

We recommend that you restrict this policy to only those Amazon S3 buckets your on\-premises instance needs to access\. If you restrict this policy, make sure to give access to the Amazon S3 buckets that contain the CodeDeploy agent\. Otherwise, an error might occur whenever the CodeDeploy agent is installed or updated on the on\-premises instance\. For information about controlling access to Amazon S3 buckets, see [Managing access permissions to your Amazon S3 resources](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html)\.

**To create the IAM role**

1. Call the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command using the `--role-name` option to specify a name for the IAM role \(for example, `CodeDeployInstanceRole`\) and the `--assume-role-policy-document` option to provide the permissions\.

   When you create the IAM role for this instance, you might give it the name `CodeDeployInstanceRole` and include the required permissions in a file named `CodeDeployRolePolicy.json`:

   ```
   aws iam create-role --role-name CodeDeployInstanceRole --assume-role-policy-document file://CodeDeployRolePolicy.json
   ```

1. In the output of the call to the create\-role command, note the value of the ARN field\. For example:

   ```
   arn:aws:iam::123456789012:role/CodeDeployInstanceRole
   ```

   You will need the role ARN when you use the AWS STS [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API to generate short\-term credentials for each instance\.

   For more information about creating IAM roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in *IAM User Guide*\.

   For information about assigning permissions to an existing role, see [put\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html) in [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

## Step 2: Generate temporary credentials for an individual instance using AWS STS<a name="register-on-premises-instance-iam-session-arn-2"></a>

Before you generate the temporary credentials that will be used for registering an on\-premises instance, you must create or choose the IAM identity \(user or role\) that you will generate the temporary credentials for\. The `sts:AssumeRole` permission must be included in the policy settings for this IAM identity\.

For information about granting `sts:AssumeRole` permissions to an IAM identity, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) and [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)\.

There are two ways to generate the temporary credentials:
+ Use the [assume\-role](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) command with the AWS CLI\. For example:

  ```
  aws sts assume-role --role-arn arn:aws:iam::12345ACCOUNT:role/role-arn --role-session-name session-name
  ```

  Where:
  + *12345ACCOUNT* is the 12\-digit account number for your organization\.
  + *role\-arn* is the ARN of the role to be assumed, which you generated in [Step 1: Create the IAM role that on\-premises instances will assume](#register-on-premises-instance-iam-session-arn-1)\.
  + *session\-name* is the name you want to give to the role session you are creating now\.
**Note**  
If you use a CRON script that periodically polls the identity and authentication system and copies the latest session credentials to the instance \(method 1 for refreshing temporary credentials described in [IAM session ARN registration prerequisites](#register-on-premises-instance-iam-session-arn-prerequisites)\), you can instead use any supported AWS SDK to call [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)\.
+ Use a tool provided by AWS\.

  The aws\-codedeploy\-session\-helper tool generates AWS STS credentials and writes them to a file you place on the instance\. This tool is best suited to method 2 for refreshing temporary credentials described in [IAM session ARN registration prerequisites](#register-on-premises-instance-iam-session-arn-prerequisites)\. In this method, the aws\-codedeploy\-session\-helper tool is placed on each instance and executes the command using an IAM user’s permissions\. Each instance uses the same IAM user’s credentials in conjunction with this tool\.

  For more information, see the [aws\-codedeploy\-session\-helper](https://github.com/awslabs/aws-codedeploy-samples/tree/master/utilities/aws-codedeploy-session-helper) GitHub repository\.
**Note**  
After you have created the IAM session credentials, place them in any location on the on\-premises instance\. In the next step, you will configure the CodeDeploy agent to access the credentials in this location\.

Before continuing, make sure the system you will use to periodically refresh the temporary credentials is in place\. If the temporary credentials are not refreshed, deployments to the on\-premises instance will fail\. For more information, see "System to refresh temporary credentials" in [IAM session ARN registration prerequisites](#register-on-premises-instance-iam-session-arn-prerequisites)\.

## Step 3: Add a configuration file to the on\-premises instance<a name="register-on-premises-instance-iam-session-arn-3"></a>

Add a configuration file to the on\-premises instance, using root or administrator permissions\. This configuration file is used to declare the IAM credentials and the target AWS region to be used for CodeDeploy\. The file must be added to a specific location on the on\-premises instance\. The file must include the IAM temporary session ARN, its secret key ID and secret access key, and the target AWS region\. 

**To add a configuration file**

1. Create a file named `codedeploy.onpremises.yml` \(for an Ubuntu Server or RHEL on\-premises instance\) or `conf.onpremises.yml` \(for a Windows Server on\-premises instance\) in the following location on the on\-premises instance:
   + For Ubuntu Server: `/etc/codedeploy-agent/conf`
   + For Windows Server: `C:\ProgramData\Amazon\CodeDeploy`

1. Use a text editor to add the following information to the newly created `codedeploy.onpremises.yml` or `conf.onpremises.yml` file: 

   ```
   ---
   iam_session_arn: iam-session-arn
   aws_credentials_file: credentials-file
   region: supported-region
   ```

   Where:
   + *iam\-session\-arn* is the IAM session ARN you noted in [Step 2: Generate temporary credentials for an individual instance using AWS STS](#register-on-premises-instance-iam-session-arn-2)\. 
   + *credentials\-file* is the location of the credentials file for the temporary session ARN, as noted in [Step 2: Generate temporary credentials for an individual instance using AWS STS](#register-on-premises-instance-iam-session-arn-2)\.
   + *supported\-region* is one of the regions that CodeDeploy supports, as listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\.

## Step 4: Prepare an on\-premises instance for CodeDeploy deployments<a name="register-on-premises-instance-iam-session-arn-4"></a>

**Install and configure the AWS CLI **

Install and configure the AWS CLI on the on\-premises instance\. \(The AWS CLI will be used to download and install the CodeDeploy agent on the on\-premises instance\.\) 

1. To install the AWS CLI on the on\-premises instance, follow the instructions in [Getting set up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.
**Note**  
CodeDeploy commands for working with on\-premises instances became available in version 1\.7\.19 of the AWS CLI\. If you have a version of the AWS CLI already installed, you can check its version by calling aws \-\-version\.

1. To configure the AWS CLI on the on\-premises instance, follow the instructions in [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.
**Important**  
As you configure the AWS CLI \(for example, by calling the aws configure command\), be sure to specify the secret key ID and secret access key of an IAM user that has, at minimum, the permissions described in [IAM session ARN registration prerequisites](#register-on-premises-instance-iam-session-arn-prerequisites)\.

**Set the AWS\_REGION Environment Variable \(Ubuntu Server and RHEL Only\)**

If you are not running Ubuntu Server or RHEL on your on\-premises instance, skip this step and go directly to "Install the CodeDeploy agent \." 

Install the CodeDeploy agent on an Ubuntu Server or RHEL on\-premises instance and enable instance to update the CodeDeploy agent whenever a new version becomes available\. You do this by setting the `AWS_REGION` environment variable on the instance to the identifier of one of the regions supported by CodeDeploy\. We recommend that you set the value to the region where your CodeDeploy applications, deployment groups, and application revisions are located \(for example, `us-west-2`\)\. For a list of regions, see [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\.

To set the environment variable, call the following from the terminal:

```
export AWS_REGION=supported-region
```

Where *supported\-region* is the region identifier \(for example, `us-west-2`\)\.

**Install the CodeDeploy agent**
+ For an Ubuntu Server on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Ubuntu Server](codedeploy-agent-operations-install-ubuntu.md), and then return to this page\.
+ For a RHEL on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Amazon Linux or RHEL](codedeploy-agent-operations-install-linux.md), and then return to this page\.
+ For a Windows Server on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Windows Server](codedeploy-agent-operations-install-windows.md), and then return to this page\.

## Step 5: Register the on\-premises instance with CodeDeploy<a name="register-on-premises-instance-iam-session-arn-5"></a>

The instructions in this step assume you are registering the on\-premises instance from the on\-premises instance itself\. You can register an on\-premises instance from a separate device or instance that has the AWS CLI installed and configured\.

Use the AWS CLI to register the on\-premises instance with CodeDeploy so that it can be used in deployments\.

Before you can use the AWS CLI, you will need the ARN of the temporary session credentials you created in [Step 3: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-session-arn-3)\. For example, for an instance you identify as `AssetTag12010298EX`:

```
arn:sts:iam::123456789012:assumed-role/CodeDeployInstanceRole/AssetTag12010298EX
```

Call the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command, specifying:
+  A name that uniquely identifies the on\-premises instance \(with the `--instance-name` option\)\.
**Important**  
To help identify the on\-premises instance, especially for debugging purposes, we strongly recommend that you specify a name that maps to some unique characteristic of the on\-premises instance \(for example, the session\-name of the STS credentials and the serial number or an internal asset identifier, if applicable\)\. If you specify a MAC address as a name, be aware that MAC addresses contain characters that CodeDeploy does not allow, such as colon \(:\)\. For a list of allowed characters, see [CodeDeploy limits](reference.md#limits)\.
+ The IAM session ARN that you set up to authenticate multiple on\-premises instances in [Step 1: Create the IAM role that on\-premises instances will assume](#register-on-premises-instance-iam-session-arn-1)\.

For example:

```
aws deploy register-on-premises-instance --instance-name name-of-instance --iam-session-arn arn:aws:sts::account-id:assumed-role/role-to-assume/session-name
```

Where:
+ *name\-of\-instance* is the name you use to identify the on\-premises instance, such as `AssetTag12010298EX`\.
+ *account\-id* is the 12\-digit account ID for your organization, such as `111222333444`\.
+ *role\-to\-assume* is the name of the IAM role you created for the instance, such as `CodeDeployInstanceRole`\.
+ *session\-name* is the name of the session role you specified in [Step 2: Generate temporary credentials for an individual instance using AWS STS](#register-on-premises-instance-iam-session-arn-2)\.

## Step 6: Tag the on\-premises instance<a name="register-on-premises-instance-iam-session-arn-6"></a>

You can use either the AWS CLI or the CodeDeploy console to tag the on\-premises instance\. \(CodeDeploy uses on\-premises instance tags to identify the deployment targets during a deployment\.\)

**To tag the on\-premises instance \(CLI\)**
+ Call the [add\-tags\-to\-on\-premises\-instances](https://docs.aws.amazon.com/cli/latest/reference/deploy/add-tags-to-on-premises-instances.html) command, specifying:
  + The name that uniquely identifies the on\-premises instance \(with the `--instance-names` option\)\. 
  + The name of the on\-premises instance tag key and tag value you want to use \(with the `--tags` option\)\. You must specify both a name and value\. CodeDeploy does not allow on\-premises instance tags that have values only\.

    For example:

    ```
    aws deploy add-tags-to-on-premises-instances --instance-names AssetTag12010298EX --tags Key=Name,Value=CodeDeployDemo-OnPrem
    ```

**To tag the on\-premises instance \(console\)**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and choose **On\-premises instances**\.

1. In the list of on\-premises instances, choose name of the on\-premises instance you want to tag\.

1. In the list of tags, select or enter the desired tag key and tag value\. After you enter the tag key and tag value, another row appears\. You can repeat this for up to 10 tags\. To remove a tag, choose **Remove**\.

1. After you have added tags, choose **Update Tags**\.

## Step 7: Deploy application revisions to the on\-premises instance<a name="register-on-premises-instance-iam-session-arn-7"></a>

You are now ready to deploy application revisions to the registered and tagged on\-premises instance\. 

You deploy application revisions to on\-premises instances in a way that's similar to deploying application revisions to Amazon EC2 instances\. For instructions, see [Create a deployment with CodeDeploy](deployments-create.md)\. These instructions include a link to prerequisites, including creating an application, creating a deployment group, and preparing an application revision\. If you need a simple sample application revision to deploy, you can create the one described in [Step 2: Create a sample application revision](tutorials-on-premises-instance-2-create-sample-revision.md) in the [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

**Important**  
If you reuse a CodeDeploy service role as part of creating a deployment group that targets on\-premises instances, you must include `Tag:get*` to the `Action` portion of the service role's policy statement\. For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

## Step 8: Track deployments to the on\-premises instance<a name="register-on-premises-instance-iam-session-arn-8"></a>

After you deploy an application revision to registered and tagged on\-premises instances, you can track the deployment's progress\.

You track deployments to on\-premises instances in a way that's similar to tracking deployments to Amazon EC2 instances\. For instructions, see [View CodeDeploydeployment details ](deployments-view-details.md)\.
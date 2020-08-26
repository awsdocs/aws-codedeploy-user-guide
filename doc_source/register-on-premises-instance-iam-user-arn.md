# Use the register\-on\-premises\-instance command \(IAM user ARN\) to register an on\-premises instance<a name="register-on-premises-instance-iam-user-arn"></a>

Follow these instructions to configure an on\-premises instance and register and tag it with CodeDeploy mostly on your own, using static IAM user credentials for authentication\.

**Topics**
+ [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)
+ [Step 2: Assign permissions to the IAM user](#register-on-premises-instance-iam-user-arn-2)
+ [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)
+ [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4)
+ [Step 5: Install and configure the AWS CLI](#register-on-premises-instance-iam-user-arn-5)
+ [Step 6: Set the AWS\_REGION environment variable \(Ubuntu Server and RHEL only\)](#register-on-premises-instance-iam-user-arn-6)
+ [Step 7: Install the CodeDeploy agent](#register-on-premises-instance-iam-user-arn-7)
+ [Step 8: Register the on\-premises instance with CodeDeploy](#register-on-premises-instance-iam-user-arn-8)
+ [Step 9: Tag the on\-premises instance](#register-on-premises-instance-iam-user-arn-9)
+ [Step 10: Deploy application revisions to the on\-premises instance](#register-on-premises-instance-iam-user-arn-10)
+ [Step 11: Track deployments to the on\-premises instance](#register-on-premises-instance-iam-user-arn-11)

## Step 1: Create an IAM user for the on\-premises instance<a name="register-on-premises-instance-iam-user-arn-1"></a>

Create an IAM user that the on\-premises instance will use to authenticate and interact with CodeDeploy\. 

**Important**  
You must create a separate IAM user for each participating on\-premises instance\. If you try to reuse an individual IAM user for multiple on\-premises instances, you might not be able to successfully register or tag those on\-premises instances with CodeDeploy\. Deployments to those on\-premises instances might be stuck in a perpetual pending state or fail altogether\.

We recommed that you assign the IAM user a name that identifies its purpose, such as CodeDeployUser\-OnPrem\.

You can use the AWS CLI or the IAM console to create an IAM user\. For information, see [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\. 

**Important**  
Whether you use the AWS CLI or the IAM console to create a new IAM user, make a note of the user ARN provided for the user\. You will need this information later in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4) and [Step 8: Register the on\-premises instance with CodeDeploy](#register-on-premises-instance-iam-user-arn-8)\.

## Step 2: Assign permissions to the IAM user<a name="register-on-premises-instance-iam-user-arn-2"></a>

If your on\-premises instance will be deploying application revisions from Amazon S3 buckets, you must assign to the IAM user the permissions to interact with those buckets\. You can use the AWS CLI or the IAM console to assign permissions\.

**Note**  
If you will be deploying application revisions only from GitHub repositories, skip this step and go directly to [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)\. \(You will still need information about the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)\. It will be used in later steps\.\)

**To assign permissions \(CLI\)**

1. Create a file with the following policy contents on the Amazon EC2 instance or device you are using to call the AWS CLI\. Name the file something like **CodeDeploy\-OnPrem\-Permissions\.json**, and then save the file\.

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
**Note**  
We recommend that you restrict this policy to only those Amazon S3 buckets your on\-premises instance needs to access\. If you restrict this policy, make sure to also give access to the Amazon S3 buckets that contain the AWS CodeDeploy agent\. Otherwise, an error might occur whenever the CodeDeploy agent is installed or updated on the associated on\-premises instance\.  
For example:  

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:Get*",
           "s3:List*"
         ],
         "Resource": [
           "arn:aws:s3:::replace-with-your-s3-bucket-name/*",
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

1. Call the [put\-user\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/put-user-policy.html) command, specifying the name of the IAM user \(with the `--user-name` option\), a name for the policy \(with the `--policy-name` option\), and the path to the newly created policy document \(with the `--policy-document` option\)\. For example, assuming that the **CodeDeploy\-OnPrem\-Permissions\.json** file is in the same directory \(folder\) from which you're calling this command:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws iam put-user-policy --user-name CodeDeployUser-OnPrem --policy-name CodeDeploy-OnPrem-Permissions --policy-document file://CodeDeploy-OnPrem-Permissions.json
   ```

**To assign permissions \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create Policy**\. \(If a **Get Started** button appears, choose it, and then choose **Create Policy**\.\)

1. Next to **Create Your Own Policy**, choose **Select**\.

1. In the **Policy Name** box, type a name for this policy \(for example, **CodeDeploy\-OnPrem\-Permissions**\)\.

1. In the **Policy Document** box, type or paste the following permissions expression, which allows AWS CodeDeploy to deploy application revisions from any Amazon S3 bucket specified in the policy to the on\-premises instance on behalf of the IAM user account:

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

1. Choose **Create Policy**\.

1. In the navigation pane, choose **Users**\.

1. In the list of users, browse to and choose the name of the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)\. 

1. On the **Permissions** tab, in **Managed Policies**, choose **Attach Policy**\.

1. Select the policy named **CodeDeploy\-OnPrem\-Permissions**, and then choose **Attach Policy**\. 

## Step 3: Get the IAM user credentials<a name="register-on-premises-instance-iam-user-arn-3"></a>

Get the secret key ID and the secret access key for the IAM user\. You will need them for [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4)\. You can use the AWS CLI or the IAM console to get the secret key ID and the secret access key\.

**Note**  
If you already have the secret key ID and the secret access key, skip this step and go directly to [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4)\.

**To get the credentials \(CLI\)**

1. Call the [list\-access\-keys](https://docs.aws.amazon.com/cli/latest/reference/iam/list-access-keys.html) command, specifying the name of the IAM user \(with the `--user-name` option\) and querying for just the access key IDs \(with the `--query` and `--output` options\)\. For example:

   ```
   aws iam list-access-keys --user-name CodeDeployUser-OnPrem --query "AccessKeyMetadata[*].AccessKeyId" --output text
   ```

1. If no keys appear in the output or information about only one key appears in the output, call the [create\-access\-key](https://docs.aws.amazon.com/cli/latest/reference/iam/create-access-key.html) command, specifying the name of the IAM user \(with the `--user-name` option\):

   ```
   aws iam create-access-key --user-name CodeDeployUser-OnPrem
   ```

   In the output of the call to the create\-access\-key command, note the value of the `AccessKeyId` and `SecretAccessKey` fields\. You will need this information in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4)\.
**Important**  
This will be the only time you will have access to this secret access key\. If you forget or lose access to this secret access key, you will need to generate a new one by following the steps in [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)\.

1. If two access keys are already listed, you must delete one of them by calling the [delete\-access\-key](https://docs.aws.amazon.com/cli/latest/reference/iam/delete-access-key.html) command, specifying the name of the IAM user \(with the `--user-name` option\), and the ID of the access key to delete \(with the `--access-key-id` option\)\. Then call the create\-access\-key command, as described earlier in this step\. Here's an example of calling the delete\-access\-key command:

   ```
   aws iam delete-access-key --user-name CodeDeployUser-OnPrem --access-key-id access-key-ID
   ```
**Important**  
If you call the delete\-access\-key command to delete one of these access keys, and an on\-premises instance is already using this access key as described in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4), you will need to follow the instructions in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4) again to specify a different access key ID and secret access key associated with this IAM user\. Otherwise, any deployments to that on\-premises instance might be stuck in a perpetual pending state or fail altogether\.

**To get the credentials \(console\)**

1. 

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. If the list of users is not displayed, in the navigation pane, choose **Users**\.

   1. In the list of users, browse to and choose the name of the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)\. 

1. On the **Security credentials** tab, if no keys or only one key is listed, choose **Create access key**\.

   If two access keys are listed, you must delete one of them\. Choose **Delete** next to one of the access keys, and then choose **Create access key**\.
**Important**  
If you choose **Delete** next to one of these access keys, and an on\-premises instance is already using this access key as described in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4), you will need to follow the instructions in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4) again to specify a different access key ID and secret access key associated with this IAM user\. Otherwise, deployments to that on\-premises instance might be stuck in a perpetual pending state or fail altogether\.

1. Choose **Show** and note the access key ID and secret access key\. You will need this information for the next step\. Alternatively, you can choose **Download \.csv file** to save a copy of the access key ID and the secret access key\.
**Important**  
Unless you make a note of or download the credentials, this will be the only time you will have access to this secret access key\. If you forget or lose access to this secret access key, you will need to generate a new one by following the steps in [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)\.

1. Choose **Close** to return to the **Users > *IAM User Name*** page\.

## Step 4: Add a configuration file to the on\-premises instance<a name="register-on-premises-instance-iam-user-arn-4"></a>

Add a configuration file to the on\-premises instance, using root or administrator permissions\. This configuration file will be used to declare the IAM user credentials and the target AWS region to be used for CodeDeploy\. The file must be added to a specific location on the on\-premises instance\. The file must include the IAM user's ARN, secret key ID, secret access key, and the target AWS region\. The file must follow a specific format\.

1. Create a file named `codedeploy.onpremises.yml` \(for an Ubuntu Server or RHEL on\-premises instance\) or `conf.onpremises.yml` \(for a Windows Server on\-premises instance\) in the following location on the on\-premises instance:
   + For Ubuntu Server: `/etc/codedeploy-agent/conf`
   + For Windows Server: `C:\ProgramData\Amazon\CodeDeploy`

1. Use a text editor to add the following information to the newly created `codedeploy.onpremises.yml` or `conf.onpremises.yml` file:

   ```
   ---
   aws_access_key_id: secret-key-id
   aws_secret_access_key: secret-access-key
   iam_user_arn: iam-user-arn
   region: supported-region
   ```

   Where:
   + *secret\-key\-id* is the corresponding IAM user's secret key ID you noted in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1) or [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)\.
   + *secret\-access\-key* is the corresponding IAM user's secret access key you noted in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1) or [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3)\.
   + *iam\-user\-arn* is the IAM user's ARN you noted earlier in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)\. 
   + *supported\-region* is the identifier of a region supported by CodeDeploy where your CodeDeploy applications, deployment groups, and application revisions are located \(for example, `us-west-2`\)\. For a list of regions, see [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\.
**Important**  
If you chose **Delete** next to one of the access keys in [Step 3: Get the IAM user credentials](#register-on-premises-instance-iam-user-arn-3), and your on\-premises instance is already using the associated access key ID and secret access key, you will need to follow the instructions in [Step 4: Add a configuration file to the on\-premises instance](#register-on-premises-instance-iam-user-arn-4) to specify a different access key ID and secret access key associated with this IAM user\. Otherwise, any deployments to your on\-premises instance might be stuck in a perpetual pending state or fail altogether\.

## Step 5: Install and configure the AWS CLI<a name="register-on-premises-instance-iam-user-arn-5"></a>

Install and configure the AWS CLI on the on\-premises instance\. \(The AWS CLI will be used in [Step 7: Install the CodeDeploy agent ](#register-on-premises-instance-iam-user-arn-7) to download and install the CodeDeploy agent on the on\-premises instance\.\)

1. To install the AWS CLI on the on\-premises instance, follow the instructions in [Getting set up with the AWS command line interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.
**Note**  
CodeDeploy commands for working with on\-premises instances became available in version 1\.7\.19 of the AWS CLI\. If you have a version of the AWS CLI already installed, you can check its version by calling aws \-\-version\.

1. To configure the AWS CLI on the on\-premises instance, follow the instructions in [Configuring the AWS command line interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.
**Important**  
As you configure the AWS CLI \(for example, by calling the aws configure command\), be sure to specify the secret key ID and secret access key of an IAM user that has, at minimum, the following AWS access permissions in addition to the access permissions specified in the [Prerequisites for configuring an on\-premises instance](instances-on-premises-prerequisites.md)\. This makes it possible for you to download and install the CodeDeploy agent on the on\-premises instance:  

   ```
   {
     "Version": "2012-10-17",
     "Statement" : [
       {
         "Effect" : "Allow",
         "Action" : [
           "codedeploy:*"
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
These access permissions can be assigned to either the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1) or to a different IAM user\. To assign these permissions to an IAM user, follow the instructions in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1), using these access permissions instead of the ones in that step\.

## Step 6: Set the AWS\_REGION environment variable \(Ubuntu Server and RHEL only\)<a name="register-on-premises-instance-iam-user-arn-6"></a>

If you are not running Ubuntu Server or RHEL on your on\-premises instance, skip this step and go directly to [Step 7: Install the CodeDeploy agent ](#register-on-premises-instance-iam-user-arn-7)\. 

Install the CodeDeploy agent on an Ubuntu Server or RHEL on\-premises instance and enable the instance to update the CodeDeploy agent whenever a new version becomes available\. You do this by setting the `AWS_REGION` environment variable on the instance to the identifier of one of the regions supported by CodeDeploy\. We recommend that you set the value to the region where your CodeDeploy applications, deployment groups, and application revisions are located \(for example, `us-west-2`\)\. For a list of regions, see [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\.

To set the environment variable, call the following from the terminal:

```
export AWS_REGION=supported-region
```

Where *supported\-region* is the region identifier \(for example, `us-west-2`\)\.

## Step 7: Install the CodeDeploy agent<a name="register-on-premises-instance-iam-user-arn-7"></a>

Install the CodeDeploy agent on the on\-premises instance:
+ For an Ubuntu Server on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Ubuntu Server](codedeploy-agent-operations-install-ubuntu.md), and then return to this page\.
+ For a RHEL on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Amazon Linux or RHEL](codedeploy-agent-operations-install-linux.md), and then return to this page\.
+ For a Windows Server on\-premises instance, follow the instructions in [Install the CodeDeploy agent for Windows Server](codedeploy-agent-operations-install-windows.md), and then return to this page\.

## Step 8: Register the on\-premises instance with CodeDeploy<a name="register-on-premises-instance-iam-user-arn-8"></a>

The instructions in this step assume you are registering the on\-premises instance from the on\-premises instance itself\. You can register an on\-premises instance from a separate device or instance that has the AWS CLI installed and configured, as described in [Step 5: Install and configure the AWS CLI](#register-on-premises-instance-iam-user-arn-5)\.

Use the AWS CLI to register the on\-premises instance with CodeDeploy so that it can be used in deployments\.

1. Before you can use the AWS CLI, you will need the user ARN of the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1)\. If you don't already have the user ARN, call the [get\-user](https://docs.aws.amazon.com/cli/latest/reference/iam/get-user.html) command, specifying the name of the IAM user \(with the `--user-name` option\) and querying for just the user ARN \(with the `--query` and `--output` options\):

   ```
   aws iam get-user --user-name CodeDeployUser-OnPrem --query "User.Arn" --output text
   ```

1. Call the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command, specifying:
   + A name that uniquely identifies the on\-premises instance \(with the `--instance-name` option\)\. 
**Important**  
To help identify the on\-premises instance, especially for debugging purposes, we strongly recommend that you specify a name that maps to some unique characteristic of the on\-premises instance \(for example, the serial number or an internal asset identifier, if applicable\)\. If you specify a MAC address as a name, be aware that MAC addresses contain characters that CodeDeploy does not allow, such as colon \(`:`\)\. For a list of allowed characters, see [CodeDeploy limits](reference.md#limits)\.
   + The user ARN of the IAM user you created in [Step 1: Create an IAM user for the on\-premises instance](#register-on-premises-instance-iam-user-arn-1) \(with the `--iam-user-arn` option\)\.

     For example:

     ```
     aws deploy register-on-premises-instance --instance-name AssetTag12010298EX --iam-user-arn arn:aws:iam::80398EXAMPLE:user/CodeDeployUser-OnPrem
     ```

## Step 9: Tag the on\-premises instance<a name="register-on-premises-instance-iam-user-arn-9"></a>

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

1. From the CodeDeploy menu, choose **On\-premises instances**\.

1. In the list of on\-premises instances, choose the arrow next to the on\-premises instance you want to tag\.

1. In the list of tags, select or enter the desired tag key and tag value\. After you enter the tag key and tag value, another row appears\. You can repeat this for up to 10 tags\. To remove a tag, choose the delete icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/delete-triggers-x.png)\)\.

1. After you have added tags, choose **Update Tags**\.

## Step 10: Deploy application revisions to the on\-premises instance<a name="register-on-premises-instance-iam-user-arn-10"></a>

You are now ready to deploy application revisions to the registered and tagged on\-premises instance\. 

You deploy application revisions to on\-premises instances in a way that's similar to deploying application revisions to Amazon EC2 instances\. For instructions, see [Create a deployment with CodeDeploy](deployments-create.md)\. These instructions include a link to prerequisites, including creating an application, creating a deployment group, and preparing an application revision\. If you need a simple sample application revision to deploy, you can create the one described in [Step 2: Create a sample application revision](tutorials-on-premises-instance-2-create-sample-revision.md) in the [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

**Important**  
If you reuse a CodeDeploy service role as part of creating a deployment group that targets on\-premises instances, you must include `Tag:get*` to the `Action` portion of the service role's policy statement\. For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

## Step 11: Track deployments to the on\-premises instance<a name="register-on-premises-instance-iam-user-arn-11"></a>

After you deploy an application revision to registered and tagged on\-premises instances, you can track the deployment's progress\.

You track deployments to on\-premises instances in a way that's similar to tracking deployments to Amazon EC2 instances\. For instructions, see [View CodeDeploydeployment details ](deployments-view-details.md)\.
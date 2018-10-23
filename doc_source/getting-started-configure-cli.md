# Step 2: Install or Upgrade and Then Configure the AWS CLI<a name="getting-started-configure-cli"></a>

To call AWS CodeDeploy commands from the AWS CLI on a local development machine, you must install the AWS CLI\. AWS CodeDeploy commands first became available in version 1\.6\.1 of the AWS CLI\. AWS CodeDeploy commands for working with on\-premises instances became available in 1\.7\.19 of the AWS CLI\. 

If you have an older version of the AWS CLI installed, you must upgrade it so the AWS CodeDeploy commands will be available\. You can call aws \-\-version to check the version\.

To install or upgrade the AWS CLI:

1. Follow the instructions in [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) to install or upgrade the AWS CLI\.

1. To configure the AWS CLI, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) and [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.
**Important**  
When you configure the AWS CLI, you will be prompted to specify an AWS region\. Specify one of the supported regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\.

1. To verify the installation or upgrade, call the following command from the AWS CLI:

   ```
   aws deploy help
   ```

   If successful, this command displays a list of available AWS CodeDeploy commands\.
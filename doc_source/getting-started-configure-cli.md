# Step 2: Install or upgrade and then configure the AWS CLI<a name="getting-started-configure-cli"></a>

To call CodeDeploy commands from the AWS CLI on a local development machine, you must install the AWS CLI\. CodeDeploy commands first became available in version 1\.6\.1 of the AWS CLI\. CodeDeploy commands for working with on\-premises instances became available in 1\.7\.19 of the AWS CLI\. 

If you have an older version of the AWS CLI installed, you must upgrade it so the CodeDeploy commands are available\. Call aws \-\-version to check the version\.

To install or upgrade the AWS CLI:

1. Follow the instructions in [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) to install or upgrade the AWS CLI\.

1. To configure the AWS CLI, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) and [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html)\.
**Important**  
When you configure the AWS CLI, you are prompted to specify an AWS Region\. Choose one of the supported regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\.

1. To verify the installation or upgrade, call the following command from the AWS CLI:

   ```
   aws deploy help
   ```

   If successful, this command displays a list of available CodeDeploy commands\.
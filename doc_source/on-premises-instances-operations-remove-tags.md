# Manually remove on\-premises instance tags from an on\-premises instance<a name="on-premises-instances-operations-remove-tags"></a>

Typically, you remove an on\-premises instance tag from an on\-premises instance when that tag is no longer being used, or you want to remove the on\-premises instance from any deployment groups that rely on that tag\. You can use the AWS CLI or the AWS CodeDeploy console to remove on\-premises instance tags from on\-premises instances\.

You do not need to remove the on\-premises instance tags from an on\-premises instance before you deregister it\. 

Manually removing on\-premises instance tags from an on\-premises instance does not deregister the instance\. It does not uninstall the CodeDeploy agent from the instance\. It does not remove the configuration file from the instance\. It does not delete the IAM user associated with the instance\. 

To automatically deregister the on\-premises instance, see [Automatically deregister an on\-premises instance](on-premises-instances-operations-deregister-automatically.md)\.

To manually deregister the on\-premises instance, see [Manually deregister an on\-premises instance](on-premises-instances-operations-deregister-manually.md)\.

To automatically uninstall the CodeDeploy agent and remove the configuration file from the on\-premises instance, see [Automatically uninstall the CodeDeploy agent and remove the configuration file from an on\-premises instance](on-premises-instances-operations-uninstall-agent.md)\.

To manually uninstall just the CodeDeploy agent from the on\-premises instance, see [Managing CodeDeploy agent operations](codedeploy-agent-operations.md)\.

To manually delete the associated IAM user, see [Deleting an IAM user from your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_DeletingUserFromAccount.html)\. 

**To remove on\-premises instance tags from an on\-premises instance \(CLI\)**
+ Call the [remove\-tags\-from\-on\-premises\-instances](https://docs.aws.amazon.com/cli/latest/reference/deploy/remove-tags-from-on-premises-instances.html), specifying:
  + The names that uniquely identify the on\-premises instance \(with the `--instance-names` option\)\. 
  + The names and values of the tags you want to remove \(with the `--tags` option\)\.

    For example:

    ```
    aws deploy remove-tags-from-on-premises-instances --instance-names AssetTag12010298EX --tags Key=Name,Value=CodeDeployDemo-OnPrem
    ```

**To remove on\-premises instance tags from an on\-premises instance \(console\)**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and choose **On\-premises instances**\.

1. In the list of on\-premises instances, choose the name of the on\-premises instance from which you want to remove tags\.

1. In **Tags**, choose **Remove** next to each tag you want to remove\.

1. After you have deleted the tags, choose **Update tags**\.
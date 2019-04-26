# Manually Deregister an On\-Premises Instance<a name="on-premises-instances-operations-deregister-manually"></a>

Typically, you deregister an on\-premises instance after you're no longer planning to deploy to it\. You use the AWS CLI to manually deregister on\-premises instances\.

Manually deregistering an on\-premises instance does not uninstall the CodeDeploy agent\. It does not remove the configuration file from the instance\. It does not delete the IAM user associated with the instance\. It does not remove any tags associated with the instance\.

To automatically uninstall the CodeDeploy agent and remove the configuration file from the on\-premises instance, see [Automatically Uninstall the CodeDeploy Agent and Remove the Configuration File from an On\-Premises Instance](on-premises-instances-operations-uninstall-agent.md)\.

To manually uninstall only the CodeDeploy agent, see [Managing CodeDeploy Agent Operations](codedeploy-agent-operations.md)\. 

To manually delete the associated IAM user, see [Deleting an IAM User from Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_DeletingUserFromAccount.html)\. 

To manually remove only the associated on\-premises instance tags, see [Manually Remove On\-Premises Instance Tags from an On\-Premises Instance](on-premises-instances-operations-remove-tags.md)\.
+ Call the [deregister\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/deregister-on-premises-instance.html) command, specifying the name that uniquely identifies the on\-premises instance \(with the `--instance-name` option\):

  ```
  aws deploy deregister-on-premises-instance --instance-name AssetTag12010298EX
  ```

  After you deregister an on\-premises instance:
  +  It stops appearing in the console immediately\. 
  +  You can create another instance with the same name immediately\. 
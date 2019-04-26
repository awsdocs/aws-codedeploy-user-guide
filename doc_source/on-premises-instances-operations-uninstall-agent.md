# Automatically Uninstall the CodeDeploy Agent and Remove the Configuration File from an On\-Premises Instance<a name="on-premises-instances-operations-uninstall-agent"></a>

Typically, you uninstall the CodeDeploy agent and remove the configuration file from an on\-premises instance after you're no longer planning to deploy to it\.

**Note**  
Automatically uninstalling the CodeDeploy agent and removing the configuration file from an on\-premises instance does not deregister an on\-premises instance\. It does not disassociate any on\-premises instance tags associated with the on\-premises instance\. It does not delete the IAM user associated with the on\-premises instance\.   
To automatically deregister the on\-premises instance, see [Automatically Deregister an On\-Premises Instance](on-premises-instances-operations-deregister-automatically.md)\.  
To manually deregister the on\-premises instance, see [Manually Deregister an On\-Premises Instance](on-premises-instances-operations-deregister-manually.md)\.  
To manually disassociate any associated on\-premises instance tags, see [Manually Remove On\-Premises Instance Tags from an On\-Premises Instance](on-premises-instances-operations-remove-tags.md)\.  
To manually uninstall the CodeDeploy agent from the on\-premises instance, see [Managing CodeDeploy Agent Operations](codedeploy-agent-operations.md)\.  
To manually delete the associated IAM user, see [Deleting an IAM User from Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_DeletingUserFromAccount.html)\. 

From the on\-premises instance, use the AWS CLI to call the [uninstall](https://docs.aws.amazon.com/cli/latest/reference/deploy/uninstall.html) command\.

For example:

```
aws deploy uninstall
```

The uninstall command does the following:

1. Stops the running CodeDeploy agent on the on\-premises instance\.

1. Uninstalls the CodeDeploy agent from the on\-premises instance\.

1. Removes the configuration file from the on\-premises instance\. \(For Ubuntu Server and RHEL, this is `/etc/codedeploy-agent/conf`/`codedeploy.onpremises.yml`\. For Windows Server, this is `C:\ProgramData\Amazon\CodeDeploy`\\`conf.onpremises.yml`\.\)
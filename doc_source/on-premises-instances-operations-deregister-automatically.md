# Automatically Deregister an On\-Premises Instance<a name="on-premises-instances-operations-deregister-automatically"></a>

Typically, you deregister an on\-premises instance after you're no longer planning to deploy to it\. When you deregister an on\-premises instance, even though the on\-premises instance might be part of a deployment group's on\-premises instance tags, the on\-premises instance will not be included in any deployments\. You can use the AWS CLI to deregister on\-premises instances\.

**Note**  
You cannot use the CodeDeploy console to deregister an on\-premises instance\. Also, deregistering an on\-premises instance does not disassociate any on\-premises instance tags that are associated with the on\-premises instance\. It does not uninstall the CodeDeploy agent from the on\-premises instance\. It does not remove the on\-premises instance configuration file from the on\-premises instance\.  
To use the CodeDeploy console to perform some \(but not all\) of the activities in this section, see the CodeDeploy console section of [Manually Deregister an On\-Premises Instance](on-premises-instances-operations-deregister-manually.md)\.  
To manually disassociate any associated on\-premises instance tags, see [Manually Remove On\-Premises Instance Tags from an On\-Premises Instance](on-premises-instances-operations-remove-tags.md)\.  
To automatically uninstall the CodeDeploy agent and remove the configuration file from the on\-premises instance, see [Automatically Uninstall the CodeDeploy Agent and Remove the Configuration File from an On\-Premises Instance](on-premises-instances-operations-uninstall-agent.md)\.  
To manually uninstall only the CodeDeploy agent from the on\-premises instance, see [Managing CodeDeploy Agent Operations](codedeploy-agent-operations.md)\. 

Use the AWS CLI to call the [deregister](https://docs.aws.amazon.com/cli/latest/reference/deploy/deregister.html) command, specifying:
+ The name that uniquely identifies the on\-premises instance to CodeDeploy \(with the `--instance-name` option\)\. 
+ Optionally, whether to delete the IAM user associated with the on\-premises instance \(with the `--delete-iam-user` option, the default\)\. If you do not want to delete the IAM user associated with the on\-premises instance, specify the `--no-delete-iam-user` option\.
+ Optionally, the AWS region where the on\-premises instance was registered with CodeDeploy \(with the `--region` option\)\. This must be one of the supported regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference* \(for example, `us-west-2`\)\. If this option is not specified, the default AWS region associated with the calling IAM user will be used\.

For example:

```
aws deploy deregister --instance-name AssetTag12010298EX --delete-iam-user --region us-west-2
```

The deregister command does the following:

1. Deregisters the on\-premises instance with CodeDeploy\.

1. If specified, deletes the IAM user associated with the on\-premises instance\.

After you deregister an on\-premises instance:
+  It stops appearing in the console immediately\. 
+  You can create another instance with the same name immediately\. 

If this command encounters any errors, an error message appears, describing how you can manually complete the remaining steps\. Otherwise, a success message appears, describing how to call the uninstall command\.
# Working with on\-premises instances for CodeDeploy<a name="instances-on-premises"></a>

An on\-premises instance is any physical device that is not an Amazon EC2 instance that can run the CodeDeploy agent and connect to public AWS service endpoints\. 

Deploying a CodeDeploy application revision to an on\-premises instance involves two major steps:
+ **Step 1** – Configure each on\-premises instance, register it with CodeDeploy, and then tag it\. 
+ **Step 2** – Deploy application revisions to the on\-premises instance\.
**Note**  
To experiment with creating and deploying a sample application revision to a correctly configured and registered on\-premises instance, see [Tutorial: Deploy an application to an on\-premises instance with CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\. For information about on\-premises instances and how they work with CodeDeploy, see [Working with on\-premises instances for CodeDeploy](#instances-on-premises)\.

If you don't want an on\-premises instance to be used in deployments anymore, you can simply remove the on\-premises instance tags from the deployment groups\. For a more robust approach, remove the on\-premises instance tags from the instance\. You can also explicitly deregister an on\-premises instance so it can no longer be used in any deployments\. For more information, see [Managing on\-premises instances operations in CodeDeploy](on-premises-instances-operations.md)\.

The instructions in this section show you how to configure an on\-premises instance and then register and tag it with CodeDeploy so it can be used in deployments\. This section also describes how to use CodeDeploy to get information about on\-premises instances and deregister an on\-premises instance after you're no longer planning to deploy to it\.

**Topics**
+ [Prerequisites for configuring an on\-premises instance](instances-on-premises-prerequisites.md)
+ [Register an on\-premises instance](on-premises-instances-register.md)
+ [Managing on\-premises instances operations](on-premises-instances-operations.md)
--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Working with On\-Premises Instances for AWS CodeDeploy<a name="instances-on-premises"></a>

An on\-premises instance is any physical device that is not an Amazon EC2 instance that can run the AWS CodeDeploy agent and connect to public AWS service endpoints\. 

Deploying an AWS CodeDeploy application revision to an on\-premises instance involves two major steps:
+ **Step 1** – Configure each on\-premises instance, register it with AWS CodeDeploy, and then tag it\. 
+ **Step 2** – Deploy application revisions to the on\-premises instance\.
**Note**  
To experiment with creating and deploying a sample application revision to a correctly configured and registered on\-premises instance, see [Tutorial: Deploy an Application to an On\-Premises Instance with AWS CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\. For information about on\-premises instances and how they work with AWS CodeDeploy, see [Working with On\-Premises Instances for AWS CodeDeploy](#instances-on-premises)\.

If you don't want an on\-premises instance to be used in deployments anymore, you can simply remove the on\-premises instance tags from the deployment groups\. For a more robust approach, remove the on\-premises instance tags from the instance\. You can also explicitly deregister an on\-premises instance so it can no longer be used in any deployments\. For more information, see [Managing On\-Premises Instances Operations in AWS CodeDeploy](on-premises-instances-operations.md)\.

The instructions in this section show you how to configure an on\-premises instance and then register and tag it with AWS CodeDeploy so it can be used in deployments\. This section also describes how to use AWS CodeDeploy to get information about on\-premises instances and deregister an on\-premises instance after you're no longer planning to deploy to it\.

**Topics**
+ [Prerequisites for Configuring an On\-Premises Instance](instances-on-premises-prerequisites.md)
+ [Register an On\-Premises Instance](on-premises-instances-register.md)
+ [Managing On\-Premises Instances Operations](on-premises-instances-operations.md)
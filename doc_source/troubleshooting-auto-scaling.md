# Troubleshoot Amazon EC2 Auto Scaling issues<a name="troubleshooting-auto-scaling"></a>

**Topics**
+ [General Amazon EC2 Auto Scaling troubleshooting](#troubleshooting-auto-scaling-general)
+ ["CodeDeployRole does not give you permission to perform operations in the following AWS service: AmazonAutoScaling" error](#troubleshooting-auto-scaling-permissions-error)
+ [Instances in an Amazon EC2 Auto Scaling group are continuously provisioned and terminated before a revision can be deployed](#troubleshooting-auto-scaling-provision-termination-loop)
+ [Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail](#troubleshooting-auto-scaling-reboot)
+ [Avoid associating multiple deployment groups with a single Amazon EC2 Auto Scaling group](#troubleshooting-multiple-depgroups)
+ [Amazon EC2 instances in an Amazon EC2 Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"](#troubleshooting-auto-scaling-heartbeat)
+ [Mismatched Amazon EC2 Auto Scaling lifecycle hooks might cause automatic deployments to Amazon EC2 Auto Scaling groups to stop or fail](#troubleshooting-auto-scaling-hooks)

## General Amazon EC2 Auto Scaling troubleshooting<a name="troubleshooting-auto-scaling-general"></a>

Deployments to Amazon EC2 instances in an Amazon EC2 Auto Scaling group can fail for the following reasons:
+ **Amazon EC2 Auto Scaling continuously launches and terminates Amazon EC2 instances\.** If CodeDeploy cannot automatically deploy your application revision, Amazon EC2 Auto Scaling continuously launches and terminates Amazon EC2 instances\. 

  Disassociate the Amazon EC2 Auto Scaling group from the CodeDeploy deployment group or change the configuration of your Amazon EC2 Auto Scaling group so that the desired number of instances matches the current number of instances \(thus preventing Amazon EC2 Auto Scaling from launching any more Amazon EC2 instances\)\. For more information, see [Change deployment group settings with CodeDeploy](deployment-groups-edit.md) or [Manual Scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-manual-scaling.html)\.
+ **The CodeDeploy agent is unresponsive\.** The CodeDeploy agent might not be installed if initialization scripts \(for example, cloud\-init scripts\) that run immediately after an Amazon EC2 instance is launched or started take more than one hour to run\. CodeDeploy has a one\-hour timeout for the CodeDeploy agent to respond to pending deployments\. To address this issue, move your initialization scripts into your CodeDeploy application revision\.
+ **An Amazon EC2 instance in an Amazon EC2 Auto Scaling group reboots during a deployment\.** Your deployment can fail if an Amazon EC2 instance is rebooted during a deployment or the CodeDeploy agent is shut down while processing a deployment command\. For more information, see [Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail](#troubleshooting-auto-scaling-reboot)\.
+ **Multiple application revisions are deployed simultaneously to the same Amazon EC2 instance in an Amazon EC2 Auto Scaling group\.** Deploying multiple application revisions to the same Amazon EC2 instance in an Amazon EC2 Auto Scaling group at the same time can fail if one of the deployments has scripts that run for more than a few minutes\. Do not deploy multiple application revisions to the same Amazon EC2 instances in an Amazon EC2 Auto Scaling group\.
+ **A deployment fails for new Amazon EC2 instances that are launched as part of an Amazon EC2 Auto Scaling group\.** In this scenario, running the scripts in a deployment can prevent the launching of Amazon EC2 instances in the Amazon EC2 Auto Scaling group\. \(Other Amazon EC2 instances in the Amazon EC2 Auto Scaling group might appear to be running normally\.\) To address this issue, make sure that all other scripts are complete first:
  + **CodeDeploy agent is not included in your AMI **: If you use the cfn\-init command to install the CodeDeploy agent while launching a new instance, place the agent installation script at the end of the `cfn-init` section of your AWS CloudFormation template\. 
  + **CodeDeploy agent is included in your AMI **: Configure the AMI so that the agent is in a `Stopped` state when the instance is created, and then include a script for starting the agent as the final step in your `cfn-init` script library\. 

  \.

## "CodeDeployRole does not give you permission to perform operations in the following AWS service: AmazonAutoScaling" error<a name="troubleshooting-auto-scaling-permissions-error"></a>

 Deployments that use an Auto Scaling group created with a launch template require the following permissions\. These are in addition to the permissions granted by the `AWSCodeDeployRole` AWS managed policy\. 
+  `ec2:RunInstances` 
+  `ec2:CreateTags` 
+  `iam:PassRole` 

 You might received this error if you are missing these permissions\. For more information, see [Tutorial: Use CodeDeploy to deploy an application to an Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group.md) and [Creating a launch template for an Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html)\. 

## Instances in an Amazon EC2 Auto Scaling group are continuously provisioned and terminated before a revision can be deployed<a name="troubleshooting-auto-scaling-provision-termination-loop"></a>

In some cases, an error can prevent a successful deployment to newly provisioned instances in an Amazon EC2 Auto Scaling group\. The results are no healthy instances and no successful deployments\. Because the deployment can't run or be completed successfully, the instances are terminated soon after being created\. The Amazon EC2 Auto Scaling group configuration then causes another batch of instances to be provisioned to try to meet the minimum healthy hosts requirement\. This batch is also terminated, and the cycle continues\.

Possible causes include:
+ Failed Amazon EC2 Auto Scaling group health checks\.
+ An error in the application revision\.

To work around this issue, follow these steps:

1. Manually create an Amazon EC2 instance that is not part of the Amazon EC2 Auto Scaling group\. Tag the instance with a unique EC2 instance tag\.

1. Add the new instance to the affected deployment group\. 

1. Deploy a new, error\-free application revision to the deployment group\.

This prompts the Amazon EC2 Auto Scaling group to deploy the application revision to future instances in the Amazon EC2 Auto Scaling group\. 

**Note**  
After you confirm that deployments are successful, delete the instance you created to avoid ongoing charges to your AWS account\.

## Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail<a name="troubleshooting-auto-scaling-reboot"></a>

If an Amazon EC2 instance is launched through Amazon EC2 Auto Scaling, and the instance is then terminated or rebooted, deployments to that instance might fail for the following reasons:
+ During an in\-progress deployment, a scale\-in event or any other termination event causes the instance to detach from the Amazon EC2 Auto Scaling group and then terminate\. Because the deployment cannot be completed, it fails\.
+ The instance is rebooted, but it takes more than five minutes for the instance to start\. CodeDeploy treats this as a timeout\. The service fails all current and future deployments to the instance\.

To address this issue:
+ In general, make sure that all deployments are complete before the instance is terminated or rebooted\. Make sure that all deployments start after the instance has started or been rebooted\. 
+ Deployments can fail if you specify a Windows Server base Amazon Machine Image \(AMI\) for an Amazon EC2 Auto Scaling configuration and use the EC2Config service to set the computer name of the instance\. To fix this issue, in the Windows Server base AMI, on the **General** tab of **Ec2 Service Properties**, clear **Set Computer Name**\. After you clear this check box, this behavior is disabled for all new Windows Server Amazon EC2 Auto Scaling instances launched with that Windows Server base AMI\. For Windows Server Amazon EC2 Auto Scaling instances on which this behavior enabled, you do not need to clear this check box\. Simply redeploy failed deployments to those instances after they have been rebooted\.

## Avoid associating multiple deployment groups with a single Amazon EC2 Auto Scaling group<a name="troubleshooting-multiple-depgroups"></a>

As a best practice, you should associate only one deployment group with each Amazon EC2 Auto Scaling group\. 

This is because if Amazon EC2 Auto Scaling scales up an instance that has hooks associated with multiple deployment groups, it sends notifications for all of the hooks at once\. This causes multiple deployments to each instance to start at the same time\. When multiple deployments send commands to the CodeDeploy agent at the same time, the five\-minute timeout between a lifecycle event and either the start of the deployment or the end of the previous lifecycle event might be reached\. If this happens, the deployment fails, even if a deployment process is otherwise running as expected\. 

**Note**  
The default timeout for a script in a lifecycle event is 30 minutes\. You can change the timeout to a different value in the AppSpec file\. For more information, see [Add an AppSpec file for an EC2/On\-Premises deployment](application-revisions-appspec-file.md#add-appspec-file-server)\.

It's not possible to control the order in which deployments occur if more than one deployment attempts to run at the same time\. 

Finally, if deployment to any instance fails, Amazon EC2 Auto Scaling immediately terminates the instance\. When that first instance shuts down, the other running deployments start to fail\. Because CodeDeploy has a one\-hour timeout for the CodeDeploy agent to respond to pending deployments, it can take up to 60 minutes for each instance to time out\. 

For more information about Amazon EC2 Auto Scaling, see [Under the hood: CodeDeploy and Auto Scaling integration](http://aws.amazon.com/blogs/devops/under-the-hood-aws-codedeploy-and-auto-scaling-integration/)\.

## Amazon EC2 instances in an Amazon EC2 Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"<a name="troubleshooting-auto-scaling-heartbeat"></a>

An Amazon EC2 Auto Scaling group might fail to launch new Amazon EC2 instances, generating a message similar to the following: 

`Launching a new Amazon EC2 instance <instance-Id>. Status Reason: Instance failed to complete user's Lifecycle Action: Lifecycle Action with token<token-Id> was abandoned: Heartbeat Timeout`\. 

This message usually indicates one of the following: 
+ The maximum number of concurrent deployments associated with an AWS account was reached\. For more information about deployment limits, see [CodeDeploy limits](reference.md#limits)\. 
+ An application in CodeDeploy was deleted before its associated deployment groups were updated or deleted\. 

When you delete an application or deployment group, CodeDeploy attempts to clean up any Amazon EC2 Auto Scaling hooks associated with it, but some hooks might remain\. If you run a command to delete a deployment group, the leftover hooks are returned in the output\. However, if you run a command to delete an application, the leftover hooks do not appear in the output\.

Therefore, as a best practice, you should delete all deployment groups associated with an application before you delete the application\. You can use the command output to identify the lifecycle hooks that must be deleted manually\. 

If you receive a “Heartbeat Timeout” error message, you can determine if leftover lifecycle hooks are the cause and resolve the problem by doing the following:

1. Run either the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command or [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command\. Examine the output of the call\. If the output contains a `hooksNotCleanedUp` structure with a list of Amazon EC2 Auto Scaling lifecycle hooks, leftover lifecycle hooks are most likely the cause of the error\. 

1. Call the [describe\-lifecycle\-hooks](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-lifecycle-hooks.html) command, specifying the name of the Amazon EC2 Auto Scaling group associated with the Amazon EC2 instances that fail to launch\. In the output, look for any Amazon EC2 Auto Scaling lifecycle hook names that correspond to the `hooksNotCleanedUp` structure you identified in step 1\. Or look for Amazon EC2 Auto Scaling lifecycle hook names that contain the name of the deployment group\.

1. Call the [delete\-lifecycle\-hook](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-lifecycle-hook.html) command for each Amazon EC2 Auto Scaling lifecycle hook\. Specify the Amazon EC2 Auto Scaling group and lifecycle hook\.

If you delete \(from an Amazon EC2 Auto Scaling group\) all of the Amazon EC2 Auto Scaling lifecycle hooks that were created by CodeDeploy, then CodeDeploy no longer deploys to Amazon EC2 instances that are scaled up as part of that Amazon EC2 Auto Scaling group\.

## Mismatched Amazon EC2 Auto Scaling lifecycle hooks might cause automatic deployments to Amazon EC2 Auto Scaling groups to stop or fail<a name="troubleshooting-auto-scaling-hooks"></a>

Amazon EC2 Auto Scaling and CodeDeploy use lifecycle hooks to determine which application revisions should be deployed to which Amazon EC2 instances after they are launched in Amazon EC2 Auto Scaling groups\. Automatic deployments can stop or fail if lifecycle hooks and information about these hooks do not match exactly in Amazon EC2 Auto Scaling and CodeDeploy\.

If deployments to an Amazon EC2 Auto Scaling group are failing, see if the lifecycle hook names in Amazon EC2 Auto Scaling and CodeDeploy match\. If not, use these AWS CLI command calls\.

First, get the list of lifecycle hook names for both the Amazon EC2 Auto Scaling group and the deployment group:

1. Call the [describe\-lifecycle\-hooks](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-lifecycle-hooks.html) command, specifying the name of the Amazon EC2 Auto Scaling group associated with the deployment group in CodeDeploy\. In the output, in the `LifecycleHooks` list, make a note of each `LifecycleHookName` value\.

1. Call the [get\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-group.html) command, specifying the name of the deployment group associated with the Amazon EC2 Auto Scaling group\. In the output, in the `autoScalingGroups` list, find each item whose name value matches the Amazon EC2 Auto Scaling group name, and then make a note of the corresponding `hook` value\.

Now compare the two sets of lifecycle hook names\. If they match exactly, character for character, then this is not the issue\. You may want to try other Amazon EC2 Auto Scaling troubleshooting steps described elsewhere in this section\.

However, if the two sets of lifecycle hook names do not match exactly, character for character, do the following:

1. If there are lifecycle hook names in the describe\-lifecycle\-hooks command output that are not also in the get\-deployment\-group command output, then do the following:

   1. For each lifecycle hook name in the describe\-lifecycle\-hooks command output, call the [delete\-lifecycle\-hook](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-lifecycle-hook.html) command\.

   1. Call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, specifying the name of the original Amazon EC2 Auto Scaling group\. CodeDeploy creates new, replacement lifecycle hooks in the Amazon EC2 Auto Scaling group and associates the lifecycle hooks with the deployment group\. Automatic deployments should now resume as new instances are added to the Amazon EC2 Auto Scaling group\. 

1. If there are lifecycle hook names in the get\-deployment\-group command output that are not also in the describe\-lifecycle\-hooks command output, do the following:

   1. Call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, but do not specify the name of the original Amazon EC2 Auto Scaling group\.

   1. Call the update\-deployment\-group command again, but this time specify the name of the original Amazon EC2 Auto Scaling group\. CodeDeploy re\-creates the missing lifecycle hooks in the Amazon EC2 Auto Scaling group\. Automatic deployments should now resume as new instances are added to the Amazon EC2 Auto Scaling group\.

After you get the two sets of lifecycle hook names to match exactly, character for character, application revisions should be deployed again, but only to new instances as they are added to the Amazon EC2 Auto Scaling group\. Deployments do not occur automatically to instances that are already in the Amazon EC2 Auto Scaling group\.
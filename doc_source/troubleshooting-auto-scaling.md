# Troubleshoot Auto Scaling Issues<a name="troubleshooting-auto-scaling"></a>


+ [General Auto Scaling troubleshooting](#troubleshooting-auto-scaling-general)
+ [Instances in an Auto Scaling group are continuously provisioned and terminated before a revision can be deployed](#troubleshooting-auto-scaling-provision-termination-loop)
+ [Terminating or rebooting an Auto Scaling instance may cause deployments to fail](#troubleshooting-auto-scaling-reboot)
+ [Avoid associating multiple deployment groups with a single Auto Scaling group](#troubleshooting-multiple-depgroups)
+ [Amazon EC2 instances in an Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"](#troubleshooting-auto-scaling-heartbeat)
+ [Mismatched Auto Scaling lifecycle hooks might cause automatic deployments to Auto Scaling groups to stop or fail](#troubleshooting-auto-scaling-hooks)

## General Auto Scaling troubleshooting<a name="troubleshooting-auto-scaling-general"></a>

Deployments to Amazon EC2 instances in an Auto Scaling group can fail for the following reasons:

+ **Auto Scaling continuously launches and terminates Amazon EC2 instances\.** If AWS CodeDeploy cannot automatically deploy your application revision, Auto Scaling will continuously launch and terminate Amazon EC2 instances\. 

  Disassociate the Auto Scaling group from the AWS CodeDeploy deployment group or change the configuration of your Auto Scaling group so that the desired number of instances matches the current number of instances \(thus preventing Auto Scaling from launching any more Amazon EC2 instances\)\. For more information, see [Change Deployment Group Settings with AWS CodeDeploy](deployment-groups-edit.md) or [Configuring Your Auto Scaling Groups](http://docs.aws.amazon.com/autoscaling/latest/userguide/WorkingWithASG.html)\.

+ **The AWS CodeDeploy agent is unresponsive\.** The AWS CodeDeploy agent may not be installed if initialization scripts \(for example, cloud\-init scripts\) that run immediately after an Amazon EC2 instance is launched or started take more than one hour to run\. AWS CodeDeploy has a one\-hour timeout for the AWS CodeDeploy agent to respond to pending deployments\. To address this issue, move your initialization scripts into your AWS CodeDeploy application revision\.

+ **An Amazon EC2 instance in an Auto Scaling group reboots during a deployment\.** Your deployment can fail if an Amazon EC2 instance is rebooted during a deployment or the AWS CodeDeploy agent is shut down while processing a deployment command\. For more information, see [Terminating or rebooting an Auto Scaling instance may cause deployments to fail](#troubleshooting-auto-scaling-reboot)\.

+ **Multiple application revisions are deployed simultaneously to the same Amazon EC2 instance in an Auto Scaling group\.** Deploying multiple application revisions to the same Amazon EC2 instance in an Auto Scaling group at the same time can fail if one of the deployments has scripts that run for more than a few minutes\. Do not deploy multiple application revisions to the same Amazon EC2 instances in an Auto Scaling group\.

+ **A deployment fails for new Amazon EC2 instances that are launched as part of an Auto Scaling group\.** Typically in this scenario, running the scripts in a deployment can prevent the launching of Amazon EC2 instances in the Auto Scaling group\. \(Other Amazon EC2 instances in the Auto Scaling group may appear to be running normally\.\) To address this issue, make sure that all other scripts are complete first:

  + **AWS CodeDeploy agent is not included in your AMI **: If you use the cfn\-init command to install the AWS CodeDeploy agent while launching a new instance, place the agent installation script at the end of the `cfn-init` section of your AWS CloudFormation template\. 

  + **AWS CodeDeploy agent is included in your AMI **: If you include the AWS CodeDeploy agent in your AMI, configure it so that the agent is in a `Stopped` state when the instance is created, and then include a script for starting the agent as the final step in your `cfn-init` script library\. 

  \.

## Instances in an Auto Scaling group are continuously provisioned and terminated before a revision can be deployed<a name="troubleshooting-auto-scaling-provision-termination-loop"></a>

In some cases, an error can prevent a successful deployment to newly provisioned instances in an Auto Scaling group\. The results are no healthy instances and no successful deployments\. Because the deployment can't run or be completed successfully, the instances are terminated soon after being created\. The Auto Scaling group configuration then causes another batch of instances to be provisioned to try to meet the minimum healthy hosts requirement\. This batch is also terminated, and the cycle continues\.

Possible causes include:

+ Failed Auto Scaling group health checks

+ An error in the application revision 

To work around this issue, follow these steps:

1. Manually create an Amazon EC2 instance that is not part of the Auto Scaling group\. Tag the instance with a unique EC2 instance tag\.

1. Add the new instance to the affected deployment group\. 

1. Deploy a new, error\-free application revision to the deployment group\.

This will prompt the Auto Scaling group to deploy the application revision to future instances in the Auto Scaling group\. 

**Note**  
After you confirm that deployments are successful, you can delete the instance you created to avoid ongoing billing charges for it\.

## Terminating or rebooting an Auto Scaling instance may cause deployments to fail<a name="troubleshooting-auto-scaling-reboot"></a>

If an Amazon EC2 instance is launched through Auto Scaling, and the instance is then terminated or rebooted, deployments to that instance may fail for the following reasons:

+ During an in\-progress deployment, a scale\-in event or any other termination event will cause the instance to detach from the Auto Scaling group and then terminate\. Because the deployment cannot be completed, it fails\.

+ The instance is rebooted, but it takes more than five minutes for the instance to start\. AWS CodeDeploy considers this to be a timeout\. The service will fail all current and future deployments to the instance\.

To address this issue:

+ In general, make sure all deployments are complete before the instance is terminated or rebooted\. Make sure all deployments start after the instance has started or been rebooted\. 

+ If you specify a Windows Server base Amazon Machine Image \(AMI\) for an Auto Scaling configuration, and you use the EC2Config service to set the computer name of the instance, this behavior can cause deployments to fail\. To disable this behavior, in the Windows Server base AMI, on the **General** tab of the **Ec2 Service Properties** dialog box, clear the **Set Computer Name** box\. After you clear this box, this behavior will be disabled for all new Windows Server Auto Scaling instances launched with that Windows Server base AMI\. For Windows Server Auto Scaling instances on which this behavior enabled, you do not need to clear this box\. Simply redeploy failed deployments to those instances after they have been rebooted\.

## Avoid associating multiple deployment groups with a single Auto Scaling group<a name="troubleshooting-multiple-depgroups"></a>

As a best practice, you should associate only one deployment group with each Auto Scaling group\. 

This is because if Auto Scaling scales up an instance that has hooks associated with multiple deployment groups, it sends notifications for all of the hooks at once\. This causes multiple deployments to each instance to start at the same time\. When multiple deployments send commands to the AWS CodeDeploy agent at the same time, the five\-minute timeout between a lifecycle event and either the start of the deployment or the end of the previous lifecycle event might be reached\. If this happens, the deployment fails, even if a deployment process is otherwise running as expected\.\) 

**Note**  
The default timeout for a script in a lifecycle event is 30 minutes\. You can change the timeout to a different value in the AppSpec file\. For more information, see [Add an AppSpec File for an EC2/On\-Premises Deployment](application-revisions-appspec-file.md#add-appspec-file-server)\.

It's not possible to control the order in which deployments occur if more than one deployment attempts to run at the same time\. 

Finally, if deployment to any instance fails, Auto Scaling immediately terminates the instance\. When that first instance shuts down, the other deployments that were running will begin to fail\. Because AWS CodeDeploy has a one\-hour timeout for the AWS CodeDeploy agent to respond to pending deployments, it can take up to 60 minutes for each instance to time out\. 

For more information about problems with attempting multiple deployments to an instance at the same time, see [Avoid concurrent deployments to the same Amazon EC2 instance](troubleshooting-general.md#troubleshooting-concurrent-deployments)\.

For more information about Auto Scaling, see [Under the Hood: AWS CodeDeploy and Auto Scaling Integration](http://aws.amazon.com/blogs/devops/under-the-hood-aws-codedeploy-and-auto-scaling-integration/)\.

## Amazon EC2 instances in an Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"<a name="troubleshooting-auto-scaling-heartbeat"></a>

An Auto Scaling group might fail to launch new Amazon EC2 instances, generating a message similar to the following: 

`Launching a new Amazon EC2 instance <instance-Id>. Status Reason: Instance failed to complete user's Lifecycle Action: Lifecycle Action with token<token-Id> was abandoned: Heartbeat Timeout`\. 

This message usually indicates one of the following: 

+ The maximum number of concurrent deployments associated with an AWS account was reached\. For more information about deployment limits, see [AWS CodeDeploy Limits](limits.md)\. 

+ An application in AWS CodeDeploy was deleted before its associated deployment groups were updated or deleted\. 

When you delete an application or deployment group, AWS CodeDeploy attempts to clean up any Auto Scaling hooks associated with it, but some hooks might remain\. If you run a command to delete a deployment group, the leftover hooks will be returned in the output; however, if you run a command to delete an application, the leftover hooks will not appear in the output\.

Therefore, as a best practice, you should delete all deployment groups associated with an application before you delete the application\. You can use the command output to identify the lifecycle hooks that must be deleted manually\. 

If you are receiving a “Heartbeat Timeout” error message, you can determine whether leftover lifecycle hooks are the cause and resolve the problem by doing the following:

1. Run either the [update\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command or [delete\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command\. Examine the output of the call\. If the output contains a `hooksNotCleanedUp` structure with a list of Auto Scaling lifecycle hooks, leftover lifecycle hooks are most likely the cause of the error\. 

1. Call the [describe\-lifecycle\-hooks](http://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-lifecycle-hooks.html) command, specifying the name of the Auto Scaling group associated with the Amazon EC2 instances that fail to launch\. In the output, look for any Auto Scaling lifecycle hook names that correspond to the `hooksNotCleanedUp` structure you identified in step 1\. Alternatively, look for Auto Scaling lifecycle hook names that contain the name of the deployment group\.

1. Call the [delete\-lifecycle\-hook](http://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-lifecycle-hook.html) command for each Auto Scaling lifecycle hook\. Specify the Auto Scaling group and lifecycle hook\.

If you delete \(from an Auto Scaling group\) all of the Auto Scaling lifecycle hooks that were created by AWS CodeDeploy, then AWS CodeDeploy will no longer deploy to Amazon EC2 instances that are scaled up as part of that Auto Scaling group\.

## Mismatched Auto Scaling lifecycle hooks might cause automatic deployments to Auto Scaling groups to stop or fail<a name="troubleshooting-auto-scaling-hooks"></a>

Auto Scaling and AWS CodeDeploy use lifecycle hooks to determine which application revisions should be deployed to which Amazon EC2 instances after they are launched in Auto Scaling groups\. Automatic deployments can stop or fail if lifecycle hooks and information about these hooks do not match exactly in Auto Scaling and AWS CodeDeploy\.

If deployments to an Auto Scaling group are failing, see if the lifecycle hook names in Auto Scaling and AWS CodeDeploy match\. If not, use these AWS CLI command calls\.

First, get the list of lifecycle hook names for both the Auto Scaling group and the deployment group:

1. Call the [describe\-lifecycle\-hooks](http://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-lifecycle-hooks.html) command, specifying the name of the Auto Scaling group associated with the deployment group in AWS CodeDeploy\. In the output, in the `LifecycleHooks` list, make a note of each `LifecycleHookName` value\.

1. Call the [get\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-group.html) command, specifying the name of the deployment group associated with the Auto Scaling group\. In the output, in the `autoScalingGroups` list, find each item whose name value matches the Auto Scaling group name, and then make a note of the corresponding `hook` value\.

Now compare the two sets of lifecycle hook names\. If they match exactly, character for character, then this is not the issue\. You may want to try other Auto Scaling troubleshooting steps described elsewhere in this section\.

However, if the two sets of lifecycle hook names do not match exactly, character for character, do the following:

1. If there are lifecycle hook names in the describe\-lifecycle\-hooks command output that are not also in the get\-deployment\-group command output, then do the following:

   1. For each lifecycle hook name in the describe\-lifecycle\-hooks command output, call the [delete\-lifecycle\-hook](http://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-lifecycle-hook.html) command\.

   1. Call the [update\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, specifying the name of the original Auto Scaling group\. AWS CodeDeploy will create new, replacement lifecycle hooks in the Auto Scaling group and associate the lifecycle hooks with the deployment group\. Automatic deployments should now resume as new instances are added to the Auto Scaling group\. 

1. If there are lifecycle hook names in the get\-deployment\-group command output that are not also in the describe\-lifecycle\-hooks command output, then do the following:

   1. Call the [update\-deployment\-group](http://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, but do not specify the name of the original Auto Scaling group\.

   1. Call the update\-deployment\-group command again, but this time specify the name of the original Auto Scaling group\. AWS CodeDeploy will re\-create the missing lifecycle hooks in the Auto Scaling group\. Automatic deployments should now resume as new instances are added to the Auto Scaling group\.

After you get the two sets of lifecycle hook names to match exactly, character for character, application revisions should be deployed again, but only to new instances as they are added to the Auto Scaling group\. Deployments will not occur automatically to instances already in the Auto Scaling group\.
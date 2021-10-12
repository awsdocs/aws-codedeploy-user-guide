# Troubleshoot Amazon EC2 Auto Scaling issues<a name="troubleshooting-auto-scaling"></a>

**Topics**
+ [General Amazon EC2 Auto Scaling troubleshooting](#troubleshooting-auto-scaling-general)
+ ["CodeDeployRole does not give you permission to perform operations in the following AWS service: AmazonAutoScaling" error](#troubleshooting-auto-scaling-permissions-error)
+ [Instances in an Amazon EC2 Auto Scaling group are continuously provisioned and terminated before a revision can be deployed](#troubleshooting-auto-scaling-provision-termination-loop)
+ [Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail](#troubleshooting-auto-scaling-reboot)
+ [Avoid associating multiple deployment groups with a single Amazon EC2 Auto Scaling group](#troubleshooting-multiple-depgroups)
+ [EC2 instances in an Amazon EC2 Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"](#troubleshooting-auto-scaling-heartbeat)
+ [Mismatched Amazon EC2 Auto Scaling lifecycle hooks might cause automatic deployments to Amazon EC2 Auto Scaling groups to stop or fail](#troubleshooting-auto-scaling-hooks)
+ ["The deployment failed because no instances were found for your deployment group" error](#troubleshooting-deployment-failed-because-no-instances-found)

## General Amazon EC2 Auto Scaling troubleshooting<a name="troubleshooting-auto-scaling-general"></a>

Deployments to EC2 instances in an Amazon EC2 Auto Scaling group can fail for the following reasons:
+ **Amazon EC2 Auto Scaling continuously launches and terminates EC2 instances\.** If CodeDeploy cannot automatically deploy your application revision, Amazon EC2 Auto Scaling continuously launches and terminates EC2 instances\. 

  Disassociate the Amazon EC2 Auto Scaling group from the CodeDeploy deployment group or change the configuration of your Amazon EC2 Auto Scaling group so that the desired number of instances matches the current number of instances \(thus preventing Amazon EC2 Auto Scaling from launching any more EC2 instances\)\. For more information, see [Change deployment group settings with CodeDeploy](deployment-groups-edit.md) or [Manual Scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-manual-scaling.html)\.
+ **The CodeDeploy agent is unresponsive\.** The CodeDeploy agent might not be installed if initialization scripts \(for example, cloud\-init scripts\) that run immediately after an EC2 instance is launched or started take more than one hour to run\. CodeDeploy has a one\-hour timeout for the CodeDeploy agent to respond to pending deployments\. To address this issue, move your initialization scripts into your CodeDeploy application revision\.
+ **An EC2 instance in an Amazon EC2 Auto Scaling group reboots during a deployment\.** Your deployment can fail if an EC2 instance is rebooted during a deployment or the CodeDeploy agent is shut down while processing a deployment command\. For more information, see [Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail](#troubleshooting-auto-scaling-reboot)\.
+ **Multiple application revisions are deployed simultaneously to the same EC2 instance in an Amazon EC2 Auto Scaling group\.** Deploying multiple application revisions to the same EC2 instance in an Amazon EC2 Auto Scaling group at the same time can fail if one of the deployments has scripts that run for more than a few minutes\. Do not deploy multiple application revisions to the same EC2 instances in an Amazon EC2 Auto Scaling group\.
+ **A deployment fails for new EC2 instances that are launched as part of an Amazon EC2 Auto Scaling group\.** In this scenario, running the scripts in a deployment can prevent the launching of EC2 instances in the Amazon EC2 Auto Scaling group\. \(Other EC2 instances in the Amazon EC2 Auto Scaling group might appear to be running normally\.\) To address this issue, make sure that all other scripts are complete first:
  + **CodeDeploy agent is not included in your AMI **: If you use the cfn\-init command to install the CodeDeploy agent while launching a new instance, place the agent installation script at the end of the `cfn-init` section of your AWS CloudFormation template\. 
  + **CodeDeploy agent is included in your AMI **: Configure the AMI so that the agent is in a `Stopped` state when the instance is created, and then include a script for starting the agent as the final step in your `cfn-init` script library\. 

## "CodeDeployRole does not give you permission to perform operations in the following AWS service: AmazonAutoScaling" error<a name="troubleshooting-auto-scaling-permissions-error"></a>

 Deployments that use an Auto Scaling group created with a launch template require the following permissions\. These are in addition to the permissions granted by the `AWSCodeDeployRole` AWS managed policy\. 
+  `EC2:RunInstances` 
+  `EC2:CreateTags` 
+  `iam:PassRole` 

 You might received this error if you are missing these permissions\. For more information, see [Tutorial: Use CodeDeploy to deploy an application to an Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group.md), [Creating a launch template for an Auto Scaling group](https://docs.aws.amazon.com/autoscaling/EC2/userguide/create-launch-template.html), and [Launch template support](https://docs.aws.amazon.com/autoscaling/EC2/userguide/EC2-auto-scaling-launch-template-permissions.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Instances in an Amazon EC2 Auto Scaling group are continuously provisioned and terminated before a revision can be deployed<a name="troubleshooting-auto-scaling-provision-termination-loop"></a>

In some cases, an error can prevent a successful deployment to newly provisioned instances in an Amazon EC2 Auto Scaling group\. The results are no healthy instances and no successful deployments\. Because the deployment can't run or be completed successfully, the instances are terminated soon after being created\. The Amazon EC2 Auto Scaling group configuration then causes another batch of instances to be provisioned to try to meet the minimum healthy hosts requirement\. This batch is also terminated, and the cycle continues\.

Possible causes include:
+ Failed Amazon EC2 Auto Scaling group health checks\.
+ An error in the application revision\.

To work around this issue, follow these steps:

1. Manually create an EC2 instance that is not part of the Amazon EC2 Auto Scaling group\. Tag the instance with a unique EC2 instance tag\.

1. Add the new instance to the affected deployment group\. 

1. Deploy a new, error\-free application revision to the deployment group\.

This prompts the Amazon EC2 Auto Scaling group to deploy the application revision to future instances in the Amazon EC2 Auto Scaling group\. 

**Note**  
After you confirm that deployments are successful, delete the instance you created to avoid ongoing charges to your AWS account\.

## Terminating or rebooting an Amazon EC2 Auto Scaling instance might cause deployments to fail<a name="troubleshooting-auto-scaling-reboot"></a>

If an EC2 instance is launched through Amazon EC2 Auto Scaling, and the instance is then terminated or rebooted, deployments to that instance might fail for the following reasons:
+ During an in\-progress deployment, a scale\-in event or any other termination event causes the instance to detach from the Amazon EC2 Auto Scaling group and then terminate\. Because the deployment cannot be completed, it fails\.
+ The instance is rebooted, but it takes more than five minutes for the instance to start\. CodeDeploy treats this as a timeout\. The service fails all current and future deployments to the instance\.

To address this issue:
+ In general, make sure that all deployments are complete before the instance is terminated or rebooted\. Make sure that all deployments start after the instance has started or been rebooted\. 
+ Deployments can fail if you specify a Windows Server base Amazon Machine Image \(AMI\) for an Amazon EC2 Auto Scaling configuration and use the EC2Config service to set the computer name of the instance\. To fix this issue, in the Windows Server base AMI, on the **General** tab of **EC2 Service Properties**, clear **Set Computer Name**\. After you clear this check box, this behavior is disabled for all new Windows Server Amazon EC2 Auto Scaling instances launched with that Windows Server base AMI\. For Windows Server Amazon EC2 Auto Scaling instances on which this behavior enabled, you do not need to clear this check box\. Simply redeploy failed deployments to those instances after they have been rebooted\.

## Avoid associating multiple deployment groups with a single Amazon EC2 Auto Scaling group<a name="troubleshooting-multiple-depgroups"></a>

As a best practice, you should associate only one deployment group with each Amazon EC2 Auto Scaling group\. 

This is because if Amazon EC2 Auto Scaling scales up an instance that has hooks associated with multiple deployment groups, it sends notifications for all of the hooks at once\. This causes multiple deployments to each instance to start at the same time\. When multiple deployments send commands to the CodeDeploy agent at the same time, the five\-minute timeout between a lifecycle event and either the start of the deployment or the end of the previous lifecycle event might be reached\. If this happens, the deployment fails, even if a deployment process is otherwise running as expected\. 

**Note**  
The default timeout for a script in a lifecycle event is 30 minutes\. You can change the timeout to a different value in the AppSpec file\. For more information, see [Add an AppSpec file for an EC2/On\-Premises deployment](application-revisions-appspec-file.md#add-appspec-file-server)\.

It's not possible to control the order in which deployments occur if more than one deployment attempts to run at the same time\. 

Finally, if deployment to any instance fails, Amazon EC2 Auto Scaling immediately terminates the instance\. When that first instance shuts down, the other running deployments start to fail\. Because CodeDeploy has a one\-hour timeout for the CodeDeploy agent to respond to pending deployments, it can take up to 60 minutes for each instance to time out\. 

For more information about Amazon EC2 Auto Scaling, see [Under the hood: CodeDeploy and Auto Scaling integration](http://aws.amazon.com/blogs/devops/under-the-hood-aws-codedeploy-and-auto-scaling-integration/)\.

## EC2 instances in an Amazon EC2 Auto Scaling group fail to launch and receive the error "Heartbeat Timeout"<a name="troubleshooting-auto-scaling-heartbeat"></a>

An Amazon EC2 Auto Scaling group might fail to launch new EC2 instances, generating a message similar to the following: 

`Launching a new EC2 instance <instance-Id>. Status Reason: Instance failed to complete user's Lifecycle Action: Lifecycle Action with token<token-Id> was abandoned: Heartbeat Timeout`\. 

This message usually indicates one of the following: 
+ The maximum number of concurrent deployments associated with an AWS account was reached\. For more information about deployment limits, see [CodeDeploy limits](limits.md)\. 
+ The Auto Scaling group tried to launch too many EC2 instances too quickly\. The API calls to [RecordLifecycleActionHeartbeat](https://docs.aws.amazon.com/autoscaling/EC2/APIReference/API_RecordLifecycleActionHeartbeat.html) or [CompleteLifecycleAction](https://docs.aws.amazon.com/autoscaling/EC2/APIReference/API_CompleteLifecycleAction.html) for each new instance were throttled\.
+ An application in CodeDeploy was deleted before its associated deployment groups were updated or deleted\.

  When you delete an application or deployment group, CodeDeploy attempts to clean up any Amazon EC2 Auto Scaling hooks associated with it, but some hooks might remain\. If you run a command to delete a deployment group, the leftover hooks are returned in the output\. However, if you run a command to delete an application, the leftover hooks do not appear in the output\.

  Therefore, as a best practice, you should delete all deployment groups associated with an application before you delete the application\. You can use the command output to identify the lifecycle hooks that must be deleted manually\. 

If you receive a “Heartbeat Timeout” error message, you can determine if leftover lifecycle hooks are the cause and resolve the problem by doing the following:

1. Do one of the following:
   + Call the [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command to delete the deployment group associated with the Auto Scaling group that is causing the heartbeat timeout\.
   + Call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command with a non\-null empty list of Auto Scaling group names to detach all CodeDeploy\-managed Auto Scaling lifecycle hooks\.

     For example, enter the following AWS CLI command:

     `aws deploy update-deployment-group --application-name my-example-app --current-deployment-group-name my-deployment-group --auto-scaling-groups`

     As another example, if you are using the CodeDeploy API with Java, call `UpdateDeploymentGroup` and set `autoScalingGroups` to `new ArrayList<String>()`\. This sets `autoScalingGroups` to an empty list and removes the existing list\. Do not use `null`, which is the default, because this leaves `autoScalingGroups` as\-is, which is not what you want\.

   Examine the output of the call\. If the output contains a `hooksNotCleanedUp` structure with a list of Amazon EC2 Auto Scaling lifecycle hooks, there are leftover lifecycle hooks\. 

1. Call the [describe\-lifecycle\-hooks](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-lifecycle-hooks.html) command, specifying the name of the Amazon EC2 Auto Scaling group associated with the EC2 instances that failed to launch\. In the output, look for any of the following:
   + Amazon EC2 Auto Scaling lifecycle hook names that correspond to the `hooksNotCleanedUp` structure you identified in step 1\.
   + Amazon EC2 Auto Scaling lifecycle hook names that contain the name of the deployment group associated with the Auto Scaling group that's failing\.
   + Amazon EC2 Auto Scaling lifecycle hook names that may have caused the heartbeat timeout for the CodeDeploy deployment\.

1. If a hook falls into one of the categories listed in step 2, call the [delete\-lifecycle\-hook](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-lifecycle-hook.html) command to delete it\. Specify the Amazon EC2 Auto Scaling group and lifecycle hook in the call\.
**Important**  
Only delete hooks that are causing problems, as outlined in step 2\. If you delete viable hooks, your deployments might fail, or CodeDeploy might not be able to deploy your application revisions to scaled out EC2 instances\.

1. Call either the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) or [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command with the desired Auto Scaling group names\. CodeDeploy re\-installs the Auto Scaling hooks with new UUIDs\.

**Note**  
If you detach an Auto Scaling group from a CodeDeploy deployment group, any in\-progress deployments to the Auto Scaling group may fail, and new EC2 instances that are scaled out by the Auto Scaling group will not receive your application revisions from CodeDeploy\. To get Auto Scaling working again with CodeDeploy, you'll need to re\-attach the Auto Scaling group to the deployment group and call a new `CreateDeployment` to start a fleet\-wide deployment\.

## Mismatched Amazon EC2 Auto Scaling lifecycle hooks might cause automatic deployments to Amazon EC2 Auto Scaling groups to stop or fail<a name="troubleshooting-auto-scaling-hooks"></a>

Amazon EC2 Auto Scaling and CodeDeploy use lifecycle hooks to determine which application revisions should be deployed to which EC2 instances after they are launched in Amazon EC2 Auto Scaling groups\. Automatic deployments can stop or fail if lifecycle hooks and information about these hooks do not match exactly in Amazon EC2 Auto Scaling and CodeDeploy\.

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

## "The deployment failed because no instances were found for your deployment group" error<a name="troubleshooting-deployment-failed-because-no-instances-found"></a>

Read this section if you see the following CodeDeploy error:

`The deployment failed because no instances were found for your deployment group. Check your deployment group settings to make sure the tags for your EC2 instances or Auto Scaling groups correctly identify the instances you want to deploy to, and then try again.`

Possible causes for this error are:

1. Your deployment group settings include tags for EC2 instances, on\-premises instances, or Auto Scaling groups that are not correct\. To fix this problem, check that your tags are correct and redeploy your application\.

1. Your fleet scaled out after the deployment started\. In this scenario, you’ll see healthy instances in the `InService` state in your fleet, but you’ll also see the error above\. To fix this problem, redeploy your application\.

1. Your Auto Scaling group does not include any instances that are in the `InService` state\. In this scenario, when you try to do a fleet\-wide deployment, the deployment will fail with the error message above because CodeDeploy needs at least one instance to be in the `InService` state\. There are many reasons why you might have no instances in the `InService` state\. A few of them include:
   + You scheduled \(or manually configured\) the Auto Scaling group size to be `0`\.
   + Auto Scaling detected bad EC2 instances \(for example, the EC2 instances had hardware failures\), so canceled them all, leaving none in the `InService` state\.
   + During a scale out event from `0` to `1`, CodeDeploy deployed a previously\-successful revision \(called a *last successful revision*\) that had become unhealthy since the last deployment\. This caused the deployment on the scaled\-out instance to fail, which in turn caused Auto Scaling to cancel the instance, leaving no instances in the `InService` state\.

     If you find that you have no instances in the `InService` state, troubleshoot the problem as described in the following procedure, [To troubleshoot the error if there are no instances in the InService state](#ToTroubleshootIfNoInServiceInstances)\.

**To troubleshoot the error if there are no instances in the InService state**

1. In the Amazon EC2 Console, verify the **Desired Capacity** setting\. If it is zero, set it to a positive number\. Wait for the instance to be `InService`, which means the deployment succeeded\. You have fixed the problem and can skip the remaining steps of this troubleshooting procedure\. For information on setting the **Desired Capacity** setting, see [Setting capacity limits on your Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-capacity-limits.html) in the *Amazon EC2 Auto Scaling User Guide*\.

1. If Auto Scaling keeps attempting to launch new EC2 instances to meet the desired capacity, but can never fulfill the scale out, it is usually due to a failing Auto Scaling lifecycle hook\. Troubleshoot this problem as follows:

   1. To check which Auto Scaling lifecycle hook event is failing, see [Verifying a scaling activity for an Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-verify-scaling-activity.html) in the Amazon EC2 Auto Scaling User Guide\.

   1. If the failing hook's name is `CodeDeploy-managed-automatic-launch-deployment-hook-DEPLOYMENT_GROUP_NAME`, go to CodeDeploy, find the deployment group, and find the failed deployment that was started by Auto Scaling\. Then investigate why the deployment failed\.

   1. If you understand why the deployment failed \(for example, CloudWatch alarms were occurring\) and you can fix the problem without changing the revision, then do so now\.

   1. If after investigation, you determine that CodeDeploy’s *last successful revision* is no longer healthy, and there are zero healthy instances in your Auto Scaling group, you are in a deployment deadlock scenario\. To solve this issue, you must fix the \(bad\) CodeDeploy revision by temporarily removing CodeDeploy’s lifecycle hook from the Auto Scaling group, and then reinstalling the hook and redeploying a new \(good\) revision\. For instructions, see:
      + [To fix the deployment deadlock issue (CLI)](#ToFixDeployDeadlockCLI)
      + [To fix the deployment deadlock issue (console)](#ToFixDeployDeadlockConsole)

**To fix the deployment deadlock issue \(CLI\)**

1. \(Optional\) Block your CI/CD pipelines that are causing the CodeDeploy error so that unexpected deployments do not occur while you’re fixing this problem\.

1. Take note of your current Auto Scaling **DesiredCapacity** setting:

   `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name ASG_NAME`

   You may need to scale back to this number at the end of this procedure\.

1. Set your Auto Scaling **DesiredCapacity** setting to `1`\. This is optional if your desired capacity was greater than `1` to begin with\. By decreasing it to `1`, the instance will take less time to provision and deploy later, which speeds up troubleshooting\. If your Auto Scaling desired capacity was originally set to `0`, you must increase it to `1`\. This is mandatory\. 

   aws autoscaling set\-desired\-capacity \-\-auto\-scaling\-group\-name *ASG\_NAME* \-\-desired\-capacity 1 
**Note**  
The remaining steps of this procedure assume you have set your **DesiredCapacity** to `1`\.

   At this point, Auto Scaling attempts to scale to one instance\. Then, because the hook that CodeDeploy added is still present, CodeDeploy tries to deploy, the deployment fails, Auto Scaling cancels the instance, and Auto Scaling tries to re\-launch an instance to reach desired capacity of one, which again fails\. You are in a cancel\-relaunch loop\.

1. Deregister the Auto Scaling group from the deployment group:
**Warning**  
The following command will launch a new EC2 instance with no software on it\. Before running the command, make sure that an Auto Scaling `InService` instance running no software is acceptable\. For example, make sure your load balancer is not sending traffic to this host without software\.
**Important**  
Use the CodeDeploy command shown below to remove the hook\. Do not remove the hook through the Auto Scaling service, because the removal will not be recognized by CodeDeploy\. 

   `aws deploy update-deployment-group --application-name APPLICATION_NAME --current-deployment-group-name DEPLOYMENT_GROUP_NAME --auto-scaling-groups`

   After running this command, the following occurs:

   1. CodeDeploy deregisters the Auto Scaling group from the deployment group\.

   1. CodeDeploy removes the Auto Scaling lifecycle hook from the Auto Scaling group\.

   1. Since the hook that was causing a failed deployment is no longer present, Auto Scaling cancels the existing EC2 instance and immediately launches a new one to scale to the desired capacity\. The new instance should soon move into `InService` state\. The new instance does not include software\.

1. Wait for the EC2 instance to enter the `InService` state\. To verify its state, use the following command:

   `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names ASG_NAME --query AutoScalingGroups[0].Instances[*].LifecycleState`

1. Add the hook back to the EC2 instance:
**Important**  
Use the CodeDeploy command shown below to add the hook\. Do not use the Auto Scaling service to add the hook, because the addition will not be recognized by CodeDeploy\. 

   `aws deploy update-deployment-group --application-name APPLICATION_NAME --current-deployment-group-name DEPLOYMENT_GROUP_NAME --auto-scaling-groups ASG_NAME`

   After running this command, the following occurs:

   1. CodeDeploy re\-installs the Auto Scaling lifecycle hook to the EC2 instance

   1. CodeDeploy reregisters the Auto Scaling group with the deployment group\.

1. Create a fleet\-wide deployment with the Amazon S3 or GitHub revision that you know is healthy and want to use\.

   For example, if the revision is a \.zip file in an Amazon S3 bucket called `my-revision-bucket` with an object key of `httpd_app.zip`, enter the following command:

   `aws deploy create-deployment --application-name APPLICATION_NAME --deployment-group-name DEPLOYMENT_GROUP_NAME --revision "revisionType=S3,s3Location={bucket=my-revision-bucket,bundleType=zip,key=httpd_app.zip}"`

   Since there is now one `InService` instance in the Auto Scaling group, this deployment should work, and you should no longer see the error *The deployment failed because no instances were found for your deployment group*\.

1. After the deployment succeeds, scale your Auto Scaling group back out to the original capacity, if you previously scaled it in:

   `aws autoscaling set-desired-capacity --auto-scaling-group-name ASG_NAME --desired-capacity ORIGINAL_CAPACITY`

**To fix the deployment deadlock issue \(console\)**

1. \(Optional\) Block your CI/CD pipelines that are causing the CodeDeploy error so that unexpected deployments do not occur while you’re fixing this problem\.

1. Go to the Amazon EC2 Console, and take note of your Auto Scaling **Desired capacity** setting\. You may need to scale back to this number at the end of this procedure\. For information on finding this setting, see [Setting capacity limits on your Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-capacity-limits.html)\.

1. Set the desired number of EC2 instances to `1`:

   This is optional if your desired capacity was greater than `1` to begin with\. By decreasing it to `1`, the instance will take less time to provision and deploy later, which speeds up troubleshooting\. If your Auto Scaling **Desired capacity** was originally set to `0`, you must increase it to `1`\. This is mandatory\. 
**Note**  
The remaining steps of this procedure assume you have set your **Desired capacity** to `1`\.

   1. Open the Amazon EC2 Auto Scaling console at [https://console\.aws\.amazon\.com/ec2autoscaling/](https://console.aws.amazon.com/ec2autoscaling/)\.

   1. Choose the appropriate Region\.

   1. Go to the problematic Auto Scaling group\.

   1. In **Group details**, choose **Edit**\.

   1. Set **Desired capacity** to **1**\.

   1. Choose **Update**\.

1. Deregister the Auto Scaling group from the deployment group:
**Warning**  
The following sub\-steps will launch a new EC2 instance with no software on it\. Before running the command, make sure that an Auto Scaling `InService` instance running no software is acceptable\. For example, make sure your load balancer is not sending traffic to this host without software\.

   1. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy/](https://console.aws.amazon.com/codedeploy/)\.

   1. Choose the appropriate Region\.

   1. In the navigation pane, choose **Applications**\.

   1. Choose the name of your CodeDeploy application\.

   1. Choose the name of your CodeDeploy deployment group\.

   1. Choose **Edit**\.

   1. In **Environment configuration**, deselect **Amazon EC2 Auto Scaling groups**\.
**Note**  
The console does not allow you to save the configuration if there is no environment configuration defined\. To bypass the check, temporarily add a tag of `EC2` or `On-premises` that you know won't resolve to any hosts\. To add a tag, select **Amazon EC2 instances** or **On\-premises instance**, and add a tag **Key** of **EC2** or **On\-premises**\. You can leave the tag **Value** empty\.

   1. Choose **Save changes**\.

      After completing these sub\-steps, the following occurs:

      1. CodeDeploy deregisters the Auto Scaling group from the deployment group\.

      1. CodeDeploy removes the Auto Scaling lifecycle hook from the Auto Scaling group\.

      1. Since the hook that was causing a failed deployment is no longer present, Auto Scaling cancels the existing EC2 instance and immediately launches a new one to scale to the desired capacity\. The new instance should soon move into `InService` state\. The new instance does not include software\.

1. Wait for the EC2 instance to enter the `InService` state\. To verify its state:

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the navigation pane, choose **Auto Scaling Groups**\.

   1. Choose your Auto Scaling group\.

   1. In the content pane, choose the **Instance Management** tab\.

   1. Under **Instances**, make sure that the **Lifecycle** column indicates **InService** next to the instance\.

1. Reregister the Auto Scaling group with the CodeDeploy deployment group using the same method you used to remove it:

   1. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy/](https://console.aws.amazon.com/codedeploy/)\.

   1. Choose the appropriate Region\.

   1. In the navigation pane, choose **Applications**\.

   1. Choose the name of your CodeDeploy application\.

   1. Choose the name of your CodeDeploy deployment group\.

   1. Choose **Edit**\.

   1. In **Environment configuration**, select **Amazon EC2 Auto Scaling groups** and select your Auto Scaling group from the list\.

   1. Under **Amazon EC2 instances** or **On\-premises instances**, find the tag you added and remove it\.

   1. Deselect the check box next to **Amazon EC2 instances** or **On\-premises instances**\. 

   1. Choose **Save changes**\. 

   This configuration re\-installs the lifecycle hook into the Auto Scaling group\.

1. Create a fleet\-wide deployment with the Amazon S3or GitHub revision that you know is healthy and want to use\. 

   For example, if the revision is a \.zip file in an Amazon S3 bucket called `my-revision-bucket` with an object key of `httpd_app.zip`, do the following:

   1. In the CodeDeploy console, in the **Deployment Group** page, choose **Create deployment**\.

   1. For **Revision type**, choose **My application is stored in Amazon S3**\.

   1. For **Revision location**, choose `s3://my-revision-bucket/httpd_app.zip`\.

   1. For **Revision file type**, choose `.zip`\.

   1. Choose **Create deployment**\.

   Since there is now one `InService` instance in the Auto Scaling group, this deployment should work, and you should no longer see the error *The deployment failed because no instances were found for your deployment group*\.

1. After the deployment succeeds, scale your Auto Scaling group back out to the original capacity, if you previously scaled it in:

   1. Open the Amazon EC2 Auto Scaling console at [https://console\.aws\.amazon\.com/ec2autoscaling/](https://console.aws.amazon.com/ec2autoscaling/)\.

   1. Choose the appropriate Region\.

   1. Go to your Auto Scaling group\.

   1. In **Group details**, choose **Edit**\.

   1. Set **Desired capacity** back to its original value\.

   1. Choose **Update**\.
# Integrating CodeDeploy with Amazon EC2 Auto Scaling<a name="integrations-aws-auto-scaling"></a>

CodeDeploy supports Amazon EC2 Auto Scaling, an AWS service that can launch Amazon EC2 instances automatically according to conditions you define\. These conditions can include limits exceeded in a specified time interval for CPU utilization, disk reads or writes, or inbound or outbound network traffic\. Amazon EC2 Auto Scaling terminates the instances when they are no longer needed\. For more information, see [What is Amazon EC2 Auto Scaling?](https://docs.aws.amazon.com/autoscaling/latest/userguide/WhatIsAutoScaling.html)\.

When new Amazon EC2 instances are launched as part of an Amazon EC2 Auto Scaling group, CodeDeploy can deploy your revisions to the new instances automatically\. You can also coordinate deployments in CodeDeploy with Amazon EC2 Auto Scaling instances registered with Elastic Load Balancing load balancers\. For more information, see [Integrating CodeDeploy with Elastic Load Balancing](integrations-aws-elastic-load-balancing.md) and [Set up a load balancer in Elastic Load Balancing for CodeDeploy Amazon EC2 deployments](deployment-groups-create-load-balancer.md)\.

**Note**  
Be aware that you might encounter issues if you associate multiple deployment groups with a single Amazon EC2 Auto Scaling group\. If one deployment fails, for example, the instance will begin to shut down, but the other deployments that were running can take an hour to time out\. For more information, see [Avoid associating multiple deployment groups with a single Amazon EC2 Auto Scaling group](troubleshooting-auto-scaling.md#troubleshooting-multiple-depgroups) and [Under the hood: CodeDeploy and Amazon EC2 Auto Scaling integration](http://aws.amazon.com/blogs/devops/under-the-hood-aws-codedeploy-and-auto-scaling-integration/)\.

**Topics**
+ [Deploying CodeDeploy applications to Amazon EC2 Auto Scaling groups](#integrations-aws-auto-scaling-deploy)
+ [Amazon EC2 Auto Scaling behaviors with CodeDeploy](#integrations-aws-auto-scaling-behaviors)
+ [Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling](#integrations-aws-auto-scaling-custom-ami)

## Deploying CodeDeploy applications to Amazon EC2 Auto Scaling groups<a name="integrations-aws-auto-scaling-deploy"></a>

To deploy a CodeDeploy application revision to an Amazon EC2 Auto Scaling group:

1. Create or locate an IAM instance profile that allows the Amazon EC2 Auto Scaling group to work with Amazon S3\.
**Note**  
You can also use CodeDeploy to deploy revisions from GitHub repositories to Amazon EC2 Auto Scaling groups\. Although Amazon EC2 instances still require an IAM instance profile, the profile doesn't need any additional permissions to deploy from a GitHub repository\. For more information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md)\.

1. Create or use an Amazon EC2 Auto Scaling group, specifying the IAM instance profile in your launch configuration or template\. For more information, see [IAM role for applications that run on Amazon EC2 instances](https://docs.aws.amazon.com/autoscaling/ec2/userguide/us-iam-role.html)\.

1. Create or locate a service role that allows CodeDeploy to create a deployment group that contains the Amazon EC2 Auto Scaling group\.

1. Create a deployment group with CodeDeploy, specifying the Amazon EC2 Auto Scaling group name and service role\.

1. Use CodeDeploy to deploy your revision to the deployment group that contains the Amazon EC2 Auto Scaling group\.

For more information, see [Tutorial: Use CodeDeploy to deploy an application to an Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group.md)\.

## Amazon EC2 Auto Scaling behaviors with CodeDeploy<a name="integrations-aws-auto-scaling-behaviors"></a>

### How CodeDeploy names Amazon EC2 Auto Scaling groups<a name="integrations-aws-auto-scaling-behaviors-naming"></a>

During blue/green deployments on an EC2/On\-Premises compute platform, you have two options for adding instances to your replacement \(green\) environment:
+  Use instances that already exist or that you create manually\. 
+  Use settings from an Amazon EC2 Auto Scaling group that you specify to define and create instances in a new Amazon EC2 Auto Scaling group\. 

 If you choose the second option, CodeDeploy provisions a new Amazon EC2 Auto Scaling group for you\. It uses the following convention to name the group: 

```
CodeDeploy_deployment_group_name_deployment_id
```

For example, if a deployment with ID `10` deploys a deployment group named `alpha-deployments`, the provisioned Amazon EC2 Auto Scaling group is named `CodeDeploy_alpha-deployments_10`\. For more information, see [Create a deployment group for an EC2/On\-Premises blue/green deployment \(console\)](deployment-groups-create-blue-green.md) and [GreenFleetProvisioningOption](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_GreenFleetProvisioningOption.html)\.

### Execution order of custom lifecycle hook events<a name="integrations-aws-auto-scaling-behaviors-hook-order"></a>

You can add your own lifecycle hooks to Amazon EC2 Auto Scaling groups to which CodeDeploy deploys\. However, the order in which those custom lifecycle hook events are executed cannot be predetermined in relation to CodeDeploy default deployment lifecycle events\. For example, if you add a custom lifecycle hook named `ReadyForSoftwareInstall` to an Amazon EC2 Auto Scaling group, you cannot know beforehand whether it will be executed before the first, or after the last, CodeDeploy default deployment lifecycle event\.

To learn how to add custom lifecycle hooks to an Amazon EC2 Auto Scaling group, see [Adding lifecycle hooks](https://docs.aws.amazon.com/autoscaling/latest/userguide/lifecycle-hooks.html#adding-lifecycle-hooks)\.

### Scale\-up events during a deployment<a name="integrations-aws-auto-scaling-behaviors-mixed-environment"></a>

If an Amazon EC2 Auto Scaling scale\-up event occurs while a deployment is underway, the new instances will be updated with the application revision that was most recently deployed, not the application revision that is currently being deployed\. If the deployment succeeds, the old instances and the newly scaled\-up instances will be hosting different application revisions\.

To resolve this problem after it occurs, you can redeploy the newer application revision to the affected deployment groups\.

To avoid this problem, we recommend suspending the Amazon EC2 Auto Scaling scale\-up processes while deployments are taking place\. You can do this through a setting in the common\_functions\.sh script that is used for load balancing with CodeDeploy\. If `HANDLE_PROCS=true`, the following Amazon EC2 Auto Scaling events are suspended automatically during the deployment process:
+ AZRebalance
+ AlarmNotification
+ ScheduledActions
+ ReplaceUnhealthy

**Important**  
Only the CodeDeployDefault\.OneAtATime deployment configuration supports this functionality\. If you are using other deployment configurations, the deployment group might still have different application revisions applied to its instances\.

For more information about using `HANDLE_PROCS=true` to avoid deployment problems when using Amazon EC2 Auto Scaling, see [Important notice about handling AutoScaling processes](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb#important-notice-about-handling-autoscaling-processes) in [aws\-codedeploy\-samples](https://github.com/awslabs/aws-codedeploy-samples) on GitHub\.

### Order of events in AWS CloudFormation cfn\-init scripts<a name="integrations-aws-auto-scaling-behaviors-event-order"></a>

If you use `cfn-init` \(or `cloud-init`\) to run scripts on newly provisioned Linux\-based instances, your deployments might fail unless you strictly control the order of events that occur after the instance starts\.

That order must be:

1. The newly provisioned instance starts\.

1. All `cfn-init` bootstrapping scripts run to completion\.

1. The CodeDeploy agent starts\.

1. The latest application revision is deployed to the instance\.

If the order of events is not carefully controlled, the CodeDeploy agent might start a deployment before all the scripts have finished running\. 

To control the order of events, use one of these best practices: 
+ Install the CodeDeploy agent through a `cfn-init` script, placing it after all other scripts\.
+ Include the CodeDeploy agent in a custom AMI and use a `cfn-init` script to start it, placing it after all other scripts\.

For information about using `cfn-init`, see [cfn\-init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-init.html) in *[AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)*\.

## Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling<a name="integrations-aws-auto-scaling-custom-ami"></a>

You have two options for specifying the base AMI to use when new Amazon EC2 instances are launched in an Amazon EC2 Auto Scaling group:
+ You can specify a base custom AMI that already has the CodeDeploy agent installed\. Because the agent is already installed, this option launches new Amazon EC2 instances more quickly than the other option\. However, this option provides a greater likelihood that initial deployments of Amazon EC2 instances will fail, especially if the CodeDeploy agent is out of date\. If you choose this option, we recommend you regularly update the CodeDeploy agent in your base custom AMI\.
+ You can specify a base AMI that doesn't have the CodeDeploy agent installed and have the agent installed as each new instance is launched in an Amazon EC2 Auto Scaling group\. Although this option launches new Amazon EC2 instances more slowly than the other option, it provides a greater likelihood that initial deployments of instances will succeed\. This option uses the most recent version of the CodeDeploy agent\.
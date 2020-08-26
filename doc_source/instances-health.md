# CodeDeploy instance health<a name="instances-health"></a>

CodeDeploy monitors the health status of the instances in a deployment group\. It fails deployments if the number of healthy instances falls below the minimum number of healthy instances that have been specified for the deployment group during a deployment\. For example, if 85% of instances must remain healthy during a deployment, and the deployment group contains 10 instances, the overall deployment will fail if deployment to even a single instance fails\. This is because when an instance is taken offline so the latest application revision can be installed, the available healthy instance counts already drops to 90%\. A failed instance plus another offline instance would mean that only 80% of instances are healthy and available\. CodeDeploy will fail the overall deployment\.

It is important to remember that for the overall deployment to succeed, the following must be true:
+ CodeDeploy is able to deploy to each instance in the deployment\.
+ Deployment to at least one instance must succeed\. This means that even if the minimum healthy hosts value is 0, deployment to at least one instance must succeed \(that is, at least one instance must be healthy\) for the overall deployment to succeed\.

The required minimum number of healthy instances is defined as part of a deployment configuration\. 

**Important**  
During a blue/green deployment, the deployment configuration and minimum healthy hosts value apply to instances in the replacement environment, not those in the original environment\. However, when instances in the original environment are deregistered from the load balancer, the overall deployment is marked as Failed if even a single original instance fails to be deregistered successfully\.

CodeDeploy provides three default deployment configurations that have commonly used minimum healthy host values:


| Default deployment configuration name | Predefined minimum healthy hosts value | 
| --- | --- | 
| CodeDeployDefault\.OneAtATime | 99% | 
| CodeDeployDefault\.HalfAtATime | 50% | 
| CodeDeployDefault\.AllAtOnce | 0 | 

You'll find more information about default deployment configurations in [Working with deployment configurations in CodeDeploy](deployment-configurations.md)\.

You can create custom deployment configurations in CodeDeploy to define your own minimum healthy host values\. You can define these values as whole numbers or percentages when using the following operations:
+ As `minimum-healthy-hosts` when you use the [create\-deployment\-config](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-config.html) command in the AWS CLI\.
+ As `Value` in the [MinimumHealthyHosts](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_MinimumHealthyHosts.html) data type in the CodeDeploy API\.
+ As `MinimumHealthyHosts` when you use [AWS::CodeDeploy::DeploymentConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codedeploy-deploymentconfig.html) in an AWS CloudFormation template\.

**Topics**
+ [Health status](#instances-health-status)
+ [Minimum healthy instances and deployments](#minimum-healthy-hosts)

## Health status<a name="instances-health-status"></a>

CodeDeploy assigns two health status values to each instance: *revision health* and *instance health*\.

Revision health  
Revision health is based on the application revision currently installed on the instance\. It has the following status values:  
+ Current: The revision installed on the instance matches the revision for the deployment group's last successful deployment\.
+ Old: The revision installed on the instance matches an older version of the application\.
+ Unknown: The application revision has not been installed successfully on the instance\.

Instance health  
Instance health is based on whether deployments to an instance have been successful\. It has the following values:  
+ Healthy: The last deployment to the instance was successful\.
+ Unhealthy: The attempt to deploy a revision to the instance failed, or a revision has not yet been deployed to the instance\.

CodeDeploy uses revision health and instance health to schedule the deployment to the deployment group's instances in the following order:

1. Unhealthy instance health\.

1. Unknown revision health\.

1. Old revision health\.

1. Current revision health\.

If the overall deployment succeeds, the revision is updated and the deployment group's health status values are updated to reflect the latest deployment\.
+ All current instances that had a successful deployment remain current\. Otherwise, they become unknown\.
+ All old or unknown instances that had a successful deployment become current\. Otherwise, they remain old or unknown\.
+ All healthy instances that had a successful deployment remain healthy\. Otherwise, they become unhealthy\.
+ All unhealthy instances that had a successful deployment become healthy\. Otherwise, they remain unhealthy\.

If the overall deployment fails or is stopped:
+ Each instance to which CodeDeploy attempted to deploy the application revision has its instance health set to healthy or unhealthy, depending on whether the deployment attempt for that instance succeeded or failed\.
+ Each instance to which CodeDeploy did not attempt to deploy the application revision retains its current instance health value\.
+ The deployment group's revision remains the same\.

## Minimum healthy instances and deployments<a name="minimum-healthy-hosts"></a>

CodeDeploy allows you to specify a minimum number of healthy instances for the deployment for two main purposes:
+ To determine whether the overall deployment succeeds or fails\. Deployment succeeds if the application revision was successfully deployed to at least the minimum number of healthy instances\.
+ To determine the number of instances that must be healthy during a deployment to allow the deployment to proceed\.

You can specify the minimum number of healthy instances for your deployment group as a number of instances or as a percentage of the total number of instances\. If you specify a percentage, then at the start of the deployment, CodeDeploy converts the percentage to the equivalent number of instances, rounding up any fractional instances\.

CodeDeploy tracks the health status of the deployment group's instances during the deployment process and uses the deployment's specified minimum number of healthy instances to determine whether to continue the deployment\. The basic principle is that a deployment must never cause the number of healthy instances to fall below the minimum number you have specified\. The one exception to this rule is when a deployment group initially has less than the specified minimum number of healthy instances\. In that case, the deployment process does not reduce the number of healthy instances any further\.

**Note**  
CodeDeploy will attempt to deploy to all instances in a deployment group, even those that are currently in a Stopped state\. In the minimum healthy host calculation, a stopped instance has the same impact as a failed instance\. To resolve deployment failures due to too many stopped instances, either restart instances or change their tags to exclude them from the deployment group\.

CodeDeploy starts the deployment process by attempting to deploy the application revision to the deployment group's unhealthy instances\. For each successful deployment, CodeDeploy changes the instance's health status to healthy and adds it to the deployment group's healthy instances\. CodeDeploy then compares the current number of healthy instances to the specified minimum number of healthy instances\.
+ If the number of healthy instances is less than or equal to the specified minimum number of healthy instances, CodeDeploy cancels the deployment to ensure the number of healthy instances doesn't decrease with more deployments\.
+ If the number of healthy instances is greater than the specified minimum number of healthy instances by at least one, CodeDeploy deploys the application revision to the original set of healthy instances\.

If a deployment to a healthy instance fails, CodeDeploy changes that instance's health status to unhealthy\. As the deployment progresses, CodeDeploy updates the current number of healthy instances and compares it to the specified minimum number of healthy instances\. If the number of healthy instances falls to the specified minimum number at any point in the deployment process, CodeDeploy stops the deployment\. This practice prevents the possibility the next deployment will fail, dropping the number of healthy instances below the specified minimum number\. 

**Note**  
Make sure the minimum number of healthy instances you specify is less than the total number of instances in the deployment group\. If you specify a percentage value, remember it will be rounded up\. Otherwise, when the deployment starts, the number of healthy instances will already be less than or equal to the specified minimum number of healthy instances, and CodeDeploy will immediately fail the overall deployment\.

CodeDeploy also uses the specified minimum number of healthy instances and the actual number of healthy instances to determine whether and how to deploy the application revision to multiple instances\. By default, CodeDeploy deploys the application revision to as many instances as it can without any risk of having the number of healthy instances fall below the specified minimum number of healthy instances\. For example:
+ If your deployment group has 10 instances and you set the minimum healthy instances number to 9, CodeDeploy deploys to one instance at a time\.
+ If your deployment group has 10 instances and you set the minimum healthy instances number to 0, CodeDeploy deploys to every instance at the same time\.

**Examples**

The following examples assume a deployment group with 10 instances\.

Minimum healthy instances: 95%  
CodeDeploy rounds the minimum healthy instances number up to 10 instances, which equals the number of healthy instances\. The overall deployment immediately fails without deploying the revision to any instances\.

Minimum healthy instances: 9  
CodeDeploy deploys the revision to one instance at a time\. If deployment to any of the instances fails, CodeDeploy immediately fails the overall deployment because the number of healthy instances equals the minimum healthy instances number\. The exception to this rule is that if the last instance fails, the deployment still succeeds\.  
CodeDeploy continues the deployment, one instance at a time, until any deployment fails or the overall deployment is complete\. If all 10 deployments succeed, the deployment group now has 10 healthy instances\. 

Minimum healthy instances: 8  
CodeDeploy deploys the revision to two instances at a time\. If two of these deployments fail, CodeDeploy immediately fails the overall deployment\. The exception to this rule is that if the last instance is the second to fail, the deployment still succeeds\.

Minimum healthy instances: 0  
CodeDeploy deploys the revision to the entire deployment group at once\. At least one deployment to an instance must succeed for the overall deployment to succeed\. If 0 instances are healthy, then the deployment fails\. This is because of the requirement that in order to mark an overall deployment as successful, at least one instance must be healthy when the the overall deployment is completed, even if the minimum healthy instances value is 0\.
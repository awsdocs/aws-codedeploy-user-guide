# Monitoring Deployments with CloudWatch Alarms in AWS CodeDeploy<a name="monitoring-create-alarms"></a>

You can create a CloudWatch alarm for an instance or Amazon EC2 Auto Scaling group you are using in your AWS CodeDeploy operations\. An alarm watches a single metric over a time period you specify and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.  CloudWatch alarms do not invoke actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\.

Using native CloudWatch alarm functionality, you can specify any of the actions supported by CloudWatch when an instance you are using in a deployment fails, such as sending an Amazon SNS notification or stopping, terminating, rebooting, or recovering an instance\. For your AWS CodeDeploy operations, you can configure a deployment group to stop a deployment whenever any CloudWatch alarm you associate with the deployment group is activated\. 

You can associate up to ten CloudWatch alarms with an AWS CodeDeploy deployment group\. If any of the specified alarms are activated, the deployment stops, and the status is updated to Stopped\. To use this option, you must grant CloudWatch permissions to your AWS CodeDeploy service role\.

For information about setting up CloudWatch alarms in the CloudWatch console, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

For information about associating a CloudWatch alarm with a deployment group in AWS CodeDeploy, see [Create a Deployment Group with AWS CodeDeploy](deployment-groups-create.md) and [Change Deployment Group Settings with AWS CodeDeploy](deployment-groups-edit.md)\.

**Topics**
+ [Grant CloudWatch Permissions to an AWS CodeDeploy Service Role](monitoring-create-alarms-grant-permissions.md)
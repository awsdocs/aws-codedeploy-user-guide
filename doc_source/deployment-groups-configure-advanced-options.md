# Configure advanced options for a deployment group<a name="deployment-groups-configure-advanced-options"></a>

When you create or update a deployment group, you can configure a number of options to provide more control and oversight over the deployments for that deployment group\.

Use the information on this page to help you configure advanced options when you work with deployment groups in the following topics: 
+ [Create an application with CodeDeploy](applications-create.md)
+ [Create a deployment group with CodeDeploy](deployment-groups-create.md)
+ [Change deployment group settings with CodeDeploy](deployment-groups-edit.md)

**Amazon SNS notification triggers**: You can add triggers to a CodeDeploy deployment group to receive notifications about events related to deployments in that deployment group\. These notifications are sent to recipients who are subscribed to an Amazon SNS topic you have made part of the trigger's action\. 

You must have already set up the Amazon SNS topic to which this trigger will point, and CodeDeploy must have permission to publish to the topic from this deployment group\. If you have not yet completed these setup steps, you can add triggers to the deployment group later\. 

If you want to create a trigger now to receive notifications about deployment events in the deployment group for this application, choose **Create trigger**\. 

If your deployment is to an Amazon EC2 instance, you can create notifications for and receive notifications about instances\.

For more information, see [Monitoring deployments with Amazon SNS event notifications](monitoring-sns-event-notifications.md)\.

**Amazon CloudWatch alarms**: You can create a CloudWatch alarm that watches a single metric over a time period you specify and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\. For an Amazon EC2 deployment, you can create an alarm for an instance or Amazon EC2 Auto Scaling group that you are using in your CodeDeploy operations\. For an AWS Lambda and an Amazon ECS deployment, you can create an alarm for errors in a Lambda function\.

You can configure a deployment to stop when an Amazon CloudWatch alarm detects that a metric has fallen below or exceeded a defined threshold\.

You must have already created the alarm in CloudWatch before you can add it to a deployment group\.

1. To add alarm monitoring to the deployment group, in **Alarms**, choose **Add alarm**\. 

1. Enter the name of a CloudWatch alarm you have already set up to monitor this deployment\.

   You must enter the CloudWatch alarm exactly as it was created in CloudWatch\. To view a list of alarms, open the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/), and then choose **ALARM**\.

Additional options:
+ If you want deployments to proceed without taking into account alarms you have added, choose **Ignore alarm configuration**\.

  This choice is useful when you want to temporarily deactivate alarm monitoring for a deployment group without having to add the same alarms again later\.
+ \(Optional\) If you want deployments to proceed in the event that CodeDeploy is unable to retrieve alarm status from Amazon CloudWatch, choose **Continue deployments even if alarm status is unavailable**\.
**Note**  
This option corresponds to ignorePollAlarmFailure in the [AlarmConfiguration](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_AlarmConfiguration.html) object in the CodeDeploy API\. 

For more information, see [Monitoring deployments with CloudWatch alarms in CodeDeploy](monitoring-create-alarms.md)\.

**Automatic rollbacks**: You can configure a deployment group or deployment to automatically roll back when a deployment fails or when a monitoring threshold you specify is met\. In this case, the last known good version of an application revision is deployed\. You can configure optional settings for a deployment group when you use the console to create an application, create a deployment group, or update a deployment group\. When you create a new deployment, you can also choose to override the automatic rollback configuration that were specified for the deployment group\. 
+ You can enable deployments to roll back to the most recent known good revision when something goes wrong by choosing one or both of the following:
  + **Roll back when a deployment fails**\. CodeDeploy will redeploy the last known good revision as a new deployment\.
  + **Roll back when alarm thresholds are met**\. If you added an alarm to this application in the previous step, CodeDeploy will redeploy the last known good revision when one or more of the specified alarms is activated\.
**Note**  
To temporarily ignore a rollback configuration, choose **Disable rollbacks**\. This choice is useful when you want to temporarily disable automatic rollbacks without having to set up the same configuration again later\.

  For more information, see [Redeploy and roll back a deployment with CodeDeploy](deployments-rollback-and-redeploy.md)\.
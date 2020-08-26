# Monitoring deployments in CodeDeploy<a name="monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of CodeDeploy and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. Before you start monitoring CodeDeploy, however, you should create a monitoring plan that includes answers to the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

The next step is to establish a baseline for normal CodeDeploy performance in your environment, by measuring performance at various times and under different load conditions\. As you monitor CodeDeploy, store historical monitoring data so that you can compare it with current performance data, identify normal performance patterns and performance anomalies, and devise methods to address issues\.

For example, if you're using CodeDeploy, you can monitor the status of deployments and target instances\. When deployments or instances fail, you might need to reconfigure an application specification file, reinstall or update the CodeDeploy agent, update settings in an application or deployment group, or make changes to instance settings or an AppSpec file\.

To establish a baseline, you should, at a minimum, monitor the following items:
+ Deployment events and status
+ Instance events and status

## Automated monitoring tools<a name="monitoring_automated_tools"></a>

AWS provides various tools that you can use to monitor CodeDeploy\. You can configure some of these tools to do the monitoring for you, while some of the tools require manual intervention\. We recommend that you automate monitoring tasks as much as possible\.

You can use the following automated monitoring tools to watch CodeDeploy and report when something is wrong:
+ **Amazon CloudWatch Alarms** – Watch a single metric over a time period that you specify, and perform one or more actions based on the value of the metric relative to a given threshold over a number of time periods\. The action is a notification sent to an Amazon Simple Notification Service \(Amazon SNS\) topic or Amazon EC2 Auto Scaling policy\. CloudWatch alarms do not invoke actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\. For more information, see [Monitoring deployments with Amazon CloudWatch tools](monitoring-cloudwatch.md)\.

  For information about updating your service role to work with CloudWatch alarm monitoring, see [Grant CloudWatch permissions to a CodeDeploy service role](monitoring-create-alarms-grant-permissions.md)\. For information about adding CloudWatch alarm monitoring to your CodeDeploy operations, see [Create an application with CodeDeploy](applications-create.md), [Create a deployment group with CodeDeploy](deployment-groups-create.md), or [Change deployment group settings with CodeDeploy](deployment-groups-edit.md)\.
+ **Amazon CloudWatch Logs** – Monitor, store, and access your log files from AWS CloudTrail or other sources\. For more information, see [Monitoring Log Files](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.

  For information about using the CloudWatch console to view CodeDeploy logs, see [View CodeDeploy logs in CloudWatch Logs console](http://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)\.
+ **Amazon CloudWatch Events** – Match events and route them to one or more target functions or streams to make changes, capture state information, and take corrective action\. For more information, see [What is Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch User Guide*\.

  For information about using CloudWatch Events in your CodeDeploy operations, see [Monitoring deployments with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)\.
+ **AWS CloudTrail Log Monitoring** – Share log files between accounts, monitor CloudTrail log files in real time by sending them to CloudWatch Logs, write log processing applications in Java, and validate that your log files have not changed after delivery by CloudTrail\. For more information, see [Working with CloudTrail Log Files](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-working-with-log-files.html) in the *AWS CloudTrail User Guide*\. 

  For information about using CloudTrail with CodeDeploy, see [Monitoring deployments with AWS CloudTrail](monitoring-cloudtrail.md)\.
+ **Amazon Simple Notification Service** — Configure event\-driven triggers to receive SMS or email notifications about deployment and instance events, such as success or failure\. For more information, see [Create a topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) and [What is Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

  For information about setting up Amazon SNS notifications for CodeDeploy, see [Monitoring deployments with Amazon SNS event notifications](monitoring-sns-event-notifications.md)\.

## Manual monitoring tools<a name="monitoring_manual_tools"></a>

Another important part of monitoring CodeDeploy involves manually monitoring those items that the CloudWatch alarms don't cover\. The CodeDeploy, CloudWatch, and other AWS console dashboards provide an at\-a\-glance view of the state of your AWS environment\. We recommend that you also check the log files on CodeDeploy deployments\.
+ CodeDeploy console shows:
  + The status of deployments
  + The date and time of each last attempted and last successful deployment of a revision
  + The number of instances that succeeded, failed, were skipped, or are in progress in a deployment
  + The status of on\-premises instances
  + The date and time when on\-premises instances were registered or deregistered
+ CloudWatch home page shows:
  + Current alarms and status
  + Graphs of alarms and resources
  + Service health status

  In addition, you can use CloudWatch to do the following: 
  + Create [customized dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CloudWatch_Dashboards.html) to monitor the services you care about
  + Graph metric data to troubleshoot issues and discover trends
  + Search and browse all your AWS resource metrics
  + Create and edit alarms to be notified of problems

**Topics**
+ [Monitoring Deployments with Amazon CloudWatch Tools](monitoring-cloudwatch.md)
+ [Monitoring Deployments with AWS CloudTrail](monitoring-cloudtrail.md)
+ [Monitoring Deployments with Amazon SNS Event Notifications](monitoring-sns-event-notifications.md)
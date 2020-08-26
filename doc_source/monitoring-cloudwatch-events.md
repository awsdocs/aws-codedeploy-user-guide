# Monitoring deployments with Amazon CloudWatch Events<a name="monitoring-cloudwatch-events"></a>

You can use Amazon CloudWatch Events to detect and react to changes in the state of an instance or a deployment \(an "event"\) in your CodeDeploy operations\. Then, based on rules you create, CloudWatch Events will invoke one or more target actions when a deployment or instance enters the state you specify in a rule\. Depending on the type of state change, you might want to send notifications, capture state information, take corrective action, initiate events, or take other actions\. You can select the following types of targets when using CloudWatch Events as part of your CodeDeploy operations:
+ AWS Lambda functions
+  Kinesis streams
+ Amazon SQS queues
+ Built\-in targets \(CloudWatch alarm actions\)
+ Amazon SNS topics

The following are some use cases:
+ Use a Lambda function to pass a notification to a Slack channel whenever deployments fail\.
+ Push data about deployments or instances to a Kinesis stream to support comprehensive, real\-time status monitoring\.
+ Use CloudWatch alarm actions to automatically stop, terminate, reboot, or recover Amazon EC2 instances when a deployment or instance event you specify occurs\.

The remainder of this topic describes the basic procedure for creating a CloudWatch Events rule for CodeDeploy\. Before you create event rules for use in your CodeDeploy operations, however, you should do the following:
+ Complete the CloudWatch Events prerequisites\. For information, see [Amazon CloudWatch Events Prerequisites](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_Prerequisites.html)\.
+ Familiarize yourself with events, rules, and targets in CloudWatch Events\. For more information, see [What is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) and [New CloudWatch Events – Track and respond to changes to your AWS resources](http://aws.amazon.com/blogs/aws/new-cloudwatch-events-track-and-respond-to-changes-to-your-aws-resources/)\.
+ Create the target or targets you will use in your event rules\. 

**To create a CloudWatch Events rule for CodeDeploy:**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**\.

1. Choose **Create rule**, and then under **Event selector**, choose **AWS CodeDeploy**\.

1. Specify a detail type:
   + To make a rule that applies to all state changes of both instances and deployments, choose **Any detail type**, and then skip to step 6\.
   + To make a rule that applies to instances only, choose **Specific detail type**, and then choose **CodeDeploy Instance State\-change Notification**\.
   + To make a rule that applies to deployments only, choose **Specific detail type**, and then choose **CodeDeploy Deployment State\-change Notification**\.

1. Specify the state changes the rule applies to:
   + To make a rule that applies to all state changes, choose **Any state**\.
   + To make a rule that applies to some state changes only, choose **Specific state\(s\)**, and then choose one or more status values from the list\. The following table lists the status values you can choose:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-cloudwatch-events.html)

1. Specify which CodeDeploy applications the rule applies to:
   + To make a rule that applies to all applications, choose **Any application**, and then skip to step 8\.
   + To make a rule that applies to one application only, choose **Specific application**, and then choose the name of the application from the list\.

1. Specify which deployment groups the rule applies to:
   + To make a rule that applies to all deployment groups associated with the selected application, choose **Any deployment group**\.
   + To make a rule that applies to only one of the deployment groups associated with the selected application, choose **Specific deployment group\(s\)**, and then choose the name of the deployment group from the list\.

1. Review your rule setup to make sure it meets your event\-monitoring requirements\.

   The following shows the setup for an event rule that will be processed whenever a deployment fails to any instance in the `MyDeploymentFleet` deployment group for the application named `MyCodeDeployApp`:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/CWE-Event-selector.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

1. In the **Targets** area, choose **Add target\***\.

1. In the **Select target type** list, choose the type of target you have prepared to use with this rule, and then configure any additional options required by that type\. 

1. Choose **Configure details**\.

1. On the **Configure rule details** page, type a name and description for the rule, and then choose the **State** box to enable to rule now\.

1. If you're satisfied with the rule, choose **Create rule**\.
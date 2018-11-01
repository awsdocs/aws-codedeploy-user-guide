--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Monitoring Deployments with Amazon CloudWatch Tools<a name="monitoring-cloudwatch"></a>

You can monitor AWS CodeDeploy deployments using the following CloudWatch tools: Amazon CloudWatch Events, CloudWatch alarms, and Amazon CloudWatch Logs\. 

Reviewing the logs created by the AWS CodeDeploy agent and deployments can help you troubleshoot the causes of deployment failures\. As an alternative to reviewing AWS CodeDeploy logs on one instance at a time, you can use CloudWatch Logs to monitor all logs in a central location\. 

For information about using the CloudWatch console to view AWS CodeDeploy logs, see [View AWS CodeDeploy Logs in CloudWatch Logs Console](http://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)\.

For information about using CloudWatch alarms and CloudWatch Events to monitor your AWS CodeDeploy deployments, see the following topics\. 

**Topics**
+ [Monitoring Deployments with CloudWatch Alarms in AWS CodeDeploy](monitoring-create-alarms.md)
+ [Monitoring Deployments with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)
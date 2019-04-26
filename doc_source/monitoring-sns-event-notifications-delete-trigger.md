# Delete a Trigger from a CodeDeploy Deployment Group<a name="monitoring-sns-event-notifications-delete-trigger"></a>

Because there is a limit of 10 triggers per deployment group, you might want to delete triggers if they are no longer being used\. You cannot undo the deletion of a trigger, but you can re\-create one\.

## Delete a Trigger from a Deployment Group \(Console\)<a name="monitoring-sns-event-notifications-delete-trigger-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting Started with CodeDeploy](getting-started-codedeploy.md)\.

1. On the **Applications** page, choose the application associated with the deployment group from which you want to delete a trigger\.

1. On the **Application details** page, choose the arrow next to the deployment group\.

1. In the **Triggers** area, locate the name of the trigger to delete, choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/delete-triggers-x.png) button at the end of its row, and then choose **Delete**\.

## Delete a Trigger from a Deployment Group \(CLI\)<a name="monitoring-sns-event-notifications-delete-trigger-cli"></a>

To use the CLI to delete a trigger, call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, with empty trigger configuration parameters, specifying:
+ The name of the application associated with the deployment group\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The name of the deployment group associated with the application\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.

For example:

```
aws deploy update-deployment-group --application-name application-name --current-deployment-group-name deployment-group-name --trigger-configurations
```
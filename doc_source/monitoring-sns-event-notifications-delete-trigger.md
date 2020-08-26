# Delete a trigger from a CodeDeploy deployment group<a name="monitoring-sns-event-notifications-delete-trigger"></a>

Because there is a limit of 10 triggers per deployment group, you might want to delete triggers if they are no longer being used\. You cannot undo the deletion of a trigger, but you can re\-create one\.

## Delete a trigger from a deployment group \(console\)<a name="monitoring-sns-event-notifications-delete-trigger-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. On the **Applications** page, choose the name of the application associated with the deployment group where you want to delete a trigger\.

1. On the **Application details** page, choose the deployment group where you want to delete a trigger\.

1.  Choose **Edit**\. 

1.  Expand **Advanced \- optional**\. 

1. In the **Triggers** area, choose the trigger you want to delete, then choose **Delete trigger**\. 

1.  Choose **Save changes**\. 

## Delete a trigger from a deployment group \(CLI\)<a name="monitoring-sns-event-notifications-delete-trigger-cli"></a>

To use the CLI to delete a trigger, call the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command, with empty trigger configuration parameters, specifying:
+ The name of the application associated with the deployment group\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The name of the deployment group associated with the application\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.

For example:

```
aws deploy update-deployment-group --application-name application-name --current-deployment-group-name deployment-group-name --trigger-configurations
```
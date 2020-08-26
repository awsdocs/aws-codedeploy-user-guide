# Create a deployment group \(CLI\)<a name="deployment-groups-create-cli"></a>

To use the AWS CLI to create a deployment group, call the [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command, specifying:
+ The application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ A name for the deployment group\. A deployment group with this name is created for the specified application\. A deployment group can only be associated with one application\.
+ Information about the tags, tag groups, or Amazon EC2 Auto Scaling group names that identify the instances to be included in the deployment group\.
+ The Amazon Resource Name \(ARN\) identifier of the service role that allows CodeDeploy to act on behalf of your AWS account when interacting with other AWS services\. To get the service role ARN, see [Get the service role ARN \(CLI\) ](getting-started-create-service-role.md#getting-started-get-service-role-cli)\. For more information about service roles, see [Roles terms and concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html#roles-about-termsandconcepts) in *IAM User Guide*\.
+ Information about the type of deployment, either in\-place or blue/green, to associate with the deployment group\.
+ \(Optional\) The name of an existing deployment configuration\. To view a list of deployment configurations, see [View deployment configuration details with CodeDeploy](deployment-configurations-view-details.md)\. If not specified, CodeDeploy uses a default deployment configuration\.
+ \(Optional\) Commands to create a trigger that pushes notifications about deployment and instance events to those who are subscribed to an Amazon Simple Notification Service topic\. For more information, see [Monitoring deployments with Amazon SNS event notifications](monitoring-sns-event-notifications.md)\.
+ \(Optional\) Commands to add existing CloudWatch alarms to the deployment group that are activated if a metric specified in an alarm falls below or exceeds a defined threshold\.
+ \(Optional\) Commands for a deployment to roll back to the last known good revision when a deployment fails or a CloudWatch alarm is activated\.
+ For in\-place deployments:
  + \(Optional\) The name of the Classic Load Balancer or Application Load Balancer in Elastic Load Balancing that manages traffic to the instances during the deployment processes\.
+ For blue/green deployments:
  + Configuration of the blue/green deployment process:
    + How new instances in the replacement environment are provisioned\.
    + Whether to reroute traffic to the replacement environment immediately or wait a specified period for traffic to be rerouted manually\.
    + Whether instances in the original environment should be terminated\. 
  + The name of the Classic Load Balancer or Application Load Balancer in Elastic Load Balancing to be used for instances registered in the replacement environment\.
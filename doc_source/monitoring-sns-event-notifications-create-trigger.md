# Create a Trigger for an AWS CodeDeploy Event<a name="monitoring-sns-event-notifications-create-trigger"></a>

You can create a trigger that publishes an Amazon Simple Notification Service \(Amazon SNS\) topic for an AWS CodeDeploy deployment or instance event\. Then, when that event occurs, all subscribers to the associated topic will receive notifications through the endpoint specified in the topic, such as an SMS message or email message\. Amazon SNS offers multiple methods for subscribing to topics\.

Before you create a trigger, you must set up the Amazon SNS topic to which the trigger will point\. For information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\. When you create a topic, we recommend you give it a name that will identify its purpose, in formats such as `Topic-group-us-west-3-deploy-fail` or `Topic-group-project-2-instance-stop`\. 

You must also grant Amazon SNS permissions to an AWS CodeDeploy service role before notifications can be sent for your trigger\. For information, see [Grant Amazon SNS Permissions to an AWS CodeDeploy Service Role](monitoring-sns-event-notifications-permisssions.md)\.

After you have created the topic, you can add subscribers\. For information about creating, managing, and subscribing to topics, see [What Is Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

## Create a Trigger to Send Notifications for AWS CodeDeploy Events \(Console\)<a name="monitoring-sns-event-notifications-create-trigger-console"></a>

You can use the AWS CodeDeploy console to create triggers for an AWS CodeDeploy event\. At the end of the setup process, a test notification message is sent to ensure that both permissions and trigger details are set up correctly\.

**To create a trigger for an AWS CodeDeploy event**

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the **Applications** page, choose the name of the application for which triggers will be sent\.

1. On the **Applications details** page, choose the arrow next to the deployment group for which triggers will be sent\.

1. In the **Triggers ** area, choose **Create trigger**\.

1. In the **Create trigger** pane, do the following:
   + In **Trigger name**, type a name for the trigger that makes it easy to identify its purpose\. We recommend formats such as `Trigger-group-us-west-3-deploy-fail` or `Trigger-group-eu-central-instance-stop`\.
   + In **Events**, choose the event type or types that will trigger the Amazon SNS topic to send notifications\. 
   + In **Amazon SNS topic**, choose the name of topic you created for sending notifications for this trigger\.

1. Choose **Create trigger**\.

   AWS CodeDeploy will send a test notification to confirm you have correctly configured access between AWS CodeDeploy and the Amazon SNS topic\. Depending on the endpoint type you selected for the topic, and if you are subscribed to the topic, you will receive confirmation in an SMS message or email message\.

## Create a Trigger to Send Notifications for AWS CodeDeploy Events \(CLI\)<a name="monitoring-sns-event-notifications-create-trigger-cli"></a>

You can use the CLI to include triggers when you create a deployment group, or you can add triggers to an existing deployment group\.

### To create a trigger to send notifications for a new deployment group<a name="monitoring-sns-event-notifications-create-trigger-cli-new"></a>

Create a JSON file to configure the deployment group, and then run the [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command using the `--cli-input-json` option\. 

The simplest way to create the JSON file is to use the `--generate-cli-skeleton` option to get a copy of the JSON format, and then provide the required values in a plain\-text editor\.

1. Run the following command, and then copy the results into a plain\-text editor\.

   ```
   aws deploy create-deployment-group --generate-cli-skeleton
   ```

1. Add the name of an existing AWS CodeDeploy application to the output:

   ```
   {
       "applicationName": "TestApp-us-east-2",
       "deploymentGroupName": "",
       "deploymentConfigName": "",
       "ec2TagFilters": [
           {
               "Key": "",
               "Value": "",
               "Type": ""
           }
       ],
       "onPremisesInstanceTagFilters": [
           {
               "Key": "",
               "Value": "",
               "Type": ""
           }
       ],
       "autoScalingGroups": [
           ""
       ],
       "serviceRoleArn": "",
       "triggerConfigurations": [
           {
               "triggerName": "",
               "triggerTargetArn": "",
               "triggerEvents": [
                   ""
               ]
           }
       ]
   }
   ```

1. Provide values for the parameters you want to configure\.

   When you use the [create\-deployment\-group](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_CreateDeploymentGroup.html) command, you must provide, at a minimum, values for the following parameters:
   + `applicationName`: The name of an application already created in your account\. 
   + `deploymentGroupName`: A name for the deployment group you are creating\.
   + `serviceRoleArn`: The ARN of an existing service role set up for AWS CodeDeploy in your account\. For information, see [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\.

   In the `triggerConfigurations` section, provide values for the following parameters: 
   + `triggerName`: The name you want to give the trigger so you can easily identify it\. We recommend formats such as `Trigger-group-us-west-3-deploy-fail` or `Trigger-group-eu-central-instance-stop`\.
   + `triggerTargetArn`: The ARN of the Amazon SNS topic you created to associate with your trigger, in this format: `arn:aws:sns:us-east-2:80398EXAMPLE:NewTestTopic`\.
   + `triggerEvents`: The type of event or events for which you want to trigger notifications\. You can specify one or more event types, separating multiple event type names with commas \(for example, `"triggerEvents":["DeploymentSuccess","DeploymentFailure","InstanceFailure"]`\)\. When you add more than one event type, notifications for all those types are sent to the topic you specified, rather than to a different topic for each one\. You can choose from the following event types:
     + DeploymentStart
     + DeploymentSuccess
     + DeploymentFailure
     + DeploymentStop
     + DeploymentRollback
     + DeploymentReady \(Applies only to replacement instances in a blue/green deployment\)
     + InstanceStart
     + InstanceSuccess
     + InstanceFailure
     + InstanceReady \(Applies only to replacement instances in a blue/green deployment\)

   The following configuration example creates a deployment group named `dep-group-ghi-789-2` for an application named `TestApp-us-east-2` and a trigger that will prompt the sending of notifications wheneve2 a deployment starts, succeeds, or fails:

   ```
   {
       "applicationName": "TestApp-us-east-2",
       "deploymentConfigName": "CodeDeployDefault.OneAtATime",
       "deploymentGroupName": "dep-group-ghi-789-2",
       "ec2TagFilters": [
           {
               "Key": "Name",
               "Value": "Project-ABC",
               "Type": "KEY_AND_VALUE"
           }
       ],
       "serviceRoleArn": "arn:aws:iam::444455556666:role/AnyCompany-service-role",
       "triggerConfigurations": [
           {
               "triggerName": "Trigger-group-us-east-2",
               "triggerTargetArn": "arn:aws:sns:us-east-2:80398EXAMPLE:us-east-deployments",
               "triggerEvents": [
                   "DeploymentStart",
                   "DeploymentSuccess",
                   "DeploymentFailure"
               ]
           }
       ]
   }
   ```

1. Save your updates as a JSON file, and then call that file using the `--cli-input-json` option when you run the create\-deployment\-group command:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws deploy create-deployment-group --cli-input-json file://filename.json
   ```

   At the end of the creation process, you will receive a test notification message that indicates both permissions and trigger details are set up correctly\.

### To create a trigger to send notifications for an existing deployment group<a name="monitoring-sns-event-notifications-create-trigger-cli-existing"></a>

To use the AWS CLI to add triggers for AWS CodeDeploy events to an existing deployment group, create a JSON file to update the deployment group, and then run the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command using the `--cli-input-json` option\. 

The simplest way to create the JSON file is to run the get\-deployment\-group command to get a copy of the deployment group's configuration, in JSON format, and then update the parameter values in a plain\-text editor\.

1.  Run the following command, and then copy the results into a plain\-text editor\.

   ```
   aws deploy get-deployment-group --application-name application --deployment-group-name deployment-group
   ```

1. Delete the following from the output:
   + At the beginning of the output, delete `{ "deploymentGroupInfo":`\.
   + At the end of the output, delete `}`\. 
   + Delete the row containing `deploymentGroupId`\.
   + Delete the row containing `deploymentGroupName`\.

   The contents of your text file should now look similar to the following:

   ```
   {
       "applicationName": "TestApp-us-east-2",
       "deploymentConfigName": "CodeDeployDefault.OneAtATime",
       "autoScalingGroups": [],
       "ec2TagFilters": [
           {
               "Type": "KEY_AND_VALUE",
               "Value": "Project-ABC",
               "Key": "Name"
           }
       ],
       "triggerConfigurations": [],
       "serviceRoleArn": "arn:aws:iam::444455556666:role/AnyCompany-service-role",
       "onPremisesInstanceTagFilters": []
   }
   ```

1. In the `triggerConfigurations` section, add data for the `triggerEvents`, `triggerTargetArn`, and `triggerName` parameters\. For information about trigger configuration parameters, see [TriggerConfig](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_TriggerConfig.html)\.

   The contents of your text file should now look similar to the following\. This code will prompt notifications to be sent whenever a deployment starts, succeeds, or fails\. 

   ```
   {
       "applicationName": "TestApp-us-east-2",
       "deploymentConfigName": "CodeDeployDefault.OneAtATime",
       "autoScalingGroups": [],
       "ec2TagFilters": [
           {
               "Type": "KEY_AND_VALUE",
               "Value": "Project-ABC",
               "Key": "Name"
           }
       ],
       "triggerConfigurations": [
           {
               "triggerEvents": [
                   "DeploymentStart",
                   "DeploymentSuccess",
                   "DeploymentFailure"
               ],
               "triggerTargetArn": "arn:aws:sns:us-east-2:80398EXAMPLE:us-east-deployments",
               "triggerName": "Trigger-group-us-east-2"
           }
       ],
       "serviceRoleArn": "arn:aws:iam::444455556666:role/AnyCompany-service-role",
       "onPremisesInstanceTagFilters": []
   }
   ```

1. Save your updates as a JSON file, and then run the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command using the `--cli-input-json` option\. Be sure to include the `--current-deployment-group-name` option and substitute the name of your JSON file for *filename*: 
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws deploy update-deployment-group --current-deployment-group-name deployment-group-name --cli-input-json file://filename.json
   ```

   At the end of the creation process, you will receive a test notification message that indicates both permissions and trigger details are set up correctly\.
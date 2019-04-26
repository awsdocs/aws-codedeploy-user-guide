# Edit a Trigger in a CodeDeploy Deployment Group<a name="monitoring-sns-event-notifications-edit-trigger"></a>

If your notification requirements change, you can modify your trigger rather than create a new one\.

## Modify a CodeDeploy Trigger \(Console\)<a name="monitoring-sns-event-notifications-edit-trigger-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting Started with CodeDeploy](getting-started-codedeploy.md)\.

1. On the **Applications** page, choose the name of the application associated with the deployment group where you will modify a trigger\.

1. On the **Application details** page, choose the arrow next to the deployment group where you will edit a trigger\.

1. In the **Triggers** area, locate the name of the trigger you want to modify, and then choose the pencil icon at the end of its row\.

1. Update the trigger name, selected events, or Amazon SNS topic, and then choose **Save**\.

## Modify a CodeDeploy Trigger \(CLI\)<a name="monitoring-sns-event-notifications-edit-trigger-cli"></a>

 To use the AWS CLI to change trigger details for CodeDeploy events when you update a deployment group, create a JSON file to define changes to the deployment group's properties, and then run the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command with the `--cli-input-json` option\. 

The simplest way to create the JSON file is to run the get\-deployment\-group command to get the current deployment group details in JSON format, and then edit the required values in a plain\-text editor\.

1. Run the following command, substituting the names of your application and deployment group for *application* and *deployment\-group*:

   ```
   aws deploy get-deployment-group --application-name application --deployment-group-name deployment-group
   ```

1. Copy the results of the command into a plain\-text editor and then delete the following:
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
               "Value": "East-1-Instances",
               "Key": "Name"
           }
       ],
       "triggerConfigurations": [
           {
               "triggerEvents": [
                   "DeploymentStart",
                   "DeploymentSuccess",
                   "DeploymentFailure",
                   "DeploymentStop"
               ],
               "triggerTargetArn": "arn:aws:sns:us-east-2:111222333444:Trigger-group-us-east-2",
               "triggerName": "Trigger-group-us-east-2"
           }
       ],
       "serviceRoleArn": "arn:aws:iam::444455556666:role/AnyCompany-service-role",
       "onPremisesInstanceTagFilters": []
   }
   ```

1. Change any parameters, as necessary\. For information about trigger configuration parameters, see [TriggerConfig](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_TriggerConfig.html)\.

1. Save your updates as a JSON file, and then run the [update\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/update-deployment-group.html) command using the `--cli-input-json` option\. Be sure to include the `--current-deployment-group-name` option and substitute the name of your JSON file for *filename*: 
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws deploy update-deployment-group --current-deployment-group-name deployment-group-name --cli-input-json file://filename.json
   ```

At the end of the creation process, you will receive a test notification message that indicates both permissions and trigger details are set up correctly\.
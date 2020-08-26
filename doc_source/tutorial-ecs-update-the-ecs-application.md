# Step 1: Update your Amazon ECS application<a name="tutorial-ecs-update-the-ecs-application"></a>

 In this section, you update your Amazon ECS application with a new revision of its task definition\. The updated revision adds a new key and tag pair\. In [ Step 3: Use the CodeDeploy console to deploy your Amazon ECS service](tutorial-ecs-deployment-deploy.md), you deploy the updated version of your Amazon ECS application\. 

**To update your task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1.  In the navigation pane, choose **Task Definitions**\. 

1.  Select the check box for the task definition used by your Amazon ECS service\.

1.  Choose **Create new revision**\. 

1.  For this tutorial, make a small update to the task definition by adding a tag\. At the bottom of the page, in **Tags**, create a new tag by entering a new key and value pair\. 

1.  Choose **Create**\. You should see your task definition's revision number has been incremented by one\. 

1.  Choose the **JSON** tab\. Make a note of the following because you need this information in the next step\. 
   +  The value for `taskDefinitionArn`\. Its format is `arn:aws:ecs:aws-region: account-id:task-definition/task-definition-family: task-definition-revision`\. This is the ARN of your updated task definition\. 
   +  In the `containerDefinitions` element, the value for `name`\. This is the name of your container\. 
   +  In the `portMappings` element, the value for `containerPort`\. This is the port for your container\. 
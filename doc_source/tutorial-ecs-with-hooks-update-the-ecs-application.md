# Step 2: Update your Amazon ECS application<a name="tutorial-ecs-with-hooks-update-the-ecs-application"></a>

 In this section, you update your Amazon ECS application to use a new revision of its task definition\. You create the new revision and add a minor update to it by adding a tag\. 

**To update your task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1.  In the navigation pane, choose **Task Definitions**\. 

1.  Select the check box for the task definition used by your Amazon ECS service\.

1.  Choose **Create new revision**\. 

1.  Make a small update to the task definition by adding a tag\. At the bottom of the page, in **Tags**, create a new tag by entering a new key and value pair\. 

1.  Choose **Create**\. You should see that your task definition's revision number has been incremented by one\. 

1.  Choose the **JSON** tab\. Make a note of the value for `taskDefinitionArn`\. Its format is `arn:aws:ecs:aws-region: account-id:task-definition/task-definition-family: task-definition-revision`\. This is the ARN of your updated task definition\. 
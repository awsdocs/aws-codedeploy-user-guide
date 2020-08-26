# Step 4: Clean up<a name="tutorial-ecs-clean-up"></a>

 The next tutorial, [Tutorial: Deploy an Amazon ECS service with a validation test](tutorial-ecs-deployment-with-hooks.md), builds on this tutorial and uses the CodeDeploy application and deployment group you created\. If you want to follow the steps in that tutorial, skip this step and do not delete the resources you created\. 

**Note**  
 Your AWS account does not incur charges for the CodeDeploy resources you created\. 

The resource names in these steps are the names suggested in this tutorial \(for example, **ecs\-demo\-codedeploy\-app** for the name of your CodeDeploy application\)\. If you used different names, then be sure to use those during your cleanup\. 

1. Use the [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command to delete the CodeDeploy deployment group\.

   ```
   aws deploy delete-deployment-group --application-name ecs-demo-codedeploy-app --deployment-group-name ecs-demo-dg --region aws-region-id
   ```

1. Use the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command to delete the CodeDeploy application\.

   ```
   aws deploy delete-application --application-name ecs-demo-codedeploy-app --region aws-region-id
   ```
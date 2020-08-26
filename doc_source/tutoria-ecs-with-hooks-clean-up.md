# Step 7: Clean up<a name="tutoria-ecs-with-hooks-clean-up"></a>

 When you have finished this tutorial, clean up the resources associated with it to avoid incurring charges for resources that you aren't using\. The resource names in this step are the names suggested in this tutorial \(for example, **ecs\-demo\-codedeploy\-app** for the name of your CodeDeploy application\)\. If you used different names, then be sure to use those in your cleanup\. 

**To clean up the tutorial resources**

1. Use the [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command to delete the CodeDeploy deployment group\.

   ```
   aws deploy delete-deployment-group --application-name ecs-demo-deployment-group --deployment-group-name ecs-demo-dg --region aws-region-id
   ```

1. Use the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command to delete the CodeDeploy application\.

   ```
   aws deploy delete-application --application-name ecs-demo-deployment-group --region aws-region-id
   ```

1. Use the [delete\-function](https://docs.aws.amazon.com/cli/latest/reference/lambda/delete-function.html) command to delete your Lambda hook function\.

   ```
   aws lambda delete-function --function-name AfterAllowTestTraffic
   ```

1. Use the [delete\-log\-group](https://docs.aws.amazon.com/cli/latest/reference/logs/delete-log-group.html) command to delete your CloudWatch log group\.

   ```
   aws logs delete-log-group --log-group-name /aws/lambda/AfterAllowTestTraffic
   ```
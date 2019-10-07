[Back to contents](index.md)

# Tutorial: Deploy an Amazon ECS Service with a Validation Test<a name="tutorial-ecs-deployment-with-hooks"></a>

 In this tutorial, you learn how to use a Lambda function to validate part of the deployment of an updated Amazon ECS application\. This tutorial uses the CodeDeploy application, CodeDeploy deployment group, and the Amazon ECS application you used in [Tutorial: Deploy an Amazon ECS Service](tutorial-ecs-deployment.md)\. Complete that tutorial before starting this one\.

 To add validation test, you first implement the test in a Lambda function\. Next, in your deployment AppSpec file, you specify the Lambda function for the lifecycle hook you want to test\. If a validation test fails, the deployment stops, is rolled back, and marked failed\. If the test succeeds, the deployment continues to the next deployment lifecycle event or hook\. 

 During an Amazon ECS deployment with validation tests, CodeDeploy uses a load balancer that is configured with two target groups: one production traffic listener and one test traffic listener\. The following diagram shows how the load balancer, production and test listeners, target groups, and your Amazon ECS application are related before the delpoyments starts\. This tutorial uses an Application Load Balancer\. You can also use a Network Load Balancer\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-1.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

 During an Amazon ECS deployment, there are five lifecycle hooks for testing\. This tutorial implements one test during the third lifecycle deployment hook, `AfterAllowTestTraffic`\. For more information, see [List of Lifecycle Event Hooks for an Amazon ECS Deployment](reference-appspec-file-structure-hooks.md#reference-appspec-file-structure-hooks-list-ecs)\. After a successful deployment, the production traffic listener serves traffic to your new replacment task set and the original task set is terminated\. The following diagram shows how your resources are related after a successful deployment\. For more information, see [What Happens During an Amazon ECS Deployment](deployment-steps-ecs.md#deployment-steps-what-happens)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-step-6.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**Note**  
Completing this tutorial might result in charges to your AWS account\. These include possible charges for CodeDeploy, AWS Lambda, and CloudWatch\. For more information, see [AWS CodeDeploy Pricing](https://aws.amazon.com/codedeploy/pricing/), [AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/), and [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

**Topics**
+ [Prerequisites](tutorial-ecs-with-hooks-prereqs.md)
+ [Step 1: Create a Test Listener](tutorial-ecs-with-hooks-create-second-listener.md)
+ [Step 2: Update Your Amazon ECS Application](tutorial-ecs-with-hooks-update-the-ecs-application.md)
+ [Step 3: Create a Lifecycle Hook Lambda Function](tutorial-ecs-with-hooks-create-hooks.md)
+ [Step 4: Update Your AppSpec File](tutorial-ecs-with-hooks-create-appspec-file.md)
+ [Step 4: Use the CodeDeploy Console to Deploy Your Amazon ECS Service](tutorial-ecs-with-hooks-deployment.md)
+ [Step 5: View Your Lambda Hook Function Output in CloudWatch Logs](tutorial-ecs-with-hooks-view-cw-logs.md)
+ [Step 7: Clean Up](tutoria-ecs-with-hooks-clean-up.md)
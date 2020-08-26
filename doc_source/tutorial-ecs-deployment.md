# Tutorial: Deploy an Amazon ECS service<a name="tutorial-ecs-deployment"></a>

 In this tutorial, you learn how to deploy an Amazon ECS application\. You start with an Amazon ECS application you already created\. The first step is to update your application by modifying its task definition file with a new tag\. Next, you use CodeDeploy to deploy the update\. During deployment, CodeDeploy installs your update into a new, replacement task set\. Then, it shifts production traffic from the original version of your Amazon ECS service, which is in its original task set, to the updated version in the replacement task set\.

**Note**  
CodeDeploy does not currently support Amazon ECS Capacity Provider\.

 During an Amazon ECS deployment, CodeDeploy uses a load balancer that is configured with two target groups and one production traffic listener\. The following diagram shows how the load balancer, production listener, target groups, and your Amazon ECS application are related before the deployment starts\. This tutorial uses an Application Load Balancer\. You can also use a Network Load Balancer\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-with-no-test-listener-step-1.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

 After a successful deployment, the production traffic listener serves traffic to your new replacement task set and the original task set is terminated\. The following diagram shows how your resources are related after a successful deployment\. For more information, see [What happens during an Amazon ECS deployment](deployment-steps-ecs.md#deployment-steps-what-happens)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/codedeploy-ecs-deployment-with-no-test-listener-step-5.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

For information about how to use the AWS CLI to deploy an Amazon ECS service, see [ Tutorial: Creating a service using a blue/green deployment](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-blue-green.html)\. For information about how to use CodePipeline to detect and automatically deploy changes to an Amazon ECS service with CodeDeploy, see [Tutorial: Create a pipeline with an Amazon ECR source and ECS\-to\-CodeDeploy deployment](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-ecs-ecr-codedeploy.html)\. 

After you complete this tutorial, you can use the CodeDeploy application and deployment group you created to add a deployment validation test in [Tutorial: Deploy an Amazon ECS service with a validation test](tutorial-ecs-deployment-with-hooks.md)\. 

**Topics**
+ [Prerequisites](tutorial-ecs-prereqs.md)
+ [Step 1: Update your Amazon ECS application](tutorial-ecs-update-the-ecs-application.md)
+ [Step 2: Create the AppSpec file](tutorial-ecs-create-appspec-file.md)
+ [Step 3: Use the CodeDeploy console to deploy your Amazon ECS service](tutorial-ecs-deployment-deploy.md)
+ [Step 4: Clean up](tutorial-ecs-clean-up.md)
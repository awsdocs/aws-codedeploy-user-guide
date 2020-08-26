# Tutorial: Use CodeDeploy to deploy an application to an Amazon EC2 Auto Scaling group<a name="tutorials-auto-scaling-group"></a>

In this tutorial, you'll use CodeDeploy to deploy an application revision to an Amazon EC2 Auto Scaling group\. Amazon EC2 Auto Scaling launches Amazon EC2 instances using predifined conditions, and then terminates those instances when they are no longer needed\. Amazon EC2 Auto Scaling can help CodeDeploy scale by ensuring it always has the correct number of Amazon EC2 instances available to handle the load for deployments\. For information about Amazon EC2 Auto Scaling integration with CodeDeploy, see [Integrating CodeDeploy with Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md)\.

**Topics**
+ [Prerequisites](tutorials-auto-scaling-group-prerequisites.md)
+ [Step 1: Create and configure the Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group-create-auto-scaling-group.md)
+ [Step 2: Deploy the application to the Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group-create-deployment.md)
+ [Step 3: Check your results](tutorials-auto-scaling-group-verify.md)
+ [Step 4: Increase the number of Amazon EC2 instances in the Amazon EC2 Auto Scaling group](tutorials-auto-scaling-group-scale-up.md)
+ [Step 5: Check your results again](tutorials-auto-scaling-group-reverify.md)
+ [Step 6: Clean up](tutorials-auto-scaling-group-clean-up.md)
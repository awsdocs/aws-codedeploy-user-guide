# Step 3: Use the CodeDeploy console to deploy your Amazon ECS service<a name="tutorial-ecs-deployment-deploy"></a>

 In this section, you create a CodeDeploy application and deployment group to deploy your updated Amazon ECS application\. During deployment, CodeDeploy shifts the production traffic for your Amazon ECS application to its new version in a new, replacement task set\. To complete this step, you need the following items: 
+  Your Amazon ECS cluster name\. 
+  Your Amazon ECS service name\. 
+  Your Application Load Balancer name\. 
+  Your production listener port\. 
+  Your target group names\. 
+  The name of the S3 bucket you created\. 

**To create a CodeDeploy application**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy/](https://console.aws.amazon.com/codedeploy/)\.

1. Choose **Create application**\.

1. In **Application name**, enter **ecs\-demo\-codedeploy\-app**\.

1. In **Compute platform**, choose **Amazon ECS**\.

1. Choose **Create application**\.

**To create a CodeDeploy deployment group**

1. On the **Deployment groups** tab of your application page, choose **Create deployment group**\.

1. In **Deployment group name**, enter **ecs\-demo\-dg**\.

1. In **Service role**, choose a service role that grants CodeDeploy access to Amazon ECS\. For more information, see [Identity and access management for AWS CodeDeploy](security-iam.md)\.

1. In **Environment configuration**, choose your Amazon ECS cluster name and service name\.

1. From **Load balancers**, choose the name of the load balancer that serves traffic to your Amazon ECS service\.

1. From **Production listener port**, choose the port and protocol for the listener that serves production traﬃc to your Amazon ECS service \(for example, **HTTP: 80**\)\. This tutorial does not include an optional test listener, so do not choose a port from **Test listener port**\. 

1. From **Target group 1 name** and **Target group 2 name**, choose two different target groups to route traffic during your deployment\. Make sure that these are the target groups you created for your load balancer\. It does not matter which is used for target group 1 and which is used for target group 2\.

1. Choose **Reroute traffic immediately**\.

1. For **Original revision termination**, choose 0 days, 0 hours, and 5 minutes\. This lets you see your deployment complete faster than if you use the default \(1 hour\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/ecs-demo-create-acd-dg.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

1. Choose **Create deployment group**\.

**To deploy your Amazon ECS application**

1. From your deployment group console page, choose **Create deployment**\.

1.  For **Deployment group**, choose **ecs\-demo\-dg**\. 

1.  For **Revision type**, choose **My application is stored in Amazon S3**\. In **Revision location**, enter the name of your S3 bucket\. 

1.  For **Revision file type**, choose **\.json** or **\.yaml**, as appropriate\. 

1.  \(Optional\) In **Deployment description**, enter a description for your deployment\. 

1. Choose **Create deployment**\.

1.  In **Deployment status**, you can monitor your deployment\. After 100% of production traffic is routed to the replacement task set and before the five\-minute wait time expires, you can choose **Terminate original task set** to immediately terminate the original task set\. If you do not choose **Terminate original task set**, the original task set terminates after the five\-minute wait time you specified expires\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/ecs-tutorial-deployment-status-without-test-listener.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)
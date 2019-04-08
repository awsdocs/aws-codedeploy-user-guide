--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Create a Deployment Group for an Amazon ECS Deployment \(Console\)<a name="deployment-groups-create-ecs"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1.  From the **Applications table**, choose the name of the application associated with the deployment group you want to edit\. 

1.  On your application page, from **Deployment groups**, choose the name of the deployment group you want to edit\. 

1. On your application page, from the **Deployment groups** tab, choose **Create deployment group**\. For more information about what you need to create a deployment group for an Amazon ECS deployment, see [Before You Begin an Amazon ECS Deployment](deployment-steps.md#deployment-steps-prerequisites-ecs)\. 

1. In **Deployment group name**, enter a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group \(including the deployment group name and the deployment configuration\), choose those settings on this page\. Although this new group and the existing group might have the same name, AWS CodeDeploy treats them as separate deployment groups, because each is associated with a separate application\.

1. In **Service role**, choose a service role that grants AWS CodeDeploy access to Amazon ECS\. For more information, see [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\.

1.  From **Load balancer name**, choose the name of the load balancer that serves traffic to your Amazon ECS service\. 

1.  From **Production listener port**, choose the port and protocol for the listener that serves production traffic to your Amazon ECS service\. 

1.  \(Optional\) From **Test listener port**, choose the port and protocol of a test listener that serves traffic to the replacement task set in your Amazon ECS service during deployment\. You can specify one or more Lambda funtions in the AppSpec file that run during the `AfterAllowTestTraffic` hook\. The functions can run validation tests\. If a validation test fails, a deployment rollback is triggered\. If the validation tests succeed, the next hook in the deployment lifecycle, `BeforeAllowTestTraffic`, is triggered\. If a test listener port is not specified, nothing happens during the `AfterAllowTestTraffic` hook\. For more information, see [AppSpec 'hooks' Section for an Amazon ECS Deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\. 

1. From **Target group 1 name** and **Target group 2 name**, choose the target groups used to route traffic during your deployment\. AWS CodeDeploy binds one target group to your Amazon ECS service's original task set and the other to its replacement task set\. For more information, see [Target Groups for Your Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html)\. 

1.  Choose **Reroute traffic immediately** or **Specify when to reroute traffic** to determine when to reroute traffic to your updated Amazon ECS service\. 
     
    If you chose to specify when to reroute traffic, then choose the number of days, hours, and minutes to wait after a successful deployment for you manually reroute traffic. If the wait time you specify passes without traffic being rerouted, the deployment status is changed to Stopped\. 

1.  For **Original revision termination**, choose the number of days, hours, and minutes to wait after a successful deployment before the original task set in your Amazon ECS service is terminated\. 

1. \(Optional\) In **Advanced**, configure any options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For more information, see [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)\. 

# Create a Deployment Group for a Blue/Green Deployment \(Console\)<a name="deployment-groups-create-blue-green"></a>

To use the AWS CodeDeploy console to create a deployment group for a blue/green deployment:

**Warning**  
Do not follow these steps if:  
You do not have instances with the AWS CodeDeploy agent installed that you want to replace during the blue/green deployment process\. To set up your instances, follow the instructions in [Working with Instances for AWS CodeDeploy](instances.md), and then follow the steps in this topic\.
You want to create an application that uses a custom deployment configuration, but you have not yet created the deployment configuration\. Follow the instructions in [Create a Deployment Configuration with AWS CodeDeploy](deployment-configurations-create.md), and then follow the steps in this topic\. 
You do not have a service role that trusts AWS CodeDeploy with, at minimum, the trust and permissions described in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. To create and configure a service role, follow the instructions in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md), and then follow the steps in this topic\.
You have not created a Classic Load Balancer or an Application Load Balancer in Elastic Load Balancing for the registration of the instances in your replacement environment\. For more information, see [Set Up a Load Balancer in Elastic Load Balancing for AWS CodeDeploy Deployments](deployment-groups-create-load-balancer.md)\.

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. From the AWS CodeDeploy menu, choose **Applications**\.

1. On the **Applications** page, choose the name of the application for which you want to create a deployment group\.

1. Choose **Create deployment group**\.

1. In the **Deployment group name** box, type a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group, specify those settings on this page\. The settings you might want to reuse include, for example, the deployment group name, tags, Auto Scaling group names, or deployment configuration\. Although the new and existing deployment group have the same name, AWS CodeDeploy treats them as separate deployment groups, because they are associated with separate applications\.

1. Choose **Blue/green deployment**\.

1. In **Environment configuration**, choose the method to use to provide instances for your replacement environment:
   + **Automatically copy Auto Scaling group**: AWS CodeDeploy creates an Auto Scaling group by copying one you specify\.
   + **Manually provision instances**: You won't specify the instances for your replacement environment until you create a deployment\. You must create the instances before you start the deployment\. Instead, here you specify the instances you want to replace\.

1. Depending on your choice in step 7, do one of the following:
   + If you chose **Automatically copy Auto Scaling group**: In **Auto Scaling group**, choose the name of the Auto Scaling group you want to use as a template for the Auto Scaling group that will be created for the instances in your replacement environment\. The number of currently healthy instances in the Auto Scaling group you select will be created in your replacement environment\.
   + If you chose **Manually provision instances**: In **Choose the EC2 instances or Auto Scaling groups where the current application revision is deployed**, enter Amazon EC2 tag values or Auto Scaling group names to identify the instances in your original environment \(that is, the instances you want to replace or that are running the current application revision\)\. 

1. In **Load balancer**, choose the Classic Load Balancer, Application Load Balancer, or Network Load Balancer to use in the registration of instances in your replacement environment during the deployment process\. 
**Note**  
The instances in your original environment can be, but are not required to be, registered with the load balancer you choose\.

   For more information about load balancers for AWS CodeDeploy deployments, see [Integrating AWS CodeDeploy with Elastic Load Balancing](integrations-aws-elastic-load-balancing.md)\.

1. In **Deployment settings**, review the default options for rerouting traffic to the replacement environment, which deployment configuration to use for the deployment, and how instances in the original environment are handled after the deployment\.

   If you want to change the settings, continue to step 11\. Otherwise, skip to step 12\.

1. To change the deployment settings for the blue/green deployment, choose **Edit deployment settings**, update any of the following settings, and then choose **Submit**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-create-blue-green.html)

1. \(Optional\) In **Advanced**, configure options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For information about specifying advanced options in deployment groups, see [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)\. 

1. In the **Service role ARN** box, choose an Amazon Resource Name \(ARN\) for a service role that trusts AWS CodeDeploy with, at minimum, the trust and permissions described in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. To get the service role ARN, see [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Choose **Create deployment group**\. 
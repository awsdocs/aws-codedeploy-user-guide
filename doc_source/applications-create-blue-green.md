# Create an application for a blue/green deployment \(console\)<a name="applications-create-blue-green"></a>

To use the CodeDeploy console to create an application for a blue/green deployment:

**Note**  
A deployment to the AWS Lambda compute platform is always a blue/green deployment\. You do not specify a deployment type option\.

**Warning**  
Do not follow these steps if:  
You do not have instances with the CodeDeploy agent installed that you want to replace during the blue/green deployment process\. To set up your instances, follow the instructions in [Working with instances for CodeDeploy](instances.md), and then follow the steps in this topic\.
You want to create an application that uses a custom deployment configuration, but you have not yet created the deployment configuration\. Follow the instructions in [Create a deployment configuration with CodeDeploy](deployment-configurations-create.md), and then follow the steps in this topic\. 
You do not have a service role that trusts CodeDeploy with, at minimum, the trust and permissions described in [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\. To create and configure a service role, follow the instructions in [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md), and then follow the steps in this topic\.
You have not created a Classic Load Balancer, Application Load Balancer, or Network Load Balancer in Elastic Load Balancing for the registration of the instances in your replacement environment\. For more information, see [Set up a load balancer in Elastic Load Balancing for CodeDeploy Amazon EC2 deployments](deployment-groups-create-load-balancer.md)\.

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Getting started**\.

1. In **Application name**, enter the name of your application \.

1. From **Compute platform**, choose **EC2/On\-Premises**\.

1. Choose **Create application**\.

1. On your application page, from the **Deployment groups** tab, choose **Create deployment group**\.

1. In **Deployment group name**, enter a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group \(including the deployment group name tags, Amazon EC2 Auto Scaling group names, and the deployment configuration\), choose those settings on this page\. Although this new deployment group and the existing deployment group have the same name, CodeDeploy treats them as separate deployment groups, because each is associated with a separate application\.

1. In **Service role**, choose a service role that grants CodeDeploy access to your target instance\.

1. In **Deployment type** choose **Blue/green**\.

1. In **Environment configuration**, choose the method to use to provide instances for your replacement environment:

   1. **Automatically copy Amazon EC2 Auto Scaling group**: CodeDeploy creates an Amazon EC2 Auto Scaling group by copying one you specify\.

   1. **Manually provision instances**: You won't specify the instances for your replacement environment until you create a deployment\. You must create the instances before you start the deployment\. Instead, here you specify the instances you want to replace\.

1. Depending on your choice in step 10, do one of the following:
   + If you chose **Automatically copy Amazon EC2 Auto Scaling group**: In **Amazon EC2 Auto Scaling group**, choose or enter the name of the Amazon EC2 Auto Scaling group you want to use as a template for the Amazon EC2 Auto Scaling group for the instances in your replacement environment\. The number of currently healthy instances in the Amazon EC2 Auto Scaling group you choose is created in your replacement environment\.
   + If you chose **Manually provision instances**: Enable **Amazon EC2 Auto Scaling groups**, **Amazon EC2 intances**, or both to specify instances to add to this deployment group\. Enter Amazon EC2 tag values or Amazon EC2 Auto Scaling group names to identify the instances in your original environment \(that is, the instances you want to replace or that are running the current application revision\)\. 

1. \(Optional\) In **Load balancer**, enable **Enable load balancing**, and then choose an existing Classic Load Balancer, Application Load Balancer, or Network Load Balancer to manage traffic to the instances during the deployment processes\.

   Each instance is deregistered from the load balancer \(Classic Load Balancers\) or target group \(Application Load Balancers and Network Load Balancers\) to prevent traffic from being routed to it during the deployment\. It is re\-registered when the deployment is complete\.

   For more information about load balancers for CodeDeploy deployments, see [Integrating CodeDeploy with Elastic Load Balancing](integrations-aws-elastic-load-balancing.md)\.

1. In **Deployment settings**, review the default options for rerouting traffic to the replacement environment, which deployment configuration to use for the deployment, and how instances in the original environment are handled after the deployment\.

   If you want to change the settings, continue to the next step\. Otherwise, skip to step 15\.

1. To change the deployment settings for the blue/green deployment, change any of the following settings\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/applications-create-blue-green.html)

1. \(Optional\) In **Advanced**, configure options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For information about specifying advanced options in deployment groups, see [Configure advanced options for a deployment group](deployment-groups-configure-advanced-options.md)\. 

1. Choose **Create deployment group**\. 

The next step is to prepare a revision to deploy to the application and deployment group\. For instructions, see [Working with application revisions for CodeDeploy](application-revisions.md)\.
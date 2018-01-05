# Create an Application for an In\-Place Deployment \(Console\)<a name="applications-create-in-place"></a>

To use the AWS CodeDeploy console to create an application for an in\-place deployment:

**Warning**  
Do not follow these steps if:  
You have not prepared your instances to be used in AWS CodeDeploy deployments\. To set up your instances, follow the instructions in [Working with Instances for AWS CodeDeploy](instances.md), and then follow the steps in this topic\.
You want to create an application that uses a custom deployment configuration, but you have not yet created the deployment configuration\. Follow the instructions in [Create a Deployment Configuration with AWS CodeDeploy](deployment-configurations-create.md), and then follow the steps in this topic\. 
You do not have a service role that trusts AWS CodeDeploy with the minimum required trust and permissions\. To create and configure a service role with the required permissions, follow the instructions in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md), and then return to the steps in this topic\.
You want to select a Classic Load Balancer, Application Load Balancer, or Network Load Balancer in Elastic Load Balancing for the in\-place deployment, but have not yet created it\.

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the AWS CodeDeploy home page appears, choose **Get Started Now**\.

1. Choose **Create application**\.

1. In the **Application name** box, type a name for the application\. \(In an AWS account, an AWS CodeDeploy application name can be used only once per region\. You can reuse an application name in different regions\.\)

1. From the **Compute platform** drop\-down list, choose **EC2/On\-Premises**\.

1. In the **Deployment group name** box, type a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group \(including the deployment group name; tags, Auto Scaling group names, or both; and the deployment configuration\), specify those settings on this page\. Although this new deployment group and the existing deployment group have the same name, AWS CodeDeploy treats them as separate deployment groups, because they are each associated with separate applications\.

1. Choose **In\-place deployment**\.

1. In **Environment configuration**, choose from the following: 

   + On the **Auto Scaling groups** tab: Choose the name of an Auto Scaling group to deploy your application revision to\. When new Amazon EC2 instances are launched as part of an Auto Scaling group, AWS CodeDeploy can deploy your revisions to the new instances automatically\. You can add up to 10 Auto Scaling groups to a deployment group\.

   + On the **Amazon EC2 instances** tab or **On\-premises instances** tab: In the **Key** and **Value** fields, type the values of the key\-value pair you used to tag the instances\. You can tag up to 10 key\-value pairs in a single tag group\.

     + You can use wildcards in the **Value** field to identify all instances tagged in certain patterns, such as similar Amazon EC2 instance, cost center, and group names, and so on\. For example, if you select **Name** in the **Key** field and type **GRP\-\*a** in the **Value** field, AWS CodeDeploy identifies all instances that fit that pattern, such as **GRP\-1a**, **GRP\-2a**, and **GRP\-XYZ\-a**\.

     + The **Value** field is case\-sensitive\. 

     + To remove a key\-value pair from the list, choose the remove icon\.

     As AWS CodeDeploy finds instances that match each specified key\-value pair or Auto Scaling group name, it displays the number of matching instances\. To see more information about the instances, click the number\.

     If you want to refine the criteria for the deployed\-to instances, choose **Add tag group** to create an tag group\. You can create up to three tag groups with up to 10 key\-value pairs each\. When you use multiple tag groups in a deployment group, only instances that are identified by all the tag groups are included in the deployment group\. That means an instance must match at least one of the tags in each of the groups to be included in the deployment group\.

     For information about using tag groups to refine your deployment group, see [Tagging Instances for Deployment Groups in AWS CodeDeploy](instances-tagging.md)\.

1. \(Optional\) In **Load balancer**, choose **Enable load balancing**, and then choose an existing Classic Load Balancer, Application Load Balancer, or Network Load Balancer to manage traffic to the instances during the deployment processes\.

   Each instance is deregistered from the load balancer \(Classic Load Balancers\) or target group \(Application Load Balancers and Network Load Balancers\) to prevent traffic from being routed to it during the deployment\. It is re\-registered when the deployment is complete\.

   For more information about load balancers for AWS CodeDeploy deployments, see [Integrating AWS CodeDeploy with Elastic Load Balancing](integrations-aws-elastic-load-balancing.md)\.

1. In the **Deployment configuration** list, choose a deployment configuration to control the rate at which instances are deployed to, such as one at a time or all at once\. For more information about deployment configurations, see [Working with Deployment Configurations in AWS CodeDeploy](deployment-configurations.md)\.

1. \(Optional\) In **Advanced**, configure any options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For more information, see [Configure Advanced Options for a Deployment Group](deployment-groups-configure-advanced-options.md)\. 

1. In **Service role ARN**, choose an Amazon Resource Name \(ARN\) for a service role that trusts AWS CodeDeploy with, at minimum, the trust and permissions described in [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\. To get the service role ARN, see [Get the Service Role ARN \(Console\) ](getting-started-create-service-role.md#getting-started-get-service-role-console)\.

1. Choose **Create application**\. 

The next step is to prepare a revision to deploy to the application and deployment group\. For instructions, see [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\.
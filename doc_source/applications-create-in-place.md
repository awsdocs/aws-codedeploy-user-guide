# Create an application for an in\-place deployment \(console\)<a name="applications-create-in-place"></a>

To use the CodeDeploy console to create an application for an in\-place deployment:

**Warning**  
Do not follow these steps if:  
You have not prepared your instances to be used in CodeDeploy deployments\. To set up your instances, follow the instructions in [Working with instances for CodeDeploy](instances.md), and then follow the steps in this topic\.
You want to create an application that uses a custom deployment configuration, but you have not yet created the deployment configuration\. Follow the instructions in [Create a deployment configuration with CodeDeploy](deployment-configurations-create.md), and then follow the steps in this topic\. 
You do not have a service role that trusts CodeDeploy with the minimum required trust and permissions\. To create and configure a service role with the required permissions, follow the instructions in [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md), and then return to the steps in this topic\.
You want to select a Classic Load Balancer, Application Load Balancer, or Network Load Balancer in Elastic Load Balancing for the in\-place deployment, but have not yet created it\.

To create an application for an in\-place deployment using the CodeDeploy console:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Getting started**\.

1. Choose **Create application**\.

1. In **Application name**, enter the name of your application \.

1. From **Compute Platform**, choose **EC2/On\-premises**\.

1. Choose **Create application**\.

1. On your application page, from the **Deployment groups** tab, choose **Create deployment group**\.

1. In **Deployment group name**, enter a name that describes the deployment group\.
**Note**  
If you want to use the same settings used in another deployment group \(including the deployment group name; tags, Amazon EC2 Auto Scaling group names, or both; and the deployment configuration\), specify those settings on this page\. Although this new deployment group and the existing deployment group have the same name, CodeDeploy treats them as separate deployment groups, because they are each associated with separate applications\.

1. In **Service role**, choose a service role that grants CodeDeploy access to your target instance\.

1. In **Deployment type**, choose **In\-place**\.

1. In **Environment configuration**, select any of the following: 

   1. **Amazon EC2 Auto Scaling groups**: Enter or choose the name of an Amazon EC2 Auto Scaling group to deploy your application revision to\. When new Amazon EC2 instances are launched as part of an Amazon EC2 Auto Scaling group, CodeDeploy can deploy your revisions to the new instances automatically\. You can add up to 10 Amazon EC2 Auto Scaling groups to a deployment group\.

   1. **Amazon EC2 instances** or **On\-premises instances**: In the **Key** and **Value** fields, enter the values of the key\-value pair you used to tag the instances\. You can tag up to 10 key\-value pairs in a single tag group\.

      1. You can use wildcards in the **Value** field to identify all instances tagged in certain patterns, such as similar Amazon EC2 instance, cost center, and group names, and so on\. For example, if you choose **Name** in the **Key** field and enter **GRP\-\*a** in the **Value** field, CodeDeploy identifies all instances that fit that pattern, such as **GRP\-1a**, **GRP\-2a**, and **GRP\-XYZ\-a**\.

      1. The **Value** field is case sensitive\. 

      1. To remove a key\-value pair from the list, choose **Remove tag**\.

      As CodeDeploy finds instances that match each specified key\-value pair or Amazon EC2 Auto Scaling group name, it displays the number of matching instances\. Choose the number to see more information about the instances\.

      If you want to refine the criteria for the deployed\-to instances, choose **Add tag group** to create an tag group\. You can create up to three tag groups with up to ten key\-value pairs each\. When you use multiple tag groups in a deployment group, only instances that are identified by all the tag groups are included in the deployment group\. That means an instance must match at least one of the tags in each of the groups to be included in the deployment group\.

      For information about using tag groups to refine your deployment group, see [Tagging instances for deployment groups in CodeDeploy](instances-tagging.md)\.

1. In **Deployment settings**, choose a deployment configuration to control the rate your application is deployed to instances, such as one at a time or all at once\. For more information about deployment configurations, see [Working with deployment configurations in CodeDeploy](deployment-configurations.md)\.

1. \(Optional\) In **Load balancer**, select **Enable load balancing**, and then choose an existing Classic Load Balancer, Application Load Balancer, or Network Load Balancer to manage traffic to the instances during the deployment processes\.

   Each instance is deregistered from the load balancer \(Classic Load Balancers\) or target group \(Application Load Balancers and Network Load Balancers\) to prevent traffic from being routed to it during the deployment\. It is re\-registered when the deployment is complete\.

   For more information about load balancers for CodeDeploy deployments, see [Integrating CodeDeploy with Elastic Load Balancing](integrations-aws-elastic-load-balancing.md)\.

1. \(Optional\) Expand **Advanced**, and configure any options you want to include in the deployment, such as Amazon SNS notification triggers, Amazon CloudWatch alarms, or automatic rollbacks\.

   For more information, see [Configure advanced options for a deployment group](deployment-groups-configure-advanced-options.md)\. 

1. Choose **Create deployment group**\. 

The next step is to prepare a revision to deploy to the application and deployment group\. For instructions, see [Working with application revisions for CodeDeploy](application-revisions.md)\.
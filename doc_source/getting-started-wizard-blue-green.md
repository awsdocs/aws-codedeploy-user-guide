# Try a Sample Blue/Green Deployment in AWS CodeDeploy<a name="getting-started-wizard-blue-green"></a>

This section guides you through the steps required to deploy a revision to one or more Amazon EC2 instances using the Sample deployment wizard, and then run a blue/green deployment to replace the original set of instances in a deployment group, the original environment, with a differerent set of instances, the replacement environment\.

**Topics**
+ [Start the wizard](#getting-started-wizard-blue-green-start)
+ [Step 1: Get started](#getting-started-wizard-blue-green-1-get-started)
+ [Step 2: Choose a deployment type](#getting-started-wizard-blue-green-2-deployment-type)
+ [Step 3: Create blue/green deployment](#getting-started-wizard-blue-green-3-create)
+ [Step 4: Monitor the blue/green deployment](#getting-started-wizard-blue-green-4-monitor)
+ [Step 5: Refresh the web application window](#getting-started-wizard-blue-green-5-refresh)
+ [Step 6: Clean up sample resources](#getting-started-wizard-blue-green-6-cleanup)

## Start the wizard<a name="getting-started-wizard-blue-green-start"></a>

To start the wizard:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If an introductory page appears, choose **Get Started Now**\. If the **Applications** page appears, in **More info**, choose **Sample deployment wizard**\.

## Step 1: Get started<a name="getting-started-wizard-blue-green-1-get-started"></a>

Choose **Sample deployment**, and then choose **Next**\. 

## Step 2: Choose a deployment type<a name="getting-started-wizard-blue-green-2-deployment-type"></a>

Choose **Blue/green deployment**, and then choose **Next**\. 

## Step 3: Create blue/green deployment<a name="getting-started-wizard-blue-green-3-create"></a>

On the **Step 3: Create blue/green deployment** page, we provide default values for most of the components you will need for a blue/green deployment\. 

1. Accept or change any of the default names in the following fields:
**Note**  
Because the fields have different validation requirements, to save time, we recommend leaving the default names for your sample deployment\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-wizard-blue-green.html)

1. From the **Key pair name** drop\-down list, choose the Amazon EC2 instance key pair you will use to connect to the Amazon EC2 instances\.
**Note**  
To create an Amazon EC2 instance key pair, follow the instructions in [Creating Your Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure your key pair is created in one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. The new Amazon EC2 instance key pair may not appear in the **Key pair name** drop\-down list until you restart the wizard\.

1. Choose **Launch environment**\.

1. After the environment is ready, review the details under **Congratulations\! Your environment is ready**\.
**Note**  
For easy reference later, we recommend that you copy the entire text in the **Congratulations\! Your environment is ready** area to a file on your computer\.
   + The name of the Classic Load Balancer created for you, such as `BlueGreenLoadBalancer-abcdefh`\.
   + The name of the Auto Scaling group created for your original environment, such as `CodeDeployBGStack-abcdefh-BlueGreenAutoScalingGroup-1IJKLMN234O56`\. 
   + The web address of the application that has been deployed in your original environment, such as `http://BlueGreenLoadBalancer-abcdefh-1234567890.us-east-2.elb.amazonaws.com`\.
**Important**  
We recommend that you open your web application in a different browser window now so you can refresh it later to review the background color change made during the blue/green deployment\.
   + The name of the AWS CloudFormation stack used to create your environment, such as `BlueGreenLoadBalancer-abcdefh`\. When you are ready to clean up resources from your sample blue/green deployment, you will use the AWS CloudFormation console to delete this stack\.
   + The location of the sample application that will be installed in the original environment, such as:

     `[https://s3\.amazonaws\.com/aws\-codedeploy\-us\-east\-2/samples/latest/SampleApp\_Linux\.zip](https://s3.amazonaws.com/aws-codedeploy-us-east-2/samples/latest/SampleApp_Linux.zip)`

1. Choose **Start blue/green deployment**\.

## Step 4: Monitor the blue/green deployment<a name="getting-started-wizard-blue-green-4-monitor"></a>

On the **Deployment** page, you can view the progress of the blue/green deployment in a dashboard format\.

The **Deployment progress** area reports the progress of the four major steps in the deployment:
+ Provisioning instances in the replacement environment\.
+ Installing the new application revision in the replacement environment\.
+ Rerouting traffic to the replacement environment\.
+ Terminating the instances in the original environment\.

The **Instances receiving traffic** area reports the counts of instances in both the original and replacement environments that are currently registered with the load balancer\.

The **Deployment details** area lists identifying information about both the deployment and the application revision that was installed in the replacement environment\.

The **Instance activity** area provides details about each instance from both the original environment and replacement environment\.

## Step 5: Refresh the web application window<a name="getting-started-wizard-blue-green-5-refresh"></a>

In the browser window where you previously opened a view of the application that was installed in the original environment, such as such as `http://BlueGreenLoadBalancer-abcdefh-1234567890.us-east-2.elb.amazonaws.com`, choose the browser's **Refresh** button\.

If the background color of the web page changes from blue to green, traffic has been successfully routed from the instances we created for you in [Step 3: Create blue/green deployment](#getting-started-wizard-blue-green-3-create) to the replacement instances created during the process in [Step 4: Monitor the blue/green deployment](#getting-started-wizard-blue-green-4-monitor)

## Step 6: Clean up sample resources<a name="getting-started-wizard-blue-green-6-cleanup"></a>

To avoid future charges, you must clean up the resources used in this wizard\. The resources must be cleaned up in the following order:
+ The Auto Scaling group that the instances for the replacement environment belong to\. \(The Auto Scaling group associated with the instances in the original environment will be deleted when you delete the AWS CloudFormation stack\.\) 
+ The AWS CloudFormation stack that the Sample deployment wizard created to provide the original environment for the blue/green deployment\.
+ The AWS CodeDeploy deployment group and application created by the Sample deployment wizard\.

### To delete the Auto Scaling group for the replacement environment<a name="getting-started-wizard-blue-green-6-cleanup-asg"></a>

You will see two Auto Scaling groups associated with the sample blue/green deployment in the Amazon EC2 console\. To avoid errors, be sure to delete the Auto Scaling group associated with the replacement environment in this step\. You can distinguish the Auto Scaling groups by their formats:
+ Delete the Auto Scaling with this format:

  `CodeDeploy_BlueGreenDemoFleet-9zyxwvut_d-ZY9XWVUTS8R` 
+ You can delete the Auto Scaling group with this format now or let it be removed when the AWS CloudFormation stack is deleted:

  `CodeDeployBGStack-abcdefh-BlueGreenAutoScalingGroup-1IJKLMN234O56`

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

1. On the **Auto Scaling** groups page, select the box by the Auto Scaling group created for the replacement environment\. For example:

   `CodeDeploy_BlueGreenDemoFleet-9zyxwvut_d-ZY9XWVUTS8R`\. 

   From the **Actions** menu, choose **Delete**\. 

1. When prompted for confirmation, choose **Yes, Delete**\.

### To delete the AWS CloudFormation stack<a name="getting-started-wizard-blue-green-6-cleanup-stack"></a>

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Select the box next to the stack created for your blue/green deployment\. For example:

   `CodeDeployBGStack-abcdefh-BlueGreenAutoScalingGroup-1IJKLMN234O56`\. 

   From the **Actions** menu, choose **Delete Stack**\.

1. When prompted, choose **Yes, Delete**\. The remaining resources that were created for this deployment in Amazon EC2, AWS Identity and Access Management, Amazon VPC, and Elastic Load Balancing will be deleted\.

### To delete AWS CodeDeploy blue/green deployment resource records<a name="getting-started-wizard-blue-green-6-cleanup-codedeploy"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page does not appear, on the AWS CodeDeploy menu, choose **Applications**\.

1. On the **Applications** page, choose the application to delete, such as `BlueGreenDemoApplication`\. 

1. On the **Application details** page, in **Deployment groups**, choose the button next to the deployment group you want to delete, such as `BlueGreenDemoFleet-1abcdef`\. On the **Actions** menu, choose **Delete**\. When prompted, type the name of the deployment group to confirm you want to delete it, and then choose **Delete**\.

1. At the bottom of the **Application details** page, choose **Delete application**\.

1. When prompted, type the name of the application, and then choose **Delete**\. 

   All records about the application and its associated deployment groups, revisions, and deployments will be deleted\.
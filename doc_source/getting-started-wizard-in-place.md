# Try a Sample In\-Place Deployment in AWS CodeDeploy<a name="getting-started-wizard-in-place"></a>

This section guides you through the steps required to deploy a revision to one or more Amazon EC2 instances using the Sample deployment wizard\. 


+ [Video of a Sample AWS CodeDeploy In\-Place Deployment](#getting-started-wizard-in-place-video)
+ [Start the wizard](#getting-started-wizard-in-place-start)
+ [Step 1: Get started](#getting-started-wizard-in-place-1-get-started)
+ [Step 2: Choose a deployment type](#getting-started-wizard-in-place-2-deployment-type)
+ [Step 3: Configure instances](#getting-started-wizard-in-place-3-instances)
+ [Step 4: Name your application](#getting-started-wizard-in-place-4-application)
+ [Step 5: Select a revision](#getting-started-wizard-in-place-5-revision)
+ [Step 6: Create a deployment group](#getting-started-wizard-in-place-6-deployment-group)
+ [Step 7: Select a service role](#getting-started-wizard-in-place-7-service-role)
+ [Step 8: Choose a deployment configuration](#getting-started-wizard-in-place-9-deployment-configuration)
+ [Step 9: Review deployment details](#getting-started-wizard-in-place-9-review)
+ [Clean up sample in\-place deployment resources](#getting-started-wizard-in-place-clean-up)

## Video of a Sample AWS CodeDeploy In\-Place Deployment<a name="getting-started-wizard-in-place-video"></a>

This short video \(5:01\) walks you through a sample AWS CodeDeploy in\-place deployment using the AWS CodeDeploy console\.

[![AWS Videos](http://img.youtube.com/vi/jcR9iIWdU7E/0.jpg)](http://www.youtube.com/watch?v=jcR9iIWdU7E)

## Start the wizard<a name="getting-started-wizard-in-place-start"></a>

To start the wizard:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If an introductory page appears, choose **Get Started Now**\. If the **Applications** page appears, in **More info**, choose **Sample deployment wizard**\.

## Step 1: Get started<a name="getting-started-wizard-in-place-1-get-started"></a>

Choose **Sample deployment**, and then choose **Next**\. 

## Step 2: Choose a deployment type<a name="getting-started-wizard-in-place-2-deployment-type"></a>

Choose **In\-place deployment**, and then choose **Next**\. 

## Step 3: Configure instances<a name="getting-started-wizard-in-place-3-instances"></a>

If you have Amazon EC2 instances that are already configured for use in AWS CodeDeploy deployments, choose **Skip**, read and follow the instructions, and then proceed to [Step 4: Name your application](#getting-started-wizard-in-place-4-application)\. 

If you want AWS CodeDeploy to launch a new set of Amazon EC2 instances:

1. Next to **Operating system**, choose **Amazon Linux** or **Windows Server**\.
**Important**  
You may be billed for the Amazon EC2 instances launched by AWS CodeDeploy, so be sure to terminate them after you've completed the wizard\. In this wizard, an AWS CloudFormation template is used to launch these Amazon EC2 instances\. To delete the AWS CloudFormation stack created to launch the Amazon EC2 instances, see [Deleting a Stack on the AWS CloudFormation Console](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html)\. The stack name will start with **CodeDeploySampleStack**\. 

1. From the **Key pair name** drop\-down list, choose the Amazon EC2 instance key pair you will use to connect to the Amazon EC2 instances\.
**Note**  
To create an Amazon EC2 instance key pair, follow the instructions in [Creating Your Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure your key pair is created in one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. The new Amazon EC2 instance key pair might not appear in the **Key pair name** drop\-down list until you restart the wizard\.

1. Leave the defaults for **Tag key and value**\. AWS CodeDeploy will use this tag key and value to locate the instances during deployments\.

    If you want to override the proposed tag key and value \(for example, if you are running through this wizard multiple times without terminating any previously created Amazon EC2 instances\), we suggest you leave the tag key of `Name` in the **Key** box and type a different tag value in the **Value** box\. For information about Amazon EC2 instance tags, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

1. Choose **Launch instances**\. 

   If you choose **See more details in AWS CloudFormation**, the AWS CloudFormation console will open in a separate web browser tab\. Look for a stack that starts with **CodeDeploySampleStack**\. When **CREATE\_COMPLETE** appears in the **Status** column, your Amazon EC2 instances have been launched\. \(This might take several minutes\.\) 

1. To continue, choose **Next**\.

## Step 4: Name your application<a name="getting-started-wizard-in-place-4-application"></a>

In the **Application name** box, leave the proposed application name or, if you prefer, type a different name, and choose **Next**\.

## Step 5: Select a revision<a name="getting-started-wizard-in-place-5-revision"></a>

Review the information about our sample application revision, and choose **Next**\.

**Note**  
If you want to examine the content of our sample revision, choose **Download sample bundle**, and follow your web browser's instructions to download and view the content\.

If you chose **Skip** in [Step 3: Configure instances](#getting-started-wizard-in-place-3-instances), from the **Revision type** drop\-down list, choose the type of application revision that corresponds to the Amazon EC2 instances type \(Amazon Linux or Windows Server\)\. 

## Step 6: Create a deployment group<a name="getting-started-wizard-in-place-6-deployment-group"></a>

1. In the **Deployment group name** box, leave the proposed deployment group name or, if you prefer, type a different name\.

1. The key and value of the key\-value pair you specified in the **Configure instances** page \(for example, `Name` and `CodeDeployDemo`\) should appear\.

   If you chose **Skip** in [Step 3: Configure instances](#getting-started-wizard-in-place-3-instances), in **Add instances**, overwrite the values of the **Key** and **Value** boxes with the key and value of the key\-value pair for your Amazon EC2 instances\. 

   Optionally, if your Amazon EC2 instances have multiple key\-value pairs, you can type them into the blank row\. A new blank row appears so you can add another key\-value pair\. You can add up to 10 key\-value pairs\. Choose the remove icon to remove a key\-value pair from the list\.
**Note**  
AWS CodeDeploy displays the number of instances that match each key\-value pair\. To view instances in the Amazon EC2 console, click the number\.   
If you are using our AWS CloudFormation template to launch new Amazon EC2 instances, and the number is larger than you're expecting, choose **Cancel**, start the wizard from the beginning, and in [Step 3: Configure instances](#getting-started-wizard-in-place-3-instances), specify a tag value different from the default \. \(Be sure to delete the AWS CloudFormation stack to terminate the Amazon EC2 instances\.\)   
If you are using your own Amazon EC2 instances, add a new tag key and value to your Amazon EC2 instances, and then specify a tag key and value different from the default in **Add instances**\. 

1. If you have an Auto Scaling group to add to the deployment group, choose **Search by Auto Scaling group names**, and then type the Auto Scaling group name\. You can add up to 10 Auto Scaling groups\. Choose the remove icon to remove an Auto Scaling group from the list\.
**Note**  
AWS CodeDeploy displays the number of Amazon EC2 instances that match each Auto Scaling group name\. To view instances in the Amazon EC2 console, click the number\.

1. Choose **Next**\.

## Step 7: Select a service role<a name="getting-started-wizard-in-place-7-service-role"></a>

Choose **Create a service role** or **Use an existing service role**\. 

 If you are using this wizard for the first time, we recommend you choose **Create a service role**, choose **Next** to accept the default name, and then proceed to [Step 8: Choose a deployment configuration](#getting-started-wizard-in-place-9-deployment-configuration)\.

If you already have a service role, choose **Use an existing service role**, choose it from the **Role name** drop\-down list, and then choose **Next**\.

## Step 8: Choose a deployment configuration<a name="getting-started-wizard-in-place-9-deployment-configuration"></a>

1. To use a built\-in configuration for this deployment, choose **Use a default deployment configuration**\. To create your own configuration for this deployment, choose **Create a custom deployment configuration**\.

1. If you chose **Use a default deployment configuration** and want to use a configuration different from the one selected, next to the desired configuration, choose **Select**\. Choose **Next**, and go to [Step 9: Review deployment details](#getting-started-wizard-in-place-9-review)\.

1. If you chose **Create a custom deployment configuration**:

   1. In the **Deployment configuration name** box, type a unique name for the configuration\.

   1. Use the **Number** or **Percentage** box to type either the number or percentage of total Amazon EC2 instances that should be available during the deployment\.

   1. Choose **Next**\.

## Step 9: Review deployment details<a name="getting-started-wizard-in-place-9-review"></a>

1. If you need to make changes, choose one of the **Edit** links\. After you've made your changes, choose **Next** until you return to the **Review deployment details** page, and then choose **Deploy**\.

1. Choose the **Refresh** button next to the table to get deployment status\. To get information about the deployment, see [View Instance Details \(Console\)](instances-view-details.md#instances-view-details-console)\.

1. Our sample revision deploys a single web page to each instance\. You can use your web browser to verify the deployment was successful by going to `http://PublicDNS` for each instance \(for example, `http://ec2-01-234-567-890.compute-1.amazonaws.com`\)\. The web page will display a message of congratulations\.

   To get the public DNS value, in the Amazon EC2 console, choose the Amazon EC2 instance\. On the **Description** tab, look for the value in **Public DNS**\. 

## Clean up sample in\-place deployment resources<a name="getting-started-wizard-in-place-clean-up"></a>

To avoid future charges, you must clean up the resources used in this wizard\. If you used our AWS CloudFormation template to launch Amazon EC2 instances, delete the AWS CloudFormation stack\. This will terminate the instances and their associated resources\. 

If you launched your own Amazon EC2 instances just for this wizard, you should terminate them\. Optionally, you can delete the deployment component records associated with this wizard from the AWS CodeDeploy console\.

### To delete the AWS CloudFormation stack<a name="getting-started-wizard-in-place-clean-up-template"></a>

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose the button next to the stack starting with `CodeDeploySampleStack`\. On the **Actions** menu, choose **Delete Stack**\.

1. When prompted, choose **Yes, Delete**\. The Amazon EC2 instances will be terminated\. The associated IAM instance profile and service role will be deleted\.

### To terminate Amazon EC2 instances<a name="getting-started-wizard-in-place-clean-up-instances"></a>

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\.

1. Select the box for each Amazon EC2 instance to terminate\.

1. On the **Actions** menu, point to **Instance State**, and then choose **Terminate**\.

1. When prompted, choose **Yes, Terminate**\. 

### To delete AWS CodeDeploy deployment component records<a name="getting-started-wizard-in-place-clean-up-records"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. If the **Applications** page does not appear, on the AWS CodeDeploy menu, choose **Applications**\.

1. On the **Applications** page, choose the application to delete\. 

1. On the **Application details** page, in **Deployment groups**, choose the button next to the deployment group you want to delete\. On the **Actions** menu, choose **Delete**\. When prompted, type the name of the deployment group to confirm you want to delete it, and then choose **Delete**\.

1. At the bottom of the **Application details** page, choose **Delete application**\.

1. When prompted, type the name of the application, and then choose **Delete**\. 

   All records about the application and its associated deployment groups, revisions, and deployments will be deleted\.
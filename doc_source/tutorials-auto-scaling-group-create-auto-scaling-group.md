# Step 1: Create and configure the Amazon EC2 Auto Scaling group<a name="tutorials-auto-scaling-group-create-auto-scaling-group"></a>

In this step, you'll create an Amazon EC2 Auto Scaling group that contains a single Amazon Linux, RHEL, or Windows Server Amazon EC2 instance\. In a later step, you will instruct Amazon EC2 Auto Scaling to add one more Amazon EC2 instance, and CodeDeploy will deploy your revision to it\.

**Topics**
+ [To create and configure the Amazon EC2 Auto Scaling group \(CLI\)](#tutorials-auto-scaling-group-create-auto-scaling-group-cli)
+ [To create and configure the Amazon EC2 Auto Scaling group \(console\)](#tutorials-auto-scaling-group-create-auto-scaling-group-console)

## To create and configure the Amazon EC2 Auto Scaling group \(CLI\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-cli"></a>

1. Call the create\-launch\-configuration command to create an Amazon EC2 Auto Scaling launch configuration\.

   Before you call this command, you need the ID of an AMI that works for this tutorial, represented by the placeholder *image\-id*\. You also need the name of an Amazon EC2 instance key pair to enable access to the Amazon EC2 instance, represented by the placeholder *key\-name*\.

   To get the ID of an AMI that works with this tutorial:

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the navigation pane, under **Instances**, choose **Instances**, and then choose **Launch Instance**\.

   1. On the **Quick Start** tab of the **Choose an Amazon Machine Image** page, note the ID of the AMI next to **Amazon Linux AMI**, **Red Hat Enterprise Linux 7\.1**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2**\. 
**Note**  
If you have a custom version of an AMI that is compatible with CodeDeploy, choose it here instead of browsing through the **Quick Start** tab\. For information about using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling, see [Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

   For the Amazon EC2 instance key pair, use the name of your Amazon EC2 instance key pair\.

   Call the create\-launch\-configuration command\.

   On local Linux, macOS, or Unix machines:

   ```
   aws autoscaling create-launch-configuration \
     --launch-configuration-name CodeDeployDemo-AS-Configuration \
     --image-id image-id \
     --key-name key-name \
     --iam-instance-profile CodeDeployDemo-EC2-Instance-Profile \
     --instance-type t1.micro
   ```

   On local Windows machines:

   ```
   aws autoscaling create-launch-configuration --launch-configuration-name CodeDeployDemo-AS-Configuration --image-id image-id --key-name key-name --iam-instance-profile CodeDeployDemo-EC2-Instance-Profile --instance-type t1.micro
   ```

   These commands create an Amazon EC2 Auto Scaling launch configuration named **CodeDeployDemo\-AS\-Configuration**, based on the specified image ID, applying the specified IAM instance profile and Amazon EC2 instance key pair\. This launch configuration is based on the t1\.micro Amazon EC2 instance type\.

1.  Call the create\-auto\-scaling\-group command to create an Amazon EC2 Auto Scaling group\. You will need the name of one of the Availability Zones in one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*, represented by the placeholder *availability\-zone*\.
**Note**  
To view a list of Availability Zones in a region, call:   

   ```
   aws ec2 describe-availability-zones --region region-name
   ```
For example, to view a list of Availability Zones in the US West \(Oregon\) Region, call:  

   ```
   aws ec2 describe-availability-zones --region us-west-2
   ```
For a list of region name identifiers, see [Resource kit bucket names by Region](resource-kit.md#resource-kit-bucket-names)\.

   On local Linux, macOS, or Unix machines:

   ```
   aws autoscaling create-auto-scaling-group \
     --auto-scaling-group-name CodeDeployDemo-AS-Group \
     --launch-configuration-name CodeDeployDemo-AS-Configuration \
     --min-size 1 \
     --max-size 1 \
     --desired-capacity 1 \
     --availability-zones availability-zone \
     --tags Key=Name,Value=CodeDeployDemo,PropagateAtLaunch=true
   ```

   On local Windows machines:

   ```
   aws autoscaling create-auto-scaling-group --auto-scaling-group-name CodeDeployDemo-AS-Group --launch-configuration-name CodeDeployDemo-AS-Configuration --min-size 1 --max-size 1 --desired-capacity 1 --availability-zones availability-zone --tags Key=Name,Value=CodeDeployDemo,PropagateAtLaunch=true
   ```

   These commands create an Amazon EC2 Auto Scaling group named **CodeDeployDemo\-AS\-Group** based on the Amazon EC2 Auto Scaling launch configuration named **CodeDeployDemo\-AS\-Configuration**\. This Amazon EC2 Auto Scaling group has only one Amazon EC2 instance, and it is created in the specified Availability Zone\. Each instance in this Amazon EC2 Auto Scaling group will have the tag `Name=CodeDeployDemo`\. The tag will be used when installing the CodeDeploy agent later\.

1. Call the describe\-auto\-scaling\-groups command against **CodeDeployDemo\-AS\-Group**:

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].[HealthStatus, LifecycleState]" --output text
   ```

   Do not proceed until the returned values show `Healthy` and `InService`\.

1.  The instances in your Auto Scaling group must have the CodeDeploy agent installed to be used in CodeDeploy deployments\. Install the CodeDeploy agent by calling the create\-association command from AWS Systems Manager with the tags that were added when the Amazon EC2 Auto Scaling group was created\. 

   ```
   aws ssm create-association \
     --name AWS-ConfigureAWSPackage \
     --targets Key=tag:Name,Values=CodeDeployDemo \
      --parameters action=Install, name=AWSCodeDeployAgent \
     --schedule-expression "cron(0 2 ? * SUN *)"
   ```

   This command creates an association in Systems Manager State Manager that will install the CodeDeploy agent on all instances in the Amazon EC2 Auto Scaling group and then attempt to update it at 2:00 every Sunday morning\. For more information about the CodeDeploy agent, see [ Working with the CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent.html)\. For more information about Systems Manager, see [What is AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)\.

## To create and configure the Amazon EC2 Auto Scaling group \(console\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-console"></a>

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the global navigation bar, make sure one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference* is selected\. Amazon EC2 Auto Scaling resources are tied to the region you specify, and CodeDeploy is supported in select regions only\.

1. In the navigation bar, under **Auto Scaling**, choose **Launch Configurations**\.

1. Choose **Create launch configuration**\.

1. On the **Quick Start** tab of the **Choose AMI** page, next to **Amazon Linux AMI**, **Red Hat Enterprise Linux 7\.2**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2 Base**, choose **Select**\.
**Note**  
If you have a custom version of an AMI that already has the CodeDeploy agent installed, choose it here instead\. For information about using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling, see [Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

1. On the **Choose Instance Type** page, leave the defaults, and choose **Next: Configure details**\.

1. On the **Configure details** page, in **Name**, type **CodeDeployDemo\-AS\-Configuration**\. In **IAM role**, choose the IAM instance profile you created earlier \(**CodeDeployDemo\-EC2\-Instance\-Profile**\)\.

   Leave the rest of the defaults, and choose **Skip to review**\.

1. On the **Review** page, choose **Create launch configuration**\.
**Note**  
In a production environment, we recommend that you restrict access to Amazon EC2 instances\. For more information, see [Tips for securing your EC2 instance](https://aws.amazon.com/articles/1233)\.

1. In the **Select an existing key pair or create a new key pair** dialog box, select **Choose an existing key pair**\. In the **Select a key pair** drop\-down list, choose the Amazon EC2 instance key pair you created or used in previous steps\. Select **I acknowledge that I have access to the selected private key file \(*key\-file\-name*\.pem\), and that without this file, I won't be able to log into my instance**, and then choose **Create launch configuration**\. 

1. Choose **Create an Auto Scaling group using this launch configuration**\.

1. On the **Configure Auto Scaling group details** page, do the following:

   1. For **Group name**, type **CodeDeployDemo\-AS\-Group**\.

   1.  Keep **Group size** set to the default value of 1 for this tutorial\. 

   1.  In **Network**, choose **Launch into EC2\-Classic**\. If it does not appear and you are not able to select a default virtual private cloud \(VPC\), choose or create a VPC and subnet\. For more information, see [Your VPC and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\. 

   1.  For **Subnet**, choose a subnet for the VPC\. 
**Note**  
 You can choose the Availability Zone for your instance by choosing its corresponding default subnet\. 

   1. Choose **Next: Configure scaling policies**\.

1. On the **Configure scaling policies** page, leave **Keep this group at its initial size** selected, and choose **Next: Configure Notifications**\.

1. Skip the step for configuring notifications, and choose **Next: Configure Tags**\.

1. In **Key**, enter **Name**\. In **Value**, enter **CodeDeployDemo**\. The tag will be used when installing the CodeDeploy agent later\.

1. Choose **Create Auto Scaling group**, and then choose **Close**\.

1. In the navigation bar, with **Auto Scaling Groups** selected, choose **CodeDeployDemo\-AS\-Group**, and then choose the **Instances** tab\. Do not proceed until the value of **InService** appears in the **Lifecycle** column and the value of **Healthy** appears in the **Health Status** column\.

1. Install the CodeDeploy agent by following the steps in [ Install the CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install.html) and using the `Name=CodeDeployDemo` instance tags\.
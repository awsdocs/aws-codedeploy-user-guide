# Step 1: Create and configure the Auto Scaling group<a name="tutorials-auto-scaling-group-create-auto-scaling-group"></a>

In this step, you'll create an Auto Scaling group that contains a single Amazon Linux, RHEL, or Windows Server Amazon EC2 instance\. In a later step, you will instruct Amazon EC2 Auto Scaling to add one more Amazon EC2 instance, and CodeDeploy will deploy your revision to it\.

**Topics**
+ [To create and configure the Auto Scaling group \(CLI\)](#tutorials-auto-scaling-group-create-auto-scaling-group-cli)
+ [To create and configure the Auto Scaling group \(console\)](#tutorials-auto-scaling-group-create-auto-scaling-group-console)

## To create and configure the Auto Scaling group \(CLI\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-cli"></a>

1. Call the create\-launch\-template command to create an Amazon EC2 launch template\.

   Before you call this command, you need the ID of an AMI that works for this tutorial, represented by the placeholder *image\-id*\. You also need the name of an Amazon EC2 instance key pair to enable access to the Amazon EC2 instance, represented by the placeholder *key\-name*\.

   To get the ID of an AMI that works with this tutorial:

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the navigation pane, under **Instances**, choose **Instances**, and then choose **Launch Instance**\.

   1. On the **Quick Start** tab of the **Choose an Amazon Machine Image** page, note the ID of the AMI next to **Amazon Linux 2 AMI**, **Red Hat Enterprise Linux 7\.1**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2**\. 
**Note**  
If you have a custom version of an AMI that is compatible with CodeDeploy, choose it here instead of browsing through the **Quick Start** tab\. For information about using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling, see [Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

   For the Amazon EC2 instance key pair, use the name of your Amazon EC2 instance key pair\.

   Call the create\-launch\-template command\.

   On local Linux, macOS, or Unix machines:

   ```
   aws ec2 create-launch-template \
     --launch-template-name CodeDeployDemo-AS-Launch-Template \
     --launch-template-data file://config.json
   ```

   The contents of the `config.json` file:

   ```
   { 
     "InstanceType":"t1.micro",
     "ImageId":"image-id",
     "IamInstanceProfile":{
       "Name":"CodeDeployDemo-EC2-Instance-Profile"
     },
     "KeyName":"key-name"
   }
   ```

   On local Windows machines:

   ```
   aws ec2 create-launch-template --launch-template-name CodeDeployDemo-AS-Launch-Template --launch-template-data file://config.json
   ```

   The contents of the `config.json` file:

   ```
   { 
     "InstanceType":"t1.micro",
     "ImageId":"image-id",
     "IamInstanceProfile":{
       "Name":"CodeDeployDemo-EC2-Instance-Profile"
     },
     "KeyName":"key-name"
   }
   ```

   These commands, along with the `config.json` file, create an Amazon EC2 launch template named CodeDeployDemo\-AS\-Launch\-Template for your Auto Scaling group that will be created in a following step based on the t1\.micro Amazon EC2 instance type\. Based on your input for `ImageId`, `IamInstanceProfile`, and `KeyName`, the launch template also specifies the AMI ID, the name of the instance profile associated with the IAM role to pass to instances at launch, and the Amazon EC2 key pair to use when connecting to instances\.

1.  Call the create\-auto\-scaling\-group command to create an Auto Scaling group\. You will need the name of one of the Availability Zones in one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*, represented by the placeholder *availability\-zone*\.
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
     --launch-template CodeDeployDemo-AS-Launch-Template,Version='$Latest' \
     --min-size 1 \
     --max-size 1 \
     --desired-capacity 1 \
     --availability-zones availability-zone \
     --tags Key=Name,Value=CodeDeployDemo,PropagateAtLaunch=true
   ```

   On local Windows machines:

   ```
   aws autoscaling create-auto-scaling-group --auto-scaling-group-name CodeDeployDemo-AS-Group --launch-template CodeDeployDemo-AS-Launch-Template,Version="$Latest" --min-size 1 --max-size 1 --desired-capacity 1 --availability-zones availability-zone --tags Key=Name,Value=CodeDeployDemo,PropagateAtLaunch=true
   ```

   These commands create an Auto Scaling group named **CodeDeployDemo\-AS\-Group** based on the Amazon EC2 launch template named **CodeDeployDemo\-AS\-Launch\-Template**\. This Auto Scaling group has only one Amazon EC2 instance, and it is created in the specified Availability Zone\. Each instance in this Auto Scaling group will have the tag `Name=CodeDeployDemo`\. The tag will be used when installing the CodeDeploy agent later\.

1. Call the describe\-auto\-scaling\-groups command against **CodeDeployDemo\-AS\-Group**:

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].[HealthStatus, LifecycleState]" --output text
   ```

   Do not proceed until the returned values show `Healthy` and `InService`\.

1.  The instances in your Auto Scaling group must have the CodeDeploy agent installed to be used in CodeDeploy deployments\. Install the CodeDeploy agent by calling the create\-association command from AWS Systems Manager with the tags that were added when the Auto Scaling group was created\. 

   ```
   aws ssm create-association \
     --name AWS-ConfigureAWSPackage \
     --targets Key=tag:Name,Values=CodeDeployDemo \
      --parameters action=Install, name=AWSCodeDeployAgent \
     --schedule-expression "cron(0 2 ? * SUN *)"
   ```

   This command creates an association in Systems Manager State Manager that will install the CodeDeploy agent on all instances in the Auto Scaling group and then attempt to update it at 2:00 every Sunday morning\. For more information about the CodeDeploy agent, see [ Working with the CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent.html)\. For more information about Systems Manager, see [What is AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)\.

## To create and configure the Auto Scaling group \(console\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-console"></a>

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the global navigation bar, make sure one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference* is selected\. Amazon EC2 Auto Scaling resources are tied to the region you specify, and CodeDeploy is supported in select regions only\.

1. In the navigation bar, under **Instances**, choose **Launch Templates**\.

1. Choose **Create launch template**\.

1. In the **Launch template name and description** dialog, for **Launch template name**, enter **CodeDeployDemo\-AS\-Launch\-Template**\. Leave the defaults for the other fields\.

1. In the **Amazon machine image \(AMI\)** dialog, click the dropdown under **AMI**, choose an AMI that works with this tutorial:

   1. On the **Quick Start** tab of the **AMI** dropdown, choose one of the following: **Amazon Linux 2 AMI**, **Red Hat Enterprise Linux 7\.1**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2**\. 
**Note**  
If you have a custom version of an AMI that is compatible with CodeDeploy, choose it here instead of browsing through the **Quick Start** tab\. For information about using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling, see [Using a custom AMI with CodeDeploy and Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

1. In **Instance type**, select the dropdown and choose **t1\.micro**\. You can use the search bar to find it more quickly\.

1. In the **Key pair \(login\)** dialog box, select **Choose an existing key pair**\. In the **Select a key pair** drop\-down list, choose the Amazon EC2 instance key pair you created or used in previous steps\.

1. In the **Network settings** dialog box, choose **Virtual Public Cloud \(VPC\)**\.

   In the **Security groups** dropdown, choose the security group you created in the [tutorial's prerequisites section](https://docs.aws.amazon.com/codedeploy/latest/userguide/tutorials-auto-scaling-group-prerequisites.html) \(**CodeDeployDemo\-AS\-SG**\)\.

1. Expand the **Advanced details** dialog box\. In the **IAM instance profile** dropdown, select the IAM role you created earlier \(**CodeDeployDemo\-EC2\-Instance\-Profile**\) under **IAM instance profile**\.

   Leave the rest of the defaults\.

1. Choose **Create launch template**\.

1. In the **Next steps** dialog box, choose **Create Auto Scaling group**\.

1. On the **Choose launch template or configuration** page, for **Auto Scaling group name**, type **CodeDeployDemo\-AS\-Group**\.

1. In the **Launch template** dialog box, your launch template \(**CodeDeployDemo\-AS\-Launch\-Template**\) should be filled in, if not, select it from the dropdown menu\. Leave the defaults and choose **Next**\. 

1. On the **Choose instance launch options page** page, in the **Network** section, for **VPC**, choose the default VPC\. Then for **Availability Zones and subnets**, choose a default subnet\. You must create a VPC if you cannot choose the default\. For more information, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html)\.

1. In the **Instance type requirements** section, use the default setting to simplify this step\. \(Do not override the launch template\.\) For this tutorial, you will launch only On\-Demand Instances using the instance type specified in your launch template\.

1. Choose **Next** to go to the **Configure advanced options** page\.

1. Keep the default values and choose **Next**\.

1. On the **Configure group size and scaling policies** page, keep the default **Group size** values of 1\. Choose **Next**\.

1. Skip the step for configuring notifications, and choose **Next**\.

1. On the **Add tags** page, add a tag to be used when installing the CodeDeploy agent later\. Choose **Add tag**\.

   1. In **Key**, enter **Name**\.

   1. In **Value**, enter **CodeDeployDemo**\.

   Choose **Next**\.

1. Review your Auto Scaling group information on the **Review** page, then choose **Create Auto Scaling group**\.

1. In the navigation bar, with **Auto Scaling Groups** selected, choose **CodeDeployDemo\-AS\-Group**, and then choose the **Instance Management** tab\. Do not proceed until the value of **InService** appears in the **Lifecycle** column and the value of **Healthy** appears in the **Health Status** column\.

1. Install the CodeDeploy agent by following the steps in [ Install the CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install.html) and using the `Name=CodeDeployDemo` instance tags\.
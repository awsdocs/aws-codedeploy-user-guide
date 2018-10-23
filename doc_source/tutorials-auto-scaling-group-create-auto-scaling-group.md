# Step 1: Create and Configure the Auto Scaling Group<a name="tutorials-auto-scaling-group-create-auto-scaling-group"></a>

In this step, you'll create an Auto Scaling group that contains a single Amazon Linux, RHEL, or Windows Server Amazon EC2 instance\. In a later step, you will instruct Auto Scaling to add one more Amazon EC2 instance, and AWS CodeDeploy will deploy your revision to it\.

**Topics**
+ [To create and configure the Auto Scaling group \(CLI\)](#tutorials-auto-scaling-group-create-auto-scaling-group-cli)
+ [To create and configure the Auto Scaling group \(console\)](#tutorials-auto-scaling-group-create-auto-scaling-group-console)

## To create and configure the Auto Scaling group \(CLI\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-cli"></a>

1. Call the create\-launch\-configuration command to create an Auto Scaling launch configuration\.

   Before you call this command, you need the ID of an AMI that works for this tutorial, represented by the placeholder *image\-id*\. You also need the name of an Amazon EC2 instance key pair to enable access to the Amazon EC2 instance, represented by the placeholder *key\-name*\. Finally, you need instructions to install the latest version of the AWS CodeDeploy agent\.

   AWS CodeDeployTo get the ID of an AMI that works with this tutorial:

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the navigation pane, under **Instances**, choose **Instances**, and then choose **Launch Instance**\.

   1. On the **Quick Start** tab of the **Choose an Amazon Machine Image** page, note the ID of the AMI next to **Amazon Linux AMI**, **Red Hat Enterprise Linux 7\.1**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2**\. 
**Note**  
If you have a custom version of an AMI that is compatible with AWS CodeDeploy, choose it here instead of browsing through the **Quick Start** tab\. For information about using a custom AMI with AWS CodeDeploy and Auto Scaling, see [Using a Custom AMI with AWS CodeDeploy and Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

   For the Amazon EC2 instance key pair, use the name of your Amazon EC2 instance key pair\.

   To install the latest version of the AWS CodeDeploy agent, on your development machine, create a file named `instance-setup.sh` \(for an Amazon Linux, Ubuntu Server or RHEL Amazon EC2 instance\) or `instance-setup.txt` \(for a Windows Server Amazon EC2 instance\) with the following contents\.
**Note**  
If you have a custom version of an AMI that is compatible with AWS CodeDeploy, you don't need to create the `instance-setup.sh` or `instance-setup.txt` file\. 

   **On Amazon Linux and RHEL Amazon EC2 instances**

   ```
   #!/bin/bash
   yum -y update
   yum install -y ruby
   cd /home/ec2-user
   curl -O https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **On Ubuntu Server Amazon EC2 instances**

   ```
   #!/bin/bash
   apt-get -y update
   apt-get -y install ruby
   apt-get -y install wget
   cd /home/ubuntu
   wget https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **On Windows Server Amazon EC2 instances**

   ```
   <powershell>  
   New-Item -Path c:\temp -ItemType "directory" -Force
   powershell.exe -Command Read-S3Object -BucketName bucket-name/latest -Key codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
   Start-Process -Wait -FilePath c:\temp\codedeploy-agent.msi -WindowStyle Hidden
   </powershell>
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   Call the create\-launch\-configuration command\.

   On local Linux, macOS, or Unix machines:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws autoscaling create-launch-configuration \
     --launch-configuration-name CodeDeployDemo-AS-Configuration \
     --image-id image-id \
     --key-name key-name \
     --iam-instance-profile CodeDeployDemo-EC2-Instance-Profile \
     --instance-type t1.micro \ 
     --user-data file://path/to/instance-setup.sh
   ```

   On local Windows machines:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws autoscaling create-launch-configuration --launch-configuration-name CodeDeployDemo-AS-Configuration --image-id image-id --key-name key-name --iam-instance-profile CodeDeployDemo-EC2-Instance-Profile --instance-type t1.micro --user-data file://path/to/instance-setup.txt
   ```
**Note**  
If you have a custom version of an AMI that is compatible with AWS CodeDeploy, omit the \-\-user\-data option in the preceding command\. 

   These commands create an Auto Scaling launch configuration named **CodeDeployDemo\-AS\-Configuration**, based on the specified image ID, applying the specified IAM instance profile and Amazon EC2 instance key pair, and running the command to install the latest version of the AWS CodeDeploy agent\. This launch configuration is based on the t1\.micro Amazon EC2 instance type\.

1.  Call the create\-auto\-scaling\-group command to create an Auto Scaling group\. You will need the name of one of the Availability Zones in one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*, represented by the placeholder *availability\-zone*\.
**Note**  
To view a list of Availability Zones in a region, call:   

   ```
   aws ec2 describe-availability-zones --region region-name
   ```
For example, to view a list of Availability Zones in the US West \(Oregon\) Region, call:  

   ```
   aws ec2 describe-availability-zones --region us-west-2
   ```
For a list of region name identifiers, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   On local Linux, macOS, or Unix machines:

   ```
   aws autoscaling create-auto-scaling-group \
     --auto-scaling-group-name CodeDeployDemo-AS-Group \
     --launch-configuration-name CodeDeployDemo-AS-Configuration \
     --min-size 1 \
     --max-size 1 \
     --desired-capacity 1 \
     --availability-zones availability-zone
   ```

   On local Windows machines:

   ```
   aws autoscaling create-auto-scaling-group --auto-scaling-group-name CodeDeployDemo-AS-Group --launch-configuration-name CodeDeployDemo-AS-Configuration --min-size 1 --max-size 1 --desired-capacity 1 --availability-zones availability-zone
   ```

   These commands create an Auto Scaling group named **CodeDeployDemo\-AS\-Group** based on the Auto Scaling launch configuration named **CodeDeployDemo\-AS\-Configuration**\. This Auto Scaling group has only one Amazon EC2 instance, and it is created in the specified Availability Zone\.

1. Call the describe\-auto\-scaling\-groups command against **CodeDeployDemo\-AS\-Group**:

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].[HealthStatus, LifecycleState]" --output text
   ```

   Do not proceed until the returned values show `Healthy` and `InService`\.

## To create and configure the Auto Scaling group \(console\)<a name="tutorials-auto-scaling-group-create-auto-scaling-group-console"></a>

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the global navigation bar, make sure one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference* is selected\. Auto Scaling resources are tied to the region you specify, and AWS CodeDeploy is supported in select regions only\.

1. In the navigation bar, under **Auto Scaling**, choose **Launch Configurations**\.

1. Choose **Create launch configuration**\.

1. On the **Quick Start** tab of the **Choose AMI** page, next to **Amazon Linux AMI**, **Red Hat Enterprise Linux 7\.2**, **Ubuntu Server 14\.04 LTS**, or **Microsoft Windows Server 2012 R2 Base**, choose **Select**\.
**Note**  
If you have a custom version of an AMI that already has the AWS CodeDeploy agent installed, choose it here instead\. For information about using a custom AMI with AWS CodeDeploy and Auto Scaling, see [Using a Custom AMI with AWS CodeDeploy and Auto Scaling](integrations-aws-auto-scaling.md#integrations-aws-auto-scaling-custom-ami)\.

1. On the **Choose Instance Type** page, leave the defaults, and choose **Next: Configure details**\.

1. On the **Configure details** page, in **Name**, type **CodeDeployDemo\-AS\-Configuration**\. In **IAM role**, choose the IAM instance profile you created earlier \(**CodeDeployDemo\-EC2\-Instance\-Profile**\)\.

   Expand **Advanced Details**, and in **User data**, type the following\.
**Note**  
If you are using a custom version of an AMI that already has the AWS CodeDeploy agent installed, skip this step\. 

   **For Amazon Linux and RHEL Amazon EC2 instances**

   ```
   #!/bin/bash
   yum -y update
   yum install -y ruby
   cd /home/ec2-user
   curl -O https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Ubuntu Server Amazon EC2 instances**

   ```
   #!/bin/bash
   apt-get -y update
   apt-get -y install ruby
   apt-get -y install wget
   cd /home/ubuntu
   wget https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Windows Server Amazon EC2 instances**

   ```
   <powershell>  
   New-Item -Path c:\temp -ItemType "directory" -Force
   powershell.exe -Command Read-S3Object -BucketName bucket-name/latest -Key codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
   Start-Process -Wait -FilePath c:\temp\codedeploy-agent.msi -WindowStyle Hidden
   </powershell>
   ```

   *bucket\-name* is the name of the Amazon S3 sds\-s3\-latest\-bucket\-name bucket that contains the AWS CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   Leave the rest of the defaults, and choose **Skip to review**\.

1. On the **Review** page, choose **Create launch configuration**\.
**Note**  
In a production environment, we recommend that you restrict access to Amazon EC2 instances\. For more information, see [Tips for Securing Your EC2 Instance](https://aws.amazon.com/articles/1233)\.

1. In the **Select an existing key pair or create a new key pair** dialog box, select **Choose an existing key pair**\. In the **Select a key pair** drop\-down list, choose the Amazon EC2 instance key pair you created or used in previous steps\. Select **I acknowledge that I have access to the selected private key file \(*key\-file\-name*\.pem\), and that without this file, I won't be able to log into my instance**, and then choose **Create launch configuration**\. 

1. Choose **Create an Auto Scaling group using this launch configuration**\.

1. On the **Configure Auto Scaling group details** page, in **Group name**, type **CodeDeployDemo\-AS\-Group**\. In **Group size**, leave the default\. In the **Availability Zone\(s\)** box, choose an Availability Zone in one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. Leave the rest of the defaults, and choose **Next: Configure scaling policies**\.
**Note**  
If **Launch into EC2\-Classic** does not appear in the **Network** list, and you are not able to select a default virtual private cloud \(VPC\), choose or create a VPC and subnet\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

1. On the **2\. Configure scaling policies** page, leave **Keep this group at its initial size** selected, and choose **Next: Configure Notifications**\.

1. Skip the step for configuring notifications, and choose **Review**\.

1. Choose **Create Auto Scaling group**, and then choose **Close**\.

1. In the navigation bar, with **Auto Scaling Groups** selected, choose **CodeDeployDemo\-AS\-Group**, and then choose the **Instances** tab\. Do not proceed until the value of **InService** appears in the **Lifecycle** column and the value of **Healthy** appears in the **Health Status** column\.
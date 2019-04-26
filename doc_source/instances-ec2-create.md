# Create an Amazon EC2 Instance for CodeDeploy \(AWS CLI or Amazon EC2 Console\)<a name="instances-ec2-create"></a>

These instructions show you how to launch a new Amazon EC2 instance that is configured for use in CodeDeploy deployments\.

You can use our AWS CloudFormation template to launch an Amazon EC2 instance running Amazon Linux or Windows Server that is already configured for use in CodeDeploy deployments\. We do not provide an AWS CloudFormation template for Amazon EC2 instances running Ubuntu Server or Red Hat Enterprise Linux \(RHEL\)\. For alternatives to the use of the template, see [Working with Instances for CodeDeploy](instances.md)\.

You can use the Amazon EC2 console, AWS CLI, or Amazon EC2 APIs to launch an Amazon EC2 instance\.

## Launch an Amazon EC2 Instance \(Console\)<a name="instances-ec2-create-console"></a>

If you have not done so already, follow the instructions in [Getting Started with CodeDeploy](getting-started-codedeploy.md) to set up and configure the AWS CLI and create an IAM instance profile\.

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**, and then choose **Launch Instance**\.

1. On the **Step 1: Choose an Amazon Machine Image \(AMI\)** page, from the **Quick Start** tab, locate the operating system and version you want to use, and then choose **Select**\. You must choose an Amazon EC2 AMI operating systems supported by CodeDeploy\. For more informaction, see [Operating Systems Supported by the CodeDeploy Agent](codedeploy-agent.md#codedeploy-agent-supported-operating-systems)\.

1. On the **Step 2: Choose an Instance Type** page, choose any available Amazon EC2 instance type, and then choose **Next: Configure Instance Details**\.

1. On the **Step 3: Configure Instance Details** page, in the **IAM role** list, choose the IAM instance role you created in [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\. If you used the suggested role name, then choose **CodeDeployDemo\-EC2\-Instance\-Profile**\. If you created your own role name, choose that\.
**Note**  
If **Launch into EC2\-Classic** or a default virtual private cloud \(VPC\) are not displayed in the **Network** list, and you cannot choose a different Amazon EC2 instance type that supports launching into EC2\-Classic, you must choose or create an Amazon VPC and subnet\. Choose **Create new VPC** or **Create new subnet** or both\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

1. Expand **Advanced Details**\.

1. Next to **User data**, with the **As text** option selected, type the following to install the CodeDeploy agent as the Amazon EC2 instance is launched\.

   **For Amazon Linux and RHEL**

   ```
   #!/bin/bash
   yum -y update
   yum install -y ruby
   cd /home/ec2-user
   curl -O https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Ubuntu Server**
**Important**  
If you are installing the CodeDeploy agent on Ubuntu Server 14\.04, change the third line the following:  
`apt-get -y install ruby2.0`

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

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Windows Server**

   ```
   <powershell>  
   New-Item -Path c:\temp -ItemType "directory" -Force
   powershell.exe -Command Read-S3Object -BucketName bucket-name/latest -Key codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
   Start-Process -Wait -FilePath c:\temp\codedeploy-agent.msi -WindowStyle Hidden
   </powershell>
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

1. Leave the rest of the items on this page unchanged, and choose **Next: Add Storage**\.

1. Leave the **Step 4: Add Storage** page unchanged, and choose **Next: Add Tags**\.

1. On the **Step 5: Add Tags** page, choose **Add Tag**\. 

1.  In the **Key** box, type **Name**\. In the **Value** box type **CodeDeployDemo**\. 
**Important**  
The contents of the **Key** and **Value** boxes are case\-sensitive\.

1. Choose **Next: Configure Security Group**\.

1. On the **Step 6: Configure Security Group** page, leave the **Create a new security group** option selected\.

   A default SSH role is configured for Amazon EC2 instances running Amazon Linux, Ubuntu Server, or RHEL\. A default RDP role is configured for Amazon EC2 instances running Windows Server\.

1. If you want to open the HTTP port, choose the **Add Rule** button, and from the **Type** drop\-down list, choose **HTTP**\. Accept the default **Source** value of **Anywhere 0\.0\.0\.0/0**, and then choose **Review and Launch**\.
**Note**  
In a production environment, we recommend restricting access to the SSH, RDP, and HTTP ports, instead of specifying **Anywhere 0\.0\.0\.0/0**\. CodeDeploy does not require unrestricted port access and does not require HTTP access\. For more information, see [Tips for Securing Your Amazon EC2 Instance](https://aws.amazon.com/articles/1233)\.

   If a **Boot from General Purpose \(SSD\)** dialog box appears, follow the instructions, and then choose **Next**\.

1. Leave the **Step 7: Review Instance Launch** page unchanged, and choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose either **Choose an existing key pair** or **Create a new key pair**\. If you've already configured an Amazon EC2 instance key pair, you can choose it here\.

   If you don't already have an Amazon EC2 instance key pair, choose **Create a new key pair** and give it a recognizable name\. Choose **Download Key Pair** to download the Amazon EC2 instance key pair to your computer\.
**Important**  
You must have a key pair if you want to access your Amazon EC2 instance with SSH or RDP\.

1. Choose **Launch Instances**\.

1. Choose the ID for your Amazon EC2 instance\. Do not continue until the instance has been launched and passed all checks\.

To verify the CodeDeploy agent is running on the instance, see [Verify the CodeDeploy Agent Is Running](codedeploy-agent-operations-verify.md), and then return to this page\. After you do this, the Amazon EC2 instance is ready for use in CodeDeploy deployments\.

## Launch an Amazon EC2 Instance \(CLI\)<a name="instances-ec2-create-cli"></a>

If you have not done so already, follow the instructions in [Getting Started with CodeDeploy](getting-started-codedeploy.md) to set up and configure the AWS CLI and create an IAM instance profile\.

1. **For Windows Server only** If you are creating an Amazon EC2 instance running Windows Server, call the create\-security\-group and authorize\-security\-group\-ingress commands to create a security group that allows RDP access \(which is not allowed by default\) and, alternatively, HTTP access\. For example, to create a security group named *CodeDeployDemo\-Windows\-Security\-Group*, run the following commands, one at a time:

   ```
   aws ec2 create-security-group --group-name CodeDeployDemo-Windows-Security-Group --description "For launching Windows Server images for use with CodeDeploy"
   ```

   ```
   aws ec2 authorize-security-group-ingress --group-name CodeDeployDemo-Windows-Security-Group --to-port 3389 --ip-protocol tcp --cidr-ip 0.0.0.0/0 --from-port 3389
   ```

   ```
   aws ec2 authorize-security-group-ingress --group-name CodeDeployDemo-Windows-Security-Group --to-port 80 --ip-protocol tcp --cidr-ip 0.0.0.0/0 --from-port 80
   ```
**Note**  
For demonstration purposes, these commands create a security group that allows unrestricted access for RDP through port 3389 and, alternatively, HTTP through port 80\. As a best practice, we recommend restricting access to the RDP and HTTP ports\. CodeDeploy does not require unrestricted port access and does not require HTTP access\. For more information, see [Tips for Securing Your Amazon EC2 Instance](https://aws.amazon.com/articles/1233)\.

1. On your development machine, create a file named `instance-setup.sh` \(for Amazon EC2 instances running Amazon Linux, Ubuntu Server, or RHEL\) or `instance-setup.txt` \(for Amazon EC2 instances running Windows Server\) that contains the following contents\. 

   As the Amazon EC2 instance is launched, this script downloads the CodeDeploy agent from the specified Amazon S3 location and then installs it on the instance\.

   **For Amazon Linux and RHEL**

   Here is the content of `instance-setup.sh` for Amazon Linux and RHEL: 

   ```
   #!/bin/bash
   yum -y update
   yum install -y ruby
   cd /home/ec2-user
   curl -O https://bucket-name.s3.amazonaws.com/latest/install
   chmod +x ./install
   ./install auto
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Ubuntu Server**

   Here is the content of `instance-setup.sh` for Ubuntu Server:
**Important**  
If you are installing the CodeDeploy agent on Ubuntu Server 14\.04, change the third line of the file to the following:  
`apt-get -y install ruby2.0`

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

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

   **For Windows Server**

   Here is the content of `instance-setup.txt` for Windows Server:

   ```
   <powershell>  
   New-Item -Path c:\temp -ItemType "directory" -Force
   powershell.exe -Command Read-S3Object -BucketName bucket-name/latest -Key codedeploy-agent.msi -File c:\temp\codedeploy-agent.msi
   Start-Process -Wait -FilePath c:\temp\codedeploy-agent.msi -WindowStyle Hidden
   </powershell>
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

1. From the same directory where you created the `instance-setup.sh` or `instance-setup.txt` file, you call the run\-instances command to create and launch the Amazon EC2 instance\.

   Before you call this command, you need to collect the following: 
   + The ID of an Amazon Machine Image \(AMI\) \(*ami\-id*\) you use for the instance\. To get the ID, see [Finding a Suitable AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html)\.
   + The name of the type of Amazon EC2 instance \(*instance\-type*\) you create, such as `t1.micro`\. For a list, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.
   + The name of an IAM instance profile with permission to access the Amazon S3 bucket where the CodeDeploy agent installation files for your region are stored\. 

     For information about creating an IAM instance profile, see [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\.
   + The name of an Amazon EC2 instance key pair \(*key\-name*\) to enable SSH access to an Amazon EC2 instance running Amazon Linux, Ubuntu Server, or RHEL or RDP access to an Amazon EC2 instance running Windows Server\.
**Important**  
Type the key pair name only, not the key pair file extension\. For example, *my\-keypair*, not *my\-keypair\.pem*\.

     To find a key pair name, open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2](https://console.aws.amazon.com/ec2)\. In the navigation pane, under **Network & Security**, choose **Key Pairs**, and note the key pair name in the list\. 

     To generate a key pair, see [Creating Your Key Pair Using Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure you create the key pair in one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\. Otherwise, you won't be able to use the Amazon EC2 instance key pair with CodeDeploy\.

   **For Amazon Linux, RHEL, and Ubuntu Server**

   To call the run\-instances command to launch an Amazon EC2 instance running Amazon Linux, Ubuntu Server, or RHEL and attach the IAM instance profile you created in [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\. For example:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws ec2 run-instances \
     --image-id ami-id \
     --key-name key-name \
     --user-data file://instance-setup.sh \
     --count 1 \
     --instance-type instance-type \
     --iam-instance-profile Name=iam-instance-profile
   ```
**Note**  
This command creates a default security group for the Amazon EC2 instance that allows access to several ports, including unrestricted access for SSH through port 22 and, alternatively, HTTP through port 80\. As a best practice, we recommend restricting access to the SSH and HTTP ports only\. CodeDeploy does not require unrestricted port access and does not require HTTP port access\. For more information, see [Tips for Securing Your Amazon EC2 Instance](https://aws.amazon.com/articles/1233)\.

   **For Windows Server**

   To call the run\-instances command to launch an Amazon EC2 instance running Windows Server and attach the IAM instance profile you created in [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md), and specify the name of the security group you created in Step 1\. For example:
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   ```
   aws ec2 run-instances --image-id ami-id --key-name key-name --user-data file://instance-setup.txt --count 1 --instance-type instance-type --iam-instance-profile Name=iam-instance-profile --security-groups CodeDeploy-Windows-Security-Group
   ```

   These commands launch a single Amazon EC2 instance with the specified AMI, key pair, and instance type, with the specified IAM instance profile, and run the specified script during launch\. 

1. Note the value of the `InstanceID` in the output\. If you forget this value, you can get it later by calling the describe\-instances command against the Amazon EC2 instance key pair\.

   ```
   aws ec2 describe-instances --filters "Name=key-name,Values=keyName" --query "Reservations[*].Instances[*].[InstanceId]" --output text
   ```

   Use the instance ID to call the create\-tags command, which tags the Amazon EC2 instance so that CodeDeploy can find it later during a deployment\. In the following example, the tag is named **CodeDeployDemo**, but you can specify any Amazon EC2 instance tag you want\.

   ```
   aws ec2 create-tags --resources instance-id --tags Key=Name,Value=CodeDeployDemo
   ```

   You can apply multiple tags to an instance at the same time\. For example:

   ```
   aws ec2 create-tags --resources instance-id --tags Key=Name,Value=testInstance Key=Region,Value=West Key=Environment,Value=Beta
   ```

   To verify the Amazon EC2 instance has been launched and passed all checks, use the instance ID to call the describe\-instance\-status command\. 

   ```
   aws ec2 describe-instance-status --instance-ids instance-id --query "InstanceStatuses[*].InstanceStatus.[Status]" --output text 
   ```

If the instance has been launched and passed all checks, `ok` appears in the output:

To verify the CodeDeploy agent is running on the instance, see [Verify the CodeDeploy Agent Is Running](codedeploy-agent-operations-verify.md), and then return to this page\. After you do this, the Amazon EC2 instance is ready for use in CodeDeploy deployments\.
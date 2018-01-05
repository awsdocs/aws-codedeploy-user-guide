# Step 1: Launch a Windows Server Amazon EC2 Instance<a name="tutorials-windows-launch-instance"></a>

To deploy the "Hello, World\!" application with AWS CodeDeploy, you'll need an Amazon EC2 instance running Windows Server\.

Follow the instructions in [Working with Instances for AWS CodeDeploy](instances.md)\. When you get to the part in those instructions about assigning an Amazon EC2 instance tag to the instance, be sure to specify the tag key of **Name** and the tag value of **CodeDeployDemo**\. \(If you specify a different tag key or tag value, then the instructions in [Step 4: Deploy Your "Hello, World\!" Application](tutorials-windows-deploy-application.md) may produce unexpected results\.\)

After you've followed the instructions to launch the Amazon EC2 instance, return to this page, and continue to the next section\. Do not continue on to [Create an Application with AWS CodeDeploy](applications-create.md) as a next step\.

## Connect to Your Amazon EC2 Instance<a name="tutorials-windows-launch-instance-connect"></a>

After your Amazon EC2 instance is launched, follow these instructions to practice connecting to it\. 

**Note**  
In these instructions, we assume you are running Windows and the Windows Desktop Connection client application\. For information, see [Connecting to Your Windows Instance Using RDP](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\. You may need to adapt these instructions for other operating systems or other RDP connection client applications\.

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\. 

1. Browse to and choose your Windows Server instance in the list\.

1. Choose **Connect**\.

1. Choose **Get Password**\.

1. Choose **Browse**\. Browse to and choose the Amazon EC2 instance key pair file associated with the Windows Server Amazon EC2 instance, and then choose **Open**\.

1. Choose **Decrypt Password**\. Make a note of the password that is displayed\.

1. Choose **Download Remote Desktop File**, and then open the file\.

1. If you are prompted to connect even though the publisher of the remote connection can't be identified, proceed\.

1. When prompted for a password, type the password you noted in step 7, and then proceed\. \(If your RDP connection client application prompts you for a user name, type **Administrator**\.\)

1. If you are prompted to connect even though the identify of the remote computer cannot be verified, proceed\. 

1. After you are connected, the desktop of the Amazon EC2 instance running Windows Server is displayed\.

1. You can now sign out of the running Amazon EC2 instance\.
**Warning**  
Do not stop or terminate the instance\. Otherwise, AWS CodeDeploy won't be able to deploy to it\.
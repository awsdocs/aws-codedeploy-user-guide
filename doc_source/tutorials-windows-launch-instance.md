# Step 1: Launch a Windows Server Amazon EC2 instance<a name="tutorials-windows-launch-instance"></a>

To deploy the Hello World application with CodeDeploy, you need an Amazon EC2 instance running Windows Server\.

Follow the instructions in [Create an Amazon EC2 instance for CodeDeploy](instances-ec2-create.md)\. When you are ready to assign an Amazon EC2 instance tag to the instance, be sure to specify the tag key of **Name** and the tag value of **CodeDeployDemo**\. \(If you specify a different tag key or tag value, then the instructions in [Step 4: Deploy your Hello World application](tutorials-windows-deploy-application.md) might produce unexpected results\.\)

After you've launched the Amazon EC2 instance, return to this page, and continue to the next section\. Do not continue on to [Create an application with CodeDeploy](applications-create.md) as a next step\.

## Connect to your Amazon EC2 instance<a name="tutorials-windows-launch-instance-connect"></a>

After your Amazon EC2 instance is launched, follow these instructions to practice connecting to it\. 

**Note**  
In these instructions, we assume you are running Windows and the Windows Desktop Connection client application\. For information, see [Connecting to your Windows instance using RDP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html)\. You might need to adapt these instructions for other operating systems or other RDP connection client applications\.

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\. 

1. Browse to and choose your Windows Server instance in the list\.

1. Choose **Connect**\.

1. Choose **Get Password**, and then choose **Choose File**\.

1. Browse to and choose the Amazon EC2 instance key pair file associated with the Windows Server Amazon EC2 instance, and then choose **Open**\.

1. Choose **Decrypt Password**\. Make a note of the password that is displayed\. You need it in step 10\.

1. Choose **Download Remote Desktop File**, and then open the file\.

1. If you are prompted to connect even though the publisher of the remote connection can't be identified, proceed\.

1. Type the password you noted in step 7, and then proceed\. \(If your RDP connection client application prompts you for a user name, type **Administrator**\.\)

1. If you are prompted to connect even though the identity of the remote computer cannot be verified, proceed\. 

1. After you are connected, the desktop of the Amazon EC2 instance running Windows Server is displayed\.

1. You can now disconnect from the Amazon EC2 instance\.
**Warning**  
Do not stop or terminate the instance\. Otherwise, CodeDeploy can't deploy to it\.

## Add an inbound rule that allows HTTP traffic to your Windows Server Amazon EC2 instance<a name="tutorials-windows-launch-instance-add-inbound-rule"></a>

The next step confirms your Amazon EC2 instance has an open HTTP port so you can see the deployed webpage on your Windows Server Amazon EC2 instance in a browser\. 

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Instances**, and then choose your instance\. 

1. On the **Description** tab, under **Security groups**, choose **view inbound rules**\. 

   You should see a list of rules in your security group like the following:

   ```
   Security Groups associated with i-1234567890abcdef0
    Ports     Protocol     Source     launch-wizard-N
    22        tcp          0.0.0.0/0          ✔
   ```

1.  Under **Security groups**, choose the security group for your Amazon EC2 instance\. It might be named **launch\-wizard\-*N***\. The ***N*** in the name is a number assigned to your security group when your instance was created\. 

    Choose the **Inbound** tab\. If the security group for your instance is configured correctly, you should see a rule with the following values: 
   + **Type**: HTTP
   + **Protocol**: TCP
   + **Port Range**: 80
   + **Source**: 0\.0\.0\.0/0

1.  If you do not see a rule with these values, use the procedures in [Adding Rules to a Security Group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule) to add them to a new security rule\. 
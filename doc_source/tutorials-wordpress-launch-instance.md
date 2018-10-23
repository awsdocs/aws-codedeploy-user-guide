# Step 1: Launch and Configure an Amazon Linux or Red Hat Enterprise Linux Amazon EC2 Instance<a name="tutorials-wordpress-launch-instance"></a>

To deploy the WordPress application with AWS CodeDeploy, you'll need an Amazon EC2 instance running Amazon Linux or Red Hat Enterprise Linux \(RHEL\)\. The Amazon EC2 instance requires a new inbound security rule that allows HTTP connections\. This rule is needed in order to view the WordPress page in a browser after it is successfully deployed\.

Follow the instructions in [Working with Instances for AWS CodeDeploy](instances.md)\. When you get to the part in those instructions about assigning an Amazon EC2 instance tag to the instance, be sure to specify the tag key of **Name** and the tag value of **CodeDeployDemo**\. \(If you specify a different tag key or tag value, then the instructions in [Step 4: Deploy Your WordPress Application](tutorials-wordpress-deploy-application.md) may produce unexpected results\.\)

After you've followed the instructions to launch the Amazon EC2 instance, return to this page, and continue to the next section\. Do not continue on to [Create an Application with AWS CodeDeploy](applications-create.md) as a next step\.

## Connect to Your Amazon Linux or RHEL Amazon EC2 Instance<a name="tutorials-wordpress-launch-instance-connect"></a>

After your new Amazon EC2 instance is launched, follow these instructions to practice connecting to it\.

1. Use the ssh command \(or an SSH\-capable terminal emulator like [PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)\) to connect to your Amazon Linux or RHEL Amazon EC2 instance\. You will need the public DNS address of the instance and the private key for the key pair you used when you started the Amazon EC2 instance\. For more information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-connect-to-instance-linux.html)\.

   For example, if the public DNS address is **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**, and your Amazon EC2 instance key pair for SSH access is named **codedeploydemo\.pem**, you would type:

   ```
   ssh -i /path/to/codedeploydemo.pem ec2-user@ec2-01-234-567-890.compute-1.amazonaws.com
   ```

   Replace `/path/to/codedeploydemo.pem` with the path to your `.pem` file and the example DNS address with the address to your Amazon Linux or RHEL Amazon EC2 instance\.
**Note**  
If you receive an error about your key file's permissions being too open, you will need to restrict its permissions to give access only to the current user \(you\)\. For example, with the chmod command on Linux, macOS, or Unix, type:

   ```
   chmod 400 /path/to/codedeploydemo.pem
   ```

1. After you are signed in, you will see the AMI banner for the Amazon EC2 instance\. For Amazon Linux, it should look like this:

   ```
          __|  __|_  )
          _|  (     /   Amazon Linux AMI
         ___|\___|___|
   ```

1.  Confirm the AWS CodeDeploy agent installed properly when you setup your Amazon EC2 instance by typing the following: 

   ```
   sudo service codedeploy-agent status
   ```

   For more information about determining the status of the AWS CodeDeploy agent, see [Verify the AWS CodeDeploy Agent Is Running](codedeploy-agent-operations-verify.md)\.

   If the AWS CodeDeploy agent is not installed, follow the instructions in [Install or reinstall the AWS CodeDeploy agent for Amazon Linux or RHEL](codedeploy-agent-operations-install-linux.md)\.

1. You can now sign out of the running Amazon EC2 instance\.
**Warning**  
Do not stop or terminate the Amazon EC2 instance\. Otherwise, AWS CodeDeploy won't be able to deploy to it\.

## Add Inbound Rule Allowing HTTP Traffic to Your Amazon Linux or RHEL Amazon EC2 Instance<a name="tutorials-wordpress-launch-instance-add-inbound-rule"></a>

The next step confirms your Amazon EC2 instance has an open HTTP port so you can see the deployed WordPress application's home page in a browser\. 

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Instances** and select your instance\. 

1.  Under **Security groups**, choose **view inbound rules**\. 

   You should see a list of rules in your security group similar to the following:

   ```
   Security Groups associated with i-1234567890abcdef0
    Ports     Protocol     Source     launch-wizard-N
    22        tcp          0.0.0.0/0          ✔
   ```

1.  Under **Security groups**, choose your Amazon EC2 instance's security group\. It may be called **launch\-wizard\-*N***\. The ***N*** in the name is a number assigned to your security group when your instance was created\. 

    Choose the **Inbound** tab\. Your instance's security group is properly configured if you see a rule with the following values: 
   + **Type**: HTTP
   + **Protocol**: TCP
   + **Port Range**: 80
   + **Source**: Custom

1.  If you do not see a rule with the values listed in the previous step, use the procedures in [Adding Rules to a Security Group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#adding-security-group-rule) to add them to a new security rule\. 
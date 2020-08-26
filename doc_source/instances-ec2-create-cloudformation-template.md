# Create an Amazon EC2 instance for CodeDeploy \(AWS CloudFormation template\)<a name="instances-ec2-create-cloudformation-template"></a>

You can use our AWS CloudFormation template to quickly launch an Amazon EC2 instance running Amazon Linux or Windows Server\. You can use the AWS CLI, the CodeDeploy console, or the AWS APIs to launch the instance with the template\. In addition to launching the instance, the template does the following:
+ Instructs AWS CloudFormation to give the instance permission to participate in CodeDeploy deployments\.
+ Tags the instance so CodeDeploy can find it during a deployment\.
+ Installs and runs the CodeDeploy agent on the instance\.

You don't have to use our AWS CloudFormation to set up an Amazon EC2 instance\. For alternatives, see [Working with instances for CodeDeploy](instances.md)\.

We do not provide an AWS CloudFormation template for Amazon EC2 instances running Ubuntu Server or Red Hat Enterprise Linux \(RHEL\)\.

**Important**  
If you use the AWS CloudFormation template to launch Amazon EC2 instances, the calling IAM user must have access to AWS CloudFormation and AWS services and actions on which AWS CloudFormation depends\. If you have not followed the steps in [Step 1: Provision an IAM user](getting-started-provision-user.md) to provision the calling IAM user, you must at least attach the following policy:  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "codedeploy:*",
                "ec2:*",
                "iam:AddRoleToInstanceProfile",
                "iam:CreateInstanceProfile",
                "iam:CreateRole",
                "iam:DeleteInstanceProfile",
                "iam:DeleteRole",
                "iam:DeleteRolePolicy",
                "iam:GetRole",
                "iam:PassRole",
                "iam:PutRolePolicy",
                "iam:RemoveRoleFromInstanceProfile"
            ],
            "Resource": "*"
        }
    ]
}
```

**Topics**
+ [Launch an Amazon EC2 instance with the AWS CloudFormation template \(console\)](#instances-ec2-create-cloudformation-template-console)
+ [Launch an Amazon EC2 instance with the AWS CloudFormation template \(AWS CLI\)](#instances-ec2-create-cloudformation-template-cli)

## Launch an Amazon EC2 instance with the AWS CloudFormation template \(console\)<a name="instances-ec2-create-cloudformation-template-console"></a>

Before you begin, you must have an instance key pair to enable SSH access to the Amazon EC2 instance running Amazon Linux or RDP access to the instance running Windows Server\. Type the key pair name only, not the key pair file extension\. 

To find a key pair name, open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2](https://console.aws.amazon.com/ec2)\. In the navigation pane, under **Network & Security**, choose **Key Pairs**, and note the key pair name in the list\. 

To generate a new key pair, see [Creating your key pair using Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure the key pair is created in one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\. Otherwise, you can't use the instance key pair with CodeDeploy\.

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.
**Important**  
Sign in to the AWS Management Console with the same account you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in *AWS General Reference*\. CodeDeploy supports these regions only\.

1. Choose **Create Stack**\.

1. In **Choose a template**, choose **Specify an Amazon S3 template URL**\. In the box, type the location of the AWS CloudFormation template for your region, and then choose **Next**\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-create-cloudformation-template.html)

1. In the **Stack name** box, type a name for the stack \(for example, **CodeDeployDemoStack**\)\.

1. In **Parameters**, type the following, and then choose **Next**\.
   + For **InstanceCount**, type the number of instances you want to launch\. \(We recommend you leave the default of **1**\.\)
   + For **InstanceType**, type the instance type you want to launch \(or leave the default of **t1\.micro**\)\.
   + For **KeyPairName**, type the instance key name\.
   + For **OperatingSystem** box, type **Windows** to launch instances running Windows Server \(or leave the default of **Linux**\)\.
   + For **SSHLocation**, type the IP address range to use for connecting to the instance with SSH or RDP \(or leave the default of **0\.0\.0\.0/0**\)\.
**Important**  
The default of **0\.0\.0\.0/0** is provided for demonstration purposes only\. CodeDeploy does not require Amazon EC2 instances to have unrestricted access to ports\. As a best practice, we recommend restricting access to SSH \(and HTTP\) ports\. For more information, see [Tips for securing your Amazon EC2 instance](https://aws.amazon.com/articles/1233)\.
   + For **TagKey**, type the instance tag key CodeDeploy will use to identify the instances during deployment \(or leave the default of **Name**\)\.
   + For **TagValue**, type the instance tag value CodeDeploy will use to identify the instances during deployment \(or leave the default of **CodeDeployDemo**\)\.

1. On the **Options** page, leave the option boxes blank, and choose **Next**\.
**Important**  
AWS CloudFormation tags are different from CodeDeploy tags\. AWS CloudFormation uses tags to simplify administration of your infrastructure\. CodeDeploy uses tags to identify Amazon EC2 instances\. You specified CodeDeploy tags on the **Specify Parameters** page\.

1. On the **Review** page, in **Capabilities**, select the **I acknowledge that AWS CloudFormation might create IAM resources** box, and then choose **Create**\.

   After AWS CloudFormation has created the stack and launched the Amazon EC2 instances, in the AWS CloudFormation console, **CREATE\_COMPLETE** will be displayed in the **Status** column\. This process can take several minutes\.

To verify the CodeDeploy agent is running on the Amazon EC2 instances, see [Managing CodeDeploy agent operations](codedeploy-agent-operations.md), and then proceed to [Create an application with CodeDeploy](applications-create.md)\.

## Launch an Amazon EC2 instance with the AWS CloudFormation template \(AWS CLI\)<a name="instances-ec2-create-cloudformation-template-cli"></a>

Follow the instructions in [Getting started with CodeDeploy](getting-started-codedeploy.md) to install and configure the AWS CLI for use with CodeDeploy\. 

Before you call the create\-stack command, you must have an Amazon EC2 instance key pair to enable SSH access to the Amazon EC2 instance running Amazon Linux or RDP access to the Amazon EC2 instance running Windows Server\. Type the key pair name only, not the key pair file extension\. 

To find a key pair name, open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2](https://console.aws.amazon.com/ec2)\. In the navigation pane, under **Network & Security**, choose **Key Pairs**, and note the key pair name in the list\. 

To generate a key pair, see [Creating your key pair using Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure the key pair is created in one of the regions listed in [Region and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. Otherwise, you can't use the instance key pair with CodeDeploy\.

1. Use our AWS CloudFormation template in a call to the create\-stack command\. This stack will launch a new Amazon EC2 instance with the CodeDeploy agent installed\.

   To launch an Amazon EC2 instance running Amazon Linux:

   ```
   aws cloudformation create-stack \
     --stack-name CodeDeployDemoStack \
     --template-url templateURL \
     --parameters ParameterKey=InstanceCount,ParameterValue=1 ParameterKey=InstanceType,ParameterValue=t1.micro \
       ParameterKey=KeyPairName,ParameterValue=keyName ParameterKey=OperatingSystem,ParameterValue=Linux \
       ParameterKey=SSHLocation,ParameterValue=0.0.0.0/0 ParameterKey=TagKey,ParameterValue=Name \
       ParameterKey=TagValue,ParameterValue=CodeDeployDemo \
     --capabilities CAPABILITY_IAM
   ```

   To launch an Amazon EC2 instance running Windows Server: 

   ```
   aws cloudformation create-stack --stack-name CodeDeployDemoStack --template-url template-url --parameters ParameterKey=InstanceCount,ParameterValue=1 ParameterKey=InstanceType,ParameterValue=t1.micro ParameterKey=KeyPairName,ParameterValue=keyName ParameterKey=OperatingSystem,ParameterValue=Windows ParameterKey=SSHLocation,ParameterValue=0.0.0.0/0 ParameterKey=TagKey,ParameterValue=Name ParameterKey=TagValue,ParameterValue=CodeDeployDemo --capabilities CAPABILITY_IAM
   ```

   *template\-url* is the location of the AWS CloudFormation template for your region:  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-create-cloudformation-template.html)

   This command creates an AWS CloudFormation stack named **CodeDeployDemoStack**, using the AWS CloudFormation template in the specified Amazon S3 bucket\. The Amazon EC2 instance is based on the t1\.micro instance type, but you can use any type\. It is tagged with the value **CodeDeployDemo**, but you can tag it with any value\. It has the specified instance key pair applied\.

1. Call the describe\-stacks command to verify the AWS CloudFormation stack named **CodeDeployDemoStack** was successfully created:

   ```
   aws cloudformation describe-stacks --stack-name CodeDeployDemoStack --query "Stacks[0].StackStatus" --output text
   ```

   Do not proceed until the value `CREATE_COMPLETE` is returned\.

To verify the CodeDeploy agent is running on the Amazon EC2 instance, see [Managing CodeDeploy agent operations](codedeploy-agent-operations.md), and then proceed to [Create an application with CodeDeploy](applications-create.md)\.
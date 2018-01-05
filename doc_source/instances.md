# Working with Instances for AWS CodeDeploy<a name="instances"></a>

AWS CodeDeploy supports deployments to instances running Amazon Linux, Ubuntu Server, Red Hat Enterprise Linux \(RHEL\), and Windows Server\. 

You can use AWS CodeDeploy to deploy to both Amazon EC2 instances and on\-premises instances\. An on\-premises instance is any physical device that is not an Amazon EC2 instance and that can run the AWS CodeDeploy agent and connect to public AWS service endpoints\. You can use AWS CodeDeploy to simultaneously deploy an application to Amazon EC2 instances running in the cloud and to desktop PCs running in your office or servers in your own data center\. 

## Comparing Amazon EC2 Instances to On\-Premises Instances<a name="instances-comparison"></a>

The following table compares Amazon EC2 instances and on\-premises instances:


| **Subject** | **Amazon EC2 Instances** | **On\-Premises Instances** | 
| --- | --- | --- | 
|  Requires you to install and run a version of the AWS CodeDeploy agent that's compatible with the operating system running on the instance\.  | Yes |  Yes  | 
|  Requires the instance to be able to connect to the AWS CodeDeploy service\.  |  Yes  |  Yes  | 
|  Requires an IAM instance profile to be attached to the instance\. The IAM instance profile must have permissions to participate in AWS CodeDeploy deployments\. For information, see [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\.  |  Yes  |  No  | 
|  Requires you to do one of the following to authenticate and register instances: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/instances.html)  |  No  |  Yes  | 
|  Requires you to register each instance with AWS CodeDeploy before you can deploy to it\.  |  No  |  Yes  | 
|  Requires you to tag each instance before AWS CodeDeploy can deploy to it\.  |  Yes  |  Yes  | 
|  Can participate in Auto Scaling and Elastic Load Balancing scenarios as part of AWS CodeDeploy deployments\.  |  Yes  |  No  | 
|  Can be deployed from Amazon S3 buckets and GitHub repositories\.  |  Yes  |  Yes  | 
|  Can support triggers that prompt the sending of SMS or email notifications when specified events occur in deployments or instances\.  |  Yes  |  Yes  | 
|  Is subject to being billed for associated deployments\.  |  No  |  Yes  | 

## Instance Tasks for AWS CodeDeploy<a name="instances-task-list"></a>

To launch or configure instances for use in deployments, choose from the following instructions:


|  |  | 
| --- |--- |
|  I want to launch a new Amazon Linux or Windows Server Amazon EC2 instance\.  |  To launch the Amazon EC2 instance with the least amount of effort, see [Create an Amazon EC2 Instance for AWS CodeDeploy \(AWS CloudFormation Template\)](instances-ec2-create-cloudformation-template.md)\. To launch the Amazon EC2 instance mostly on your own, see [Create an Amazon EC2 Instance for AWS CodeDeploy \(AWS CLI or Amazon EC2 Console\)](instances-ec2-create.md)\.  | 
|  I want to launch a new Ubuntu Server or RHEL Amazon EC2 instance\.  |  See [Create an Amazon EC2 Instance for AWS CodeDeploy \(AWS CLI or Amazon EC2 Console\)](instances-ec2-create.md)\.  | 
| I want to configure an Amazon Linux, Windows Server, Ubuntu Server, or RHEL Amazon EC2 instance\. | See [Configure an Amazon EC2 Instance to Work with AWS CodeDeploy](instances-ec2-configure.md)\. | 
| I want to configure a Windows Server, Ubuntu Server, or RHEL on\-premises instance \(physical devices that are not Amazon EC2 instances\)\. | See [Working with On\-Premises Instances for AWS CodeDeploy](instances-on-premises.md)\. | 
| I want AWS CodeDeploy to provision a replacement fleet of instances during a blue/green deployment\. | See [Working with Deployments in AWS CodeDeploy](deployments.md)\. | 

To prepare Amazon EC2 instances in Auto Scaling groups, you must follow some additional steps\. For more information, see [Integrating AWS CodeDeploy with Auto Scaling](integrations-aws-auto-scaling.md)\.

**Topics**

+ [Tagging Instances for AWS CodeDeploy Deployments](instances-tagging.md)

+ [Working with Amazon EC2 Instances](instances-ec2.md)

+ [Working with On-Premises Instances](instances-on-premises.md)

+ [View Instance Details](instances-view-details.md)

+ [Instance Health](instances-health.md)
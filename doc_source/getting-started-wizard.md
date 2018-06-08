# Step 5: Try the AWS CodeDeploy Sample Deployment Wizard<a name="getting-started-wizard"></a>

**Note**  
 The AWS CodeDeploy Sample Deployment Wizard currently does not apply to deployments that use the AWS Lambda compute platform\. 

After you have completed the first four steps in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md), try the Sample deployment wizard\. It guides you through the steps for creating an AWS CodeDeploy deployment\. The Sample deployment wizard lets you try an in\-place deployment and a blue/green deployment\. 
+ **In\-place deployment**: The application on each instance in the deployment group is stopped, the latest application revision is installed, and the new version of the application is started and validated\. You can use a load balancer so that each instance is deregistered during its deployment and then restored to service after the deployment is complete\. Only deployments that use the EC2/On\-Premises compute platform can use in\-place deployments\. For more information about in\-place deployments, see [Overview of an In\-Place Deployment](welcome.md#welcome-deployment-overview-in-place)\.
+ **Blue/green deployment**: The behavior of your deployment depends on which compute platform you use:
  + **Blue/green on an EC2/On\-Premises compute platform**: The instances in a deployment group \(the original environment\) are replaced by a different set of instances \(the replacement environment\) using these steps:
    + Instances are provisioned for the replacement environment\.
    + The latest application revision is installed on the replacement instances\.
    + An optional wait time occurs for activities such as application testing and system verification\.
    + Instances in the replacement environment are registered with an Elastic Load Balancing load balancer, causing traffic to be rerouted to them\. Instances in the original environment are deregistered and can be terminated or kept running for other uses\.
**Note**  
When using an EC2/On\-Premises compute platform, blue/green deployments work with Amazon EC2 instances only\.
  + **Blue/green on an AWS Lambda compute platform**: Traffic is shifted from your current serverless environment to one with your updated Lambda function versions\. You can specify Lambda functions that perform validation tests and choose the way in which the traffic shift occurs\. All AWS Lambda compute platform deployments are blue/green deployments\. For this reason, you do not need to specify a deployment type\. 

  For more information about blue/green deployments, see [Overview of a Blue/Green Deployment](welcome.md#welcome-deployment-overview-blue-green)\.

For both deployment type samples, we assume you have no prior experience with AWS CodeDeploy and have not yet created any resources, such as applications, application revisions, or deployment groups in AWS CodeDeploy\.

These topics refer to resources and concepts that are unique to AWS CodeDeploy\. To familiarize yourself with them before you start, see [AWS CodeDeploy Primary Components](primary-components.md)\. 

## Prerequisites<a name="getting-started-wizard-prerequisites"></a>

If you want AWS CodeDeploy to create some sample Amazon EC2 instances, you must have an Amazon EC2 instance key pair\. To create an Amazon EC2 instance key pair, follow the instructions in [Creating Your Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair)\. Be sure your Amazon EC2 instance key pair is created in one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. You must create an Amazon EC2 instance key pair before you start the wizard\. Otherwise, it will not appear in the **Key pair name** drop\-down list in the Sample deployment wizard\.

If you use the AWS CloudFormation template to launch Amazon EC2 instances, the calling IAM user must have access to AWS CloudFormation and AWS services and actions on which AWS CloudFormation depends\. If you have not followed the steps in [Step 1: Provision an IAM User](getting-started-provision-user.md) to provision the calling IAM user, you must at least attach the following policy:

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

The following portion of the policy grants the calling IAM user access to the IAM actions required to create the service role\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateRole",
        "iam:PutRolePolicy"
      ],
      "Resource": "*"
    }
  ]
}
```

 The following portion of the policy grants the calling IAM user permission to create applications and deployment groups and deploy applications\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codedeploy:*"
      ],
      "Resource": "*"
    }
  ]
}
```

Not what you're looking for?
+ To create a deployment that uses an existing application, revision, deployment group, or custom deployment configuration in AWS CodeDeploy, follow the instructions in [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.
+ To practice deploying to on\-premises instances instead of Amazon EC2 instances, see [Tutorial: Deploy an Application to an On\-Premises Instance with AWS CodeDeploy \(Windows Server, Ubuntu Server, or Red Hat Enterprise Linux\)](tutorials-on-premises-instance.md)\.

**Topics**
+ [Prerequisites](#getting-started-wizard-prerequisites)
+ [Try a Sample Blue/Green Deployment in AWS CodeDeploy](getting-started-wizard-blue-green.md)
+ [Try a Sample In\-Place Deployment in AWS CodeDeploy](getting-started-wizard-in-place.md)
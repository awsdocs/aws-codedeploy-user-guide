# AWS CloudFormation templates for CodeDeploy reference<a name="reference-cloudformation-templates"></a>

This section introduces the AWS CloudFormation resources, transform, and hook designed to work with CodeDeploy deployments\. For a walkthrough of creating a stack update managed by the AWS CloudFormation hook for CodeDeploy, see [Create an Amazon ECS blue/green deployment through AWS CloudFormation](deployments-create-ecs-cfn.md)

**Note**  
AWS CloudFormation hooks are part of the AWS CloudFormation components for AWS and are different from CodeDeploy lifecycle event hooks\.

In addition to the other methods available to you in CodeDeploy, you can use AWS CloudFormation templates to perform the following tasks:
+ Create applications\.
+ Create deployment groups and specify a target revision\.
+ Create deployment configurations\.
+ Create Amazon EC2 instances\.

AWS CloudFormation is a service that helps you model and set up your AWS resources using templates\. An AWS CloudFormation template is a text file whose format complies with the JSON standard\. You create a template that describes all of the AWS resources you want, and AWS CloudFormation takes care of provisioning and configuring those resources for you\.

For more information, see [What is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) and [Working with AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in *AWS CloudFormation User Guide*\. 

If you plan to use AWS CloudFormation templates that are compatible with CodeDeploy in your organization, as an administrator, you must grant access to AWS CloudFormation and to the AWS services and actions on which AWS CloudFormation depends\. To grant permissions to create applications, deployment groups, and deployment configurations, attach the following policy to the IAM users who will work with AWS CloudFormation: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [                
        "cloudformation:*"        
      ],
      "Resource": "*"
    }
  ]
}
```

For more information about managed policies, see the following topics:
+ To view the policy that must be attached to IAM users who will create Amazon EC2 instances, see [Create an Amazon EC2 instance for CodeDeploy \(AWS CloudFormation template\)](instances-ec2-create-cloudformation-template.md)\.
+ For information about attaching policies to IAM users, see [Working with managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html) in *IAM User Guide*\. 
+ To learn how to restrict users to a limited set of CodeDeploy actions and resources, see [AWS managed \(predefined\) policies for CodeDeploy](security_iam_id-based-policy-examples.md#managed-policies)\.

The following table shows the actions an AWS CloudFormation template can perform on your behalf and includes links to more information about the AWS resource types and their property types you can add to an AWS CloudFormation template\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/reference-cloudformation-templates.html)
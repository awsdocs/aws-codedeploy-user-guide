# Using Identity\-Based Policies \(IAM Policies\) for CodeDeploy<a name="auth-and-access-control-iam-identity-based-access-control"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on CodeDeploy resources\. For information about the policy that must be attached to an IAM user in order to use CodeDeploy, see [Step 1: Provision an IAM User](getting-started-provision-user.md)\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CodeDeploy resources\. For more information, see [Overview of Managing Access Permissions to Your CodeDeploy Resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions Required to Use the CodeDeploy Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for CodeDeploy](#managed-policies)
+ [Customer Managed Policy Examples](#customer-managed-policies)

The following shows an example of a permissions policy that allows a user to delete the deployment group named **WordPress\_DepGroup** associated with the application named **WordPress\_App** in the **us\-west\-2** region\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:DeleteDeploymentGroup"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:deploymentgroup:WordPress_App/WordPress_DepGroup"
      ]
    }
  ]
}
```

## Permissions Required to Use the CodeDeploy Console<a name="console-permissions"></a>

For a user to work with the CodeDeploy console, that user must have a minimum set of permissions that allows the user to describe other AWS resources for their AWS account\. In order to use fully use CodeDeploy in the CodeDeploy console, you must have permissions from the following services:
+ Amazon EC2 Auto Scaling
+ AWS CodeDeploy
+ Amazon Elastic Compute Cloud
+ Elastic Load Balancing
+ AWS Identity and Access Management
+ Amazon Simple Storage Service
+ Amazon Simple Notification Service
+ Amazon CloudWatch

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the CodeDeploy console, also attach the `AWSCodeDeployReadOnlyAccess` managed policy to the user, as described in [AWS Managed \(Predefined\) Policies for CodeDeploy](#managed-policies)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the CodeDeploy API\.

## AWS Managed \(Predefined\) Policies for CodeDeploy<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to CodeDeploy:
+ **AWSCodeDeployFullAccess** – Grants full access to CodeDeploy\.
**Note**  
AWSCodeDeployFullAccess does not provide permissions to operations in other services required to deploy your applications, such as Amazon EC2 and Amazon S3, only to operations specific to CodeDeploy\.
+ **AWSCodeDeployDeployerAccess** – Grants access to an IAM user to register and deploy revisions\.

   
+ **AWSCodeDeployReadOnlyAccess** – Grants read\-only access to CodeDeploy\.

   
+ **AWSCodeDeployRole** – Allows CodeDeploy to identify Amazon EC2 instances by their Amazon EC2 tags or Amazon EC2 Auto Scaling group names, and on\-premises instances by their on\-premises instance tags, and to deploy application revisions to them accordingly\. Provides permissions needed to publish notification to an Amazon SNS topic and retrieve information about alarms from CloudWatch\.

   
+ **AWSCodeDeployRoleForLambda** – Grants CodeDeploy permission to access AWS Lambda and any other resource required for a deployment\.

   
+  **AWSCodeDeployRoleForECS** – Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment\. 

   
+  **AWSCodeDeployRoleForECSLimited** – Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment with the following exceptions: 
  +  In the `hooks` section of the AppSpec file, only Lambda functions with names that begin with `CodeDeployHook_` can be used\. For more information, see [AppSpec 'hooks' Section for an Amazon ECS Deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\. 
  +  Amazon S3 bucket access is limited to S3 buckets with a registration tag, `UseWithCodeDeploy`, that has a value of `true`\. For more information, see [Object Tagging](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-tagging.html)\. 

Permissions for some aspects of the deployment process are granted to two other role types that act on behalf of CodeDeploy, rather than to IAM users:
+ **IAM instance profile**: An IAM role that you attach to your Amazon EC2 instances\. This profile includes the permissions required to access the Amazon S3 buckets or GitHub repositories where the applications are stored\. For more information, see [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\.
+ **Service role**: An IAM role that grants permissions to an AWS service so it can access AWS resources\. The policies you attach to the service role determine which AWS resources the service can access and the actions it can perform with those resources\. For CodeDeploy, a service role is used for the following:
  + To read either the tags applied to the instances or the Amazon EC2 Auto Scaling group names associated with the instances\. This enables CodeDeploy to identify instances to which it can deploy applications\.
  + To perform operations on instances, Amazon EC2 Auto Scaling groups, and Elastic Load Balancing load balancers\.
  + To publish information to Amazon SNS topics so that notifications can be sent when specified deployment or instance events occur\.
  + To retrieve information about CloudWatch alarms to set up alarm monitoring for deployments\.

  For more information, see [Step 3: Create a Service Role for CodeDeploy](getting-started-create-service-role.md)\.

You can also create your own custom IAM policies to allow permissions for CodeDeploy actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\.

## Customer Managed Policy Examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for various CodeDeploy actions\. These policies work when you are using the CodeDeploy API, AWS SDKs, or the AWS CLI\. When you are using the console, you need to grant additional permissions specific to the console, which is discussed in [Permissions Required to Use the CodeDeploy Console](#console-permissions)\.

You can use the following sample IAM policies listed to limit the CodeDeploy access for your IAM users and roles\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow a User to Perform CodeDeploy Operations in a Single Region](#identity-based-policies-example-1)
+ [Example 2: Allow a User to Register Revisions for a Single Application](#identity-based-policies-example-2)
+ [Example 3: Allow a User to Create Deployments for a Single Deployment Group](#identity-based-policies-example-3)

### Example 1: Allow a User to Perform CodeDeploy Operations in a Single Region<a name="identity-based-policies-example-1"></a>

The following example grants permissions to perform CodeDeploy operations in the **us\-west\-2** region only:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:*"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:*"
      ]
    }
  ]
}
```

### Example 2: Allow a User to Register Revisions for a Single Application<a name="identity-based-policies-example-2"></a>

The following example grants permissions to register application revisions for all applications that begin with **Test** in the **us\-west\-2** region:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:RegisterApplicationRevision"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:application:Test*"
      ]
    }
  ]
}
```

### Example 3: Allow a User to Create Deployments for a Single Deployment Group<a name="identity-based-policies-example-3"></a>

The following example allows the specified user to create deployments for the deployment group named **WordPress\_DepGroup** associated with the application named **WordPress\_App**, the custom deployment configuration named **ThreeQuartersHealthy**, and any application revisions associated with the application named **WordPress\_App**\. All of these resources are associated with the **us\-west\-2** region\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:CreateDeployment"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:deploymentgroup:WordPress_App/WordPress_DepGroup"
      ]
    },
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:GetDeploymentConfig"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:deploymentconfig:ThreeQuartersHealthy"
      ]
    },
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:GetApplicationRevision"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:application:WordPress_App"
      ]
    }
  ]
}
```
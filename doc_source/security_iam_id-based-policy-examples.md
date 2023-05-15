# AWS CodeDeploy identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, users don't have permission to create or modify CodeDeploy resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. You must create IAM policies that grant IAM roles permission to perform API operations on the specified resources they need\. You must then attach those IAM roles to users or groups who require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

In CodeDeploy, identity\-based policies are used to manage permissions to the various resources related to the deployment process\. You can control access to the following resource types:
+ Applications and application revisions\.
+ Deployments\.
+ Deployment configurations\.
+ Instances and on\-premises instances\.

The capabilities controlled by resource policies vary depending on the resource type, as outlined in the following table:


****  

|  Resource types  |  Capabilities  | 
| --- | --- | 
|  All  |  View and list details about resources  | 
|  Applications Deployment configurations Deployment groups  |  Create resources Delete resources  | 
|  Deployments  |  Create deployments Stop deployments  | 
|  Application revisions  |  Register application revisions  | 
|  Applications Deployment groups  |  Update resources  | 
|  On\-premises instances  |  Add tags to instances Remove tags from instances Register instances Deregister instances  | 

The following example shows a permissions policy that allows a user to delete the deployment group named **WordPress\_DepGroup** associated with the application named **WordPress\_App** in the **us\-west\-2** Region\.

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
        "arn:aws:codedeploy:us-west-2:444455556666:deploymentgroup:WordPress_App/WordPress_DepGroup"
      ]
    }
  ]
}
```

**Topics**
+ [Customer\-managed policy examples](#customer-managed-policies)
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the CodeDeploy console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)

## Customer\-managed policy examples<a name="customer-managed-policies"></a>

In this section, you can find example policies that grant permissions for various CodeDeploy actions\. These policies work when you are using the CodeDeploy API, AWS SDKs, or the AWS CLI\. You must grant additional permissions for actions you perform in the console\. To learn more about granting console permissions, see [Using the CodeDeploy console](#security_iam_id-based-policy-examples-console) \.



**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow permission to perform CodeDeploy operations in a single Region](#identity-based-policies-example-1)
+ [Example 2: Allow permission to register revisions for a single application](#identity-based-policies-example-2)
+ [Example 3: Allow permission to create deployments for a single deployment group](#identity-based-policies-example-3)

### Example 1: Allow permission to perform CodeDeploy operations in a single Region<a name="identity-based-policies-example-1"></a>

The following example grants permissions to perform CodeDeploy operations in the **us\-west\-2** Region only:

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
        "arn:aws:codedeploy:us-west-2:444455556666:*"
      ]
    }
  ]
}
```

### Example 2: Allow permission to register revisions for a single application<a name="identity-based-policies-example-2"></a>

The following example grants permissions to register application revisions for all applications that begin with **Test** in the **us\-west\-2** Region:

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
        "arn:aws:codedeploy:us-west-2:444455556666:application:Test*"
      ]
    }
  ]
}
```

### Example 3: Allow permission to create deployments for a single deployment group<a name="identity-based-policies-example-3"></a>

The following example allows permission to create deployments for the deployment group named **WordPress\_DepGroup** associated with the application named **WordPress\_App**, the custom deployment configuration named **ThreeQuartersHealthy**, and any application revisions associated with the application named **WordPress\_App**\. All of these resources are in the **us\-west\-2** Region\.

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
        "arn:aws:codedeploy:us-west-2:444455556666:deploymentgroup:WordPress_App/WordPress_DepGroup"
      ]
    },
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:GetDeploymentConfig"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:444455556666:deploymentconfig:ThreeQuartersHealthy"
      ]
    },
    {
      "Effect" : "Allow",
      "Action" : [
        "codedeploy:GetApplicationRevision"
      ],
      "Resource" : [
        "arn:aws:codedeploy:us-west-2:444455556666:application:WordPress_App"
      ]
    }
  ]
}
```

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete CodeDeploy resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the CodeDeploy console<a name="security_iam_id-based-policy-examples-console"></a>

If you use the CodeDeploy console, you must have a minimum set of permissions that allows you to describe other AWS resources for your AWS account\. To use CodeDeploy in the CodeDeploy console, you must have permissions from the following services:
+ Amazon EC2 Auto Scaling
+ AWS CodeDeploy
+ Amazon Elastic Compute Cloud
+ Elastic Load Balancing
+ AWS Identity and Access Management
+ Amazon Simple Storage Service
+ Amazon Simple Notification Service
+ Amazon CloudWatch

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users who have a role with that IAM policy\. To ensure that those users can still use the CodeDeploy console, also attach the `AWSCodeDeployReadOnlyAccess` managed policy to the role assigned to the user, as described in [AWS managed \(predefined\) policies for CodeDeploy](managed-policies.md)\.

You don't need to allow minimum console permissions for users who are making calls only to the AWS CLI or the CodeDeploy API\.

## Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```
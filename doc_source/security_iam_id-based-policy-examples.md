# AWS CodeDeploy identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify CodeDeploy resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups who require those permissions\.

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
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:deploymentgroup:WordPress_App/WordPress_DepGroup"
      ]
    }
  ]
}
```

**Topics**
+ [AWS managed \(predefined\) policies for CodeDeploy](#managed-policies)
+ [Customer\-managed policy examples](#customer-managed-policies)
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the CodeDeploy console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)

## AWS managed \(predefined\) policies for CodeDeploy<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS\-managed policies grant permissions for common use cases so you can avoid having to investigate which permissions are required\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to CodeDeploy:
+ `AWSCodeDeployFullAccess`: Grants full access to CodeDeploy\.
**Note**  
AWSCodeDeployFullAccess does not provide permissions to operations in other services required to deploy your applications, such as Amazon EC2 and Amazon S3, only to operations specific to CodeDeploy\.
+ `AWSCodeDeployDeployerAccess`: Grants access to an IAM user to register and deploy revisions\.

   
+ `AWSCodeDeployReadOnlyAccess`: Grants read\-only access to CodeDeploy\.

   
+ `AWSCodeDeployRole`: Allows CodeDeploy to identify EC2 instances by their Amazon EC2 tags or Amazon EC2 Auto Scaling group names, and on\-premises instances by their on\-premises instance tags, and to deploy application revisions to them accordingly\. Provides permissions required to publish notifications to an Amazon SNS topic and retrieve information about alarms from CloudWatch\.

   
+ `AWSCodeDeployRoleForLambda`: Grants CodeDeploy permission to access AWS Lambda and any other resource required for a deployment\.

   
+  `AWSCodeDeployRoleForECS`: Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment\. 

   
+  `AWSCodeDeployRoleForECSLimited`: Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment with the following exceptions: 
  +  In the `hooks` section of the AppSpec file, only Lambda functions with names that begin with `CodeDeployHook_` can be used\. For more information, see [AppSpec 'hooks' section for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\. 
  +  S3 bucket access is limited to S3 buckets with a registration tag, `UseWithCodeDeploy`, that has a value of `true`\. For more information, see [Object tagging](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-tagging.html)\. 

Permissions for some aspects of the deployment process are granted to two other role types that act on behalf of CodeDeploy, rather than to IAM users:
+ **IAM instance profile**: An IAM role that you attach to your Amazon EC2 instances\. This profile includes the permissions required to access the Amazon S3 buckets or GitHub repositories where the applications are stored\. For more information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md)\.
+ **Service role**: An IAM role that grants permissions to an AWS service so it can access AWS resources\. The policies you attach to the service role determine which AWS resources the service can access and the actions it can perform with those resources\. For CodeDeploy, a service role is used for the following:
  + To read either the tags applied to the instances or the Amazon EC2 Auto Scaling group names associated with the instances\. This enables CodeDeploy to identify instances to which it can deploy applications\.
  + To perform operations on instances, Amazon EC2 Auto Scaling groups, and Elastic Load Balancing load balancers\.
  + To publish information to Amazon SNS topics so that notifications can be sent when specified deployment or instance events occur\.
  + To retrieve information about CloudWatch alarms to set up alarm monitoring for deployments\.

  For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

You can also create custom IAM policies to grant permissions for CodeDeploy actions and resources\. You can attach these custom policies to the IAM users or groups who require those permissions\.

### CodeDeploy managed policies and notifications<a name="notifications-permissions"></a>

CodeDeploy supports notifications to make users aware of important changes to deployments\.  Managed policies for CodeDeploy include policy statements for notification functionality\. For more information, see [What are notifications?](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/welcome.html)\.

#### Permissions for notifications in full access managed policies<a name="notifications-fullaccess"></a>

The `AWSCodeDeployFullAccess` managed policy includes the following statements to allow full access to notifications\. Users with this managed policy applied can also create and manage Amazon SNS topics for notifications, subscribe and unsubscribe users to topics, and list topics to choose as targets for notification rules\.

```
    {
        "Sid": "CodeStarNotificationsReadWriteAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:CreateNotificationRule",
            "codestar-notifications:DescribeNotificationRule",
            "codestar-notifications:UpdateNotificationRule",
            "codestar-notifications:DeleteNotificationRule",
            "codestar-notifications:Subscribe",
            "codestar-notifications:Unsubscribe"
        ],
        "Resource": "*",
        "Condition" : {
            "StringLike" : {"codestar-notifications:NotificationsForResource" : "arn:aws:codedeploy:*"} 
        }
    },    
    {
        "Sid": "CodeStarNotificationsListAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:ListNotificationRules",
            "codestar-notifications:ListTargets",
            "codestar-notifications:ListTagsforResource"
        ],
        "Resource": "*"
    },
    {
        "Sid": "CodeStarNotificationsSNSTopicCreateAccess",
        "Effect": "Allow",
        "Action": [
            "sns:CreateTopic",
            "sns:SetTopicAttributes"
        ],
        "Resource": "arn:aws:sns:*:*:codestar-notifications*"
    },
    {
        "Sid": "SNSTopicListAccess",
        "Effect": "Allow",
        "Action": [
            "sns:ListTopics"
        ],
        "Resource": "*"
    }
```

#### Permissions for notifications in read\-only managed policies<a name="notifications-readonly"></a>

The `AWSCodeDeployReadOnlyAccess` managed policy includes the following statements to allow read\-only access to notifications\. Users with this managed policy applied can view notifications for resources, but cannot create, manage, or subscribe to them\. 

```
   {
        "Sid": "CodeStarNotificationsPowerUserAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:DescribeNotificationRule"
        ],
        "Resource": "*",
        "Condition" : {
            "StringLike" : {"codestar-notifications:NotificationsForResource" : "arn:aws:codedeploy:*"} 
        }
    },    
    {
        "Sid": "CodeStarNotificationsListAccess",
        "Effect": "Allow",
        "Action": [
            "codestar-notifications:ListNotificationRules"
        ],
        "Resource": "*"
    }
```

For more information, see [Identity and access management for AWS CodeStar Notifications](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/security-iam.html)\.

## Customer\-managed policy examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for various CodeDeploy actions\. These policies work when you are using the CodeDeploy API, AWS SDKs, or the AWS CLI\. You must grant additional permissions for actions you perform in the console\. To learn more about granting console permissions, see [Using the CodeDeploy console](#security_iam_id-based-policy-examples-console) \.

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow a user to perform CodeDeploy operations in a single Region](#identity-based-policies-example-1)
+ [Example 2: Allow a user to register revisions for a single application](#identity-based-policies-example-2)
+ [Example 3: Allow a user to create deployments for a single deployment group](#identity-based-policies-example-3)

### Example 1: Allow a user to perform CodeDeploy operations in a single Region<a name="identity-based-policies-example-1"></a>

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
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:*"
      ]
    }
  ]
}
```

### Example 2: Allow a user to register revisions for a single application<a name="identity-based-policies-example-2"></a>

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
        "arn:aws:codedeploy:us-west-2:80398EXAMPLE:application:Test*"
      ]
    }
  ]
}
```

### Example 3: Allow a user to create deployments for a single deployment group<a name="identity-based-policies-example-3"></a>

The following example allows the specified user to create deployments for the deployment group named **WordPress\_DepGroup** associated with the application named **WordPress\_App**, the custom deployment configuration named **ThreeQuartersHealthy**, and any application revisions associated with the application named **WordPress\_App**\. All of these resources are in the **us\-west\-2** Region\.

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

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete CodeDeploy resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get Started Using AWS Managed Policies** – To start using CodeDeploy quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get Started Using Permissions With AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant Least Privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for Sensitive Operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use Policy Conditions for Extra Security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

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

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the CodeDeploy console, also attach the `AWSCodeDeployReadOnlyAccess` managed policy to the user, as described in [AWS managed \(predefined\) policies for CodeDeploy](#managed-policies)\.

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
# Step 3: Limit the CodeDeploy user's permissions<a name="getting-started-policy"></a>

We recommend that you limit the permissions of the administrative user and programmatic user that you created in [Step 1: Setting up](getting-started-setting-up.md) to just those shown in the following IAM permissions policy\. The permissions allow users to create and manage deployments in CodeDeploy, and to work with AWS services and resources that CodeDeploy relies on\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CodeDeployAccessPolicy",
      "Effect": "Allow",
      "Action": [
        "autoscaling:*",
        "codedeploy:*",
        "ec2:*",
        "lambda:*",
        "ecs:*",
        "elasticloadbalancing:*",
        "iam:AddRoleToInstanceProfile",
        "iam:AttachRolePolicy",
        "iam:CreateInstanceProfile",
        "iam:CreateRole",
        "iam:DeleteInstanceProfile",
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:GetInstanceProfile",
        "iam:GetRole",
        "iam:GetRolePolicy",
        "iam:ListInstanceProfilesForRole",
        "iam:ListRolePolicies",
        "iam:ListRoles",
        "iam:PutRolePolicy",
        "iam:RemoveRoleFromInstanceProfile",
        "s3:*",
        "ssm:*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeDeployRolePolicy",
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::account-ID:role/CodeDeployServiceRole"
    }
  ]
}
```

In this policy, replace *arn:aws:iam::account\-ID:role/CodeDeployServiceRole* with the ARN value of the CodeDeploy service role that you created in [Step 2: Create a service role for CodeDeploy](getting-started-create-service-role.md)\. You can find the ARN value in the details page of the service role in the IAM console\.

The preceding policy lets you deploy an application to an AWS Lambda compute platform, an EC2/On\-Premises compute platform, and an Amazon ECS compute platform\.

You can use the AWS CloudFormation templates provided in this documentation to launch Amazon EC2 instances that are compatible with CodeDeploy\. To use AWS CloudFormation templates to create applications, deployment groups, or deployment configurations, you must provide access to AWS CloudFormation—and AWS services and actions that AWS CloudFormation depends on—by adding the `cloudformation:*` permission to the CodeDeploy user's permission policy, like this:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        ...
        "cloudformation:*"        
      ],
      "Resource": "*"
    }
  ]
}
```
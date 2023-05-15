# AWS managed \(predefined\) policies for CodeDeploy<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS\-managed policies grant permissions for common use cases so you can avoid having to investigate which permissions are required\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

**Topics**
+ [List of AWS managed policies for CodeDeploy](#managed-policies-list)
+ [CodeDeploy managed policies and notifications](#notifications-permissions)

## List of AWS managed policies for CodeDeploy<a name="managed-policies-list"></a>

The following AWS managed policies, which you can attach to users in your account, are specific to CodeDeploy:
+ `AWSCodeDeployFullAccess`: Grants full access to CodeDeploy\.

   
**Note**  
AWSCodeDeployFullAccess does not provide permissions to operations in other services required to deploy your applications, such as Amazon EC2 and Amazon S3, only to operations specific to CodeDeploy\.
+ `AWSCodeDeployDeployerAccess`: Grants permission to register and deploy revisions\.

   
+ `AWSCodeDeployReadOnlyAccess`: Grants read\-only access to CodeDeploy\.

   
+ <a name="ACD-policy"></a>`AWSCodeDeployRole`: Allows CodeDeploy to:
  + read the tags on your instances or identify your Amazon EC2 instances by Amazon EC2 Auto Scaling group names
  + read, create, update, and delete Amazon EC2 Auto Scaling groups, lifecycle hooks, scaling policies, and warm pool features
  + publish information to Amazon SNS topics
  + retrieve information about Amazon CloudWatch alarms
  + read and update resources in the Elastic Load Balancing service

  The policy contains the following code:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "autoscaling:CompleteLifecycleAction",
          "autoscaling:DeleteLifecycleHook",
          "autoscaling:DescribeAutoScalingGroups",
          "autoscaling:DescribeLifecycleHooks",
          "autoscaling:PutLifecycleHook",
          "autoscaling:RecordLifecycleActionHeartbeat",
          "autoscaling:CreateAutoScalingGroup",
          "autoscaling:CreateOrUpdateTags",
          "autoscaling:UpdateAutoScalingGroup",
          "autoscaling:EnableMetricsCollection",
          "autoscaling:DescribePolicies",
          "autoscaling:DescribeScheduledActions",
          "autoscaling:DescribeNotificationConfigurations",
          "autoscaling:SuspendProcesses",
          "autoscaling:ResumeProcesses",
          "autoscaling:AttachLoadBalancers",
          "autoscaling:AttachLoadBalancerTargetGroups",
          "autoscaling:PutScalingPolicy",
          "autoscaling:PutScheduledUpdateGroupAction",
          "autoscaling:PutNotificationConfiguration",
          "autoscaling:DescribeScalingActivities",
          "autoscaling:DeleteAutoScalingGroup",
          "autoscaling:PutWarmPool",
          "ec2:DescribeInstances",
          "ec2:DescribeInstanceStatus",
          "ec2:TerminateInstances",
          "tag:GetResources",
          "sns:Publish",
          "cloudwatch:DescribeAlarms",
          "cloudwatch:PutMetricAlarm",
          "elasticloadbalancing:DescribeLoadBalancers",
          "elasticloadbalancing:DescribeInstanceHealth",
          "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
          "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
          "elasticloadbalancing:DescribeTargetGroups",
          "elasticloadbalancing:DescribeTargetHealth",
          "elasticloadbalancing:RegisterTargets",
          "elasticloadbalancing:DeregisterTargets"
        ],
        "Resource": "*"
      }
    ]
  }
  ```

   
+ `AWSCodeDeployRoleForLambda`: Grants CodeDeploy permission to access AWS Lambda and any other resource required for a deployment\.

   
+  `AWSCodeDeployRoleForECS`: Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment\. 

   
+  `AWSCodeDeployRoleForECSLimited`: Grants CodeDeploy permission to access Amazon ECS and any other resource required for a deployment with the following exceptions: 
  +  In the `hooks` section of the AppSpec file, only Lambda functions with names that begin with `CodeDeployHook_` can be used\. For more information, see [AppSpec 'hooks' section for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\. 
  +  S3 bucket access is limited to S3 buckets with a registration tag, `UseWithCodeDeploy`, that has a value of `true`\. For more information, see [Object tagging](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-tagging.html)\. 
+ <a name="EC2-policy"></a>`AmazonEC2RoleforAWSCodeDeployLimited`: Grants CodeDeploy permission to get and list objects in a CodeDeploy Amazon S3 bucket\. The policy contains the following code:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject",
                  "s3:GetObjectVersion",
                  "s3:ListBucket"
              ],
              "Resource": "arn:aws:s3:::*/CodeDeploy/*"
          },
          {
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject",
                  "s3:GetObjectVersion"
              ],
              "Resource": "*",
              "Condition": {
                  "StringEquals": {
                      "s3:ExistingObjectTag/UseWithCodeDeploy": "true"
                  }
              }
          }
      ]
  }
  ```

Permissions for some aspects of the deployment process are granted to two other role types that act on behalf of CodeDeploy:
+ An *IAM instance profile* is an IAM role that you attach to your Amazon EC2 instances\. This profile includes the permissions required to access the Amazon S3 buckets or GitHub repositories where the applications are stored\. For more information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md)\.
+ A *service role* is an IAM role that grants permissions to an AWS service so it can access AWS resources\. The policies you attach to the service role determine which AWS resources the service can access and the actions it can perform with those resources\. For CodeDeploy, a service role is used for the following:
  + To read either the tags applied to the instances or the Amazon EC2 Auto Scaling group names associated with the instances\. This enables CodeDeploy to identify instances to which it can deploy applications\.
  + To perform operations on instances, Amazon EC2 Auto Scaling groups, and Elastic Load Balancing load balancers\.
  + To publish information to Amazon SNS topics so that notifications can be sent when specified deployment or instance events occur\.
  + To retrieve information about CloudWatch alarms to set up alarm monitoring for deployments\.

  For more information, see [Step 2: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

You can also create custom IAM policies to grant permissions for CodeDeploy actions and resources\. You attach these custom policies to IAM roles, and then you assign the roles to users or groups who require the permissions\.

## CodeDeploy managed policies and notifications<a name="notifications-permissions"></a>

CodeDeploy supports notifications to make users aware of important changes to deployments\.  Managed policies for CodeDeploy include policy statements for notification functionality\. For more information, see [What are notifications?](https://docs.aws.amazon.com/codestar-notifications/latest/userguide/welcome.html)\.

### Permissions for notifications in full access managed policies<a name="notifications-fullaccess"></a>

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

### Permissions for notifications in read\-only managed policies<a name="notifications-readonly"></a>

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
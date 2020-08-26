# Step 3: Create a service role for CodeDeploy<a name="getting-started-create-service-role"></a>

In AWS, service roles are used to grant permissions to an AWS service so it can access AWS resources\. The policies that you attach to the service role determine which AWS resources the service can access and what it can do with those resources\. 

The service role you create for CodeDeploy must be granted the permissions required for your compute platform\. If you deploy to more than one compute platform, create one service role for each\. To add permissions, attach one or more of the following AWS\-supplied policies:

For EC2/On\-Premises deployments, attach the **AWSCodeDeployRole** policy\. It provides the permissions for your service role to:
+ Read the tags on your instances or identify your Amazon EC2 instances by Amazon EC2 Auto Scaling group names\.
+ Read, create, update, and delete Amazon EC2 Auto Scaling groups, lifecycle hooks, and scaling policies\.
+ Publish information to Amazon SNS topics\.
+ Retrieve information about CloudWatch alarms\.
+ Read and update Elastic Load Balancing\.
**Note**  
 If you create your Auto Scaling group with a launch template, you must add the following permissions:   
 `ec2:RunInstances` 
 `ec2:CreateTags` 
 `iam:PassRole` 
 For more information, see [Step 3: Create a service role](#getting-started-create-service-role) and [Creating a Launch Template for an Auto Scaling Group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html)\. 

For Amazon ECS deployments, if you want full access to support services, attach the **AWSCodeDeployRoleForECS** policy\. It provides the permissions for your service role to:
+  Read, update, and delete Amazon ECS task sets\. 
+  Update Elastic Load Balancing target groups, listeners, and rules\. 
+  Invoke AWS Lambda functions\. 
+  Access revision files in Amazon S3 buckets\. 
+  Retrieve information about CloudWatch alarms\. 
+ Publish information to Amazon SNS topics\.

For Amazon ECS deployments, if you want limited access to support services, attach the **AWSCodeDeployRoleForECSLimited** policy\. It provides the permissions for your service role to:
+  Read, update, and delete Amazon ECS task sets\. 
+  Retrieve information about CloudWatch alarms\. 
+ Publish information to Amazon SNS topics\.

For AWS Lambda deployments, attach the **AWSCodeDeployRoleForLambda** policy\. It provides the permissions for your service role to:
+  Read, update, and invoke AWS Lambda functions and aliases\. 
+  Access revision files in Amazon S3 buckets\. 
+  Publish information to Amazon SNS topics\. 
+  Retrieve information about CloudWatch alarms\. 

As part of setting up the service role, you also update its trust relationship to specify the endpoints to which you want to grant it access\.

You can create a service role with the IAM console, the AWS CLI, or the IAM APIs\.

**Topics**
+ [Create a service role \(console\)](#getting-started-create-service-role-console)
+ [Create a service role \(CLI\)](#getting-started-create-service-role-cli)
+ [Get the service role ARN \(console\)](#getting-started-get-service-role-console)
+ [Get the service role ARN \(CLI\)](#getting-started-get-service-role-cli)

## Create a service role \(console\)<a name="getting-started-create-service-role-console"></a>

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and from the **Choose the service that will use this role** list, choose **CodeDeploy**\.

1. From **Select your use case**, choose your use case:
   +  For EC2/On\-Premises deployments, choose **CodeDeploy**\. 
   +  For Amazon ECS deployments, choose **CodeDeploy \- ECS**\. 
   +  For AWS Lambda deployments, choose **CodeDeploy for Lambda**\. 

1. Choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, the permission policy is displayed\. Choose **Next: Tags**\.

1. On the **Review** page, in **Role name**, enter a name for the service role \(for example, **CodeDeployServiceRole**\), and then choose **Create role**\.

   You can also enter a description for this service role in **Role description**\.

1. If you want this service role to have permission to access all currently supported endpoints, you are finished with this procedure\.

   To restrict this service role from access to some endpoints, in the list of roles, browse to and choose the role you created, and continue to the next step\.

1. On the **Trust relationships** tab, choose **Edit trust relationship**\.

1. You should see the following policy, which provides the service role permission to access all supported endpoints:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "",
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "codedeploy.amazonaws.com"
                   ]
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

   To grant the service role access to only some supported endpoints, replace the contents of the **Policy Document** box with the following policy\. Remove the lines for the endpoints you want to prevent access to, and then choose **Update Trust Policy**\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "",
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "codedeploy.us-east-2.amazonaws.com",
                       "codedeploy.us-east-1.amazonaws.com",
                       "codedeploy.us-west-1.amazonaws.com",
                       "codedeploy.us-west-2.amazonaws.com",
                       "codedeploy.eu-west-3.amazonaws.com",
                       "codedeploy.ca-central-1.amazonaws.com",
                       "codedeploy.eu-west-1.amazonaws.com",
                       "codedeploy.eu-west-2.amazonaws.com",
                       "codedeploy.eu-central-1.amazonaws.com",
                       "codedeploy.ap-east-1.amazonaws.com",
                       "codedeploy.ap-northeast-1.amazonaws.com",
                       "codedeploy.ap-northeast-2.amazonaws.com",
                       "codedeploy.ap-southeast-1.amazonaws.com",
                       "codedeploy.ap-southeast-2.amazonaws.com",
                       "codedeploy.ap-south-1.amazonaws.com",
                       "codedeploy.sa-east-1.amazonaws.com"
                   ]
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

For more information about creating service roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-creatingrole-service.html) in the *IAM User Guide*\.

## Create a service role \(CLI\)<a name="getting-started-create-service-role-cli"></a>

1. On your development machine, create a text file named, for example, `CodeDeployDemo-Trust.json`\. This file is used to allow CodeDeploy to work on your behalf\.

   Do one of the following: 
   + To grant access to all supported AWS Regions, save the following content in the file:

     ```
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Sid": "",
                 "Effect": "Allow",
                 "Principal": {
                     "Service": [
                         "codedeploy.amazonaws.com"
                     ]
                 },
                 "Action": "sts:AssumeRole"
             }
         ]
     }
     ```
   + To grant access to only some supported regions, type the following content into the file, and remove the lines for the regions to which you want to exclude access:

     ```
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Sid": "",
                 "Effect": "Allow",
                 "Principal": {
                     "Service": [
                         "codedeploy.us-east-2.amazonaws.com",
                         "codedeploy.us-east-1.amazonaws.com",
                         "codedeploy.us-west-1.amazonaws.com",
                         "codedeploy.us-west-2.amazonaws.com",
                         "codedeploy.eu-west-3.amazonaws.com",
                         "codedeploy.ca-central-1.amazonaws.com",
                         "codedeploy.eu-west-1.amazonaws.com",
                         "codedeploy.eu-west-2.amazonaws.com",
                         "codedeploy.eu-central-1.amazonaws.com",
                         "codedeploy.ap-east-1.amazonaws.com",
                         "codedeploy.ap-northeast-1.amazonaws.com",
                         "codedeploy.ap-northeast-2.amazonaws.com",
                         "codedeploy.ap-southeast-1.amazonaws.com",
                         "codedeploy.ap-southeast-2.amazonaws.com",
                         "codedeploy.ap-south-1.amazonaws.com",
                         "codedeploy.sa-east-1.amazonaws.com"
                     ]
                 },
                 "Action": "sts:AssumeRole"
             }
         ]
     }
     ```
**Note**  
Do not use a comma after the last endpoint in the list\.

1. From the same directory, call the create\-role command to create a service role named **CodeDeployServiceRole** based on the information in the text file you just created:

   ```
   aws iam create-role --role-name CodeDeployServiceRole --assume-role-policy-document file://CodeDeployDemo-Trust.json
   ```
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   In the command's output, make a note of the value of the `Arn` entry under the `Role` object\. You need it later when you create deployment groups\. If you forget the value, follow the instructions in [Get the service role ARN \(CLI\) ](#getting-started-get-service-role-cli)\. 

1. The managed policy you use depends on the compute platform\.
   + If your deployment is to an EC2/On\-Premises compute platform:

     Call the attach\-role\-policy command to give the service role named **CodeDeployServiceRole** the permissions based on the IAM managed policy named **AWSCodeDeployRole**\. For example:

     ```
     aws iam attach-role-policy --role-name CodeDeployServiceRole --policy-arn arn:aws:iam::aws:policy/service-role/AWSCodeDeployRole
     ```
   + If your deployment is to an AWS Lambda compute platform:

     Call the attach\-role\-policy command to give the service role named **CodeDeployServiceRole** the permissions based on the IAM managed policy named **AWSCodeDeployRoleForLambda**\. For example:

     ```
     aws iam attach-role-policy --role-name CodeDeployServiceRole --policy-arn arn:aws:iam::aws:policy/service-role/AWSCodeDeployRoleForLambda
     ```
   + If your deployment is to an Amazon ECS compute platform:

     Call the attach\-role\-policy command to give the service role named **CodeDeployServiceRole** the permissions based on the IAM managed policy named **AWSCodeDeployRoleForECS** or **AWSCodeDeployRoleForECSLimited**\. For example:

     ```
     aws iam attach-role-policy --role-name CodeDeployServiceRole --policy-arn arn:aws:iam::aws:policy/AWSCodeDeployRoleForECS
     ```

For more information about creating service roles, see [Creating a role for an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/create-role-xacct.html) in the *IAM User Guide*\.

## Get the service role ARN \(console\)<a name="getting-started-get-service-role-console"></a>

To use the IAM console to get the ARN of the service role:

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the **Filter** box, type **CodeDeployServiceRole**, and then press Enter\.

1. Choose **CodeDeployServiceRole**\.

1. Make a note of the value of the **Role ARN** field\.

## Get the service role ARN \(CLI\)<a name="getting-started-get-service-role-cli"></a>

To use the AWS CLI to get the ARN of the service role, call the get\-role command against the service role named **CodeDeployServiceRole**:

```
aws iam get-role --role-name CodeDeployServiceRole --query "Role.Arn" --output text
```

The value returned is the ARN of the service role\.
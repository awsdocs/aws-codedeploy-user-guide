# Deploy an application in a different AWS account<a name="deployments-cross-account"></a>

Organizations commonly have multiple AWS accounts that they use for different purposes \(for example, one for system administration tasks and another for development, test, and production tasks or one associated with development and test environments and another associated with the production environment\)\.

Although you might perform related work in different accounts, CodeDeploy deployment groups and the Amazon EC2 instances to which they deploy are strictly tied to the accounts under which they were created\. You cannot, for example, add an instance that you launched in one account to a deployment group in another\.

Assume you have two AWS accounts: your development account and your production account\. You work primarily in the development account, but you want to be able kick off deployments in your production account without a full set of credentials there or without having to sign out of the development account and in to the production account\. 

After following the cross\-account configuration steps, you can initiate deployments that belong to another of your organization’s accounts without needing a full set of credentials for that other account\. You do this, in part, by using a capability provided by the AWS Security Token Service \(AWS STS\) that grants you temporary access to that account\. 

## Step 1: Create an S3 bucket in either account<a name="deployments-cross-account-1-create-s3-bucket"></a>

In either the development account or the production account:
+ If you have not already done so, create an Amazon S3 bucket where the application revisions for the production account will be stored\. For information, see [Create a Bucket in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\. You can even use the same bucket and application revisions for both accounts, deploying the same files to your production environment that you tested and verified in your development account\.

## Step 2: Grant Amazon S3 bucket permissions to the production account's IAM instance profile<a name="deployments-cross-account-2-grant-bucket-permissions"></a>

If the Amazon S3 bucket you created in step 1 is in your production account, this step is not required\. The role you assume later will already have access to this bucket because it is also in the production account\.

If you created the Amazon S3 bucket in the development account, do the following:
+ In the production account, create an IAM instance profile\. For information, see [Step 4: Create an IAM instance profile for your Amazon EC2 instances](getting-started-create-iam-instance-profile.md)\. 
**Note**  
Make note of the ARN for this IAM instance profile\. You will need to add it to the cross\-bucket policy you create next\.
+ In the development account, give access to the Amazon S3 bucket you created in the development account to the IAM instance profile you just created in your production account\. For information, see [ Example 2: Bucket owner granting cross\-account bucket permissions](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.html)\. 

  Note the following as you complete the process of granting cross\-account bucket permissions:
  + In the sample walkthrough, Account A represents your development account and Account B represents your production account\. 
  + When you [perform the Account A \(development account\) tasks](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.html#access-policies-walkthrough-cross-account-permissions-acctA-tasks), modify the following bucket policy to grant cross\-account permissions instead of using the sample policy provided in the walkthrough\.

    ```
    {
       "Version": "2012-10-17",
       "Statement": [
          {
             "Sid": "Cross-account permissions",
             "Effect": "Allow",
             "Principal": {
                "AWS": "arn:aws:iam::account-id:role/role-name" 
             },
             "Action": [
                "s3:Get*",
                "s3:List*"
             ],
             "Resource": [
                "arn:aws:s3:::bucket-name/*"
             ]
          }
       ]
    }
    ```

    *account\-id* represents the account number of the production account where you just created the IAM instance profile\.

    *role\-name* represents the name of the IAM instance profile you just created\.

    *bucket\-name* represents the name of the bucket you created in step 1\. Be sure to include the `/*` after the name of your bucket to provide access to each of the files inside the bucket\.

## Step 3: Create resources and a cross\-account role in the production account<a name="deployments-cross-account-3-create-resources-and-role"></a>

In your production account:
+ Create your CodeDeploy resources — application, deployment group, deployment configuration, Amazon EC2 instances, Amazon EC2 instance profile, service role, and so on — using the instructions in this guide\.
+ Create an additional role, a cross\-account IAM role, that a user in your development account can assume to perform CodeDeploy operations in this production account\. 

  Use the [Walkthrough: Delegate access across AWS accounts using IAM roles ](https://docs.aws.amazon.com/IAM/latest/UserGuide/walkthru_cross-account-with-roles.html) as a guide to help you create the cross\-account role\. Instead of adding the sample permissions in the walkthrough to your policy document, you should attach, at minimum, the following two AWS\-supplied policies to the role: 
  + `AmazonS3FullAccess`: Required only if the S3 bucket is in the development account\. Provides the assumed production account role with full access to the Amazon S3 services and resources in the development account, where the revision is stored\. 
  + `AWSCodeDeployDeployerAccess`: Enables an IAM user to register and deploy revisions\. 

  If you want to create and manage deployment groups and not just initiate deployments, add the `AWSCodeDeployFullAccess` policy instead of the `AWSCodeDeployDeployerAccess` policy\. For more information about using IAM managed policies to grant permissions for CodeDeploy tasks, see [AWS managed \(predefined\) policies for CodeDeploy](security_iam_id-based-policy-examples.md#managed-policies)\. 

  You can attach additional policies if you want to perform tasks in other AWS services while using this cross\-account role\.

**Important**  
As you create the cross\-account IAM role, make a note of the details you will need to gain access to the production account\.  
To use the AWS Management Console to switch roles, you will need to supply either of the following:  
A URL for accessing the production account with the assumed role's credentials\. You will find the URL on the **Review** page, which is displayed at the end of the cross\-account role creation process\.
The name of the cross\-account role and either the account ID number or alias\. 
To use the AWS CLI to switch roles, you will need to supply the following:  
The ARN of the cross\-account role you will assume\.

## Step 4: Upload the application revision to Amazon S3 bucket<a name="deployments-cross-account-4-upload-application-revision"></a>

In the account in which you created the Amazon S3 bucket:
+ Upload your application revision to the Amazon S3 bucket\. For information, see [Push a revision for CodeDeploy to Amazon S3 \(EC2/On\-Premises deployments only\)](application-revisions-push.md)\. 

## Step 5: Assume the cross\-account role and deploy applications<a name="deployments-cross-account-5-assume-role-and-deploy"></a>

In the development account, you can use the AWS CLI or the AWS Management Console to assume the cross\-account role and initiate the deployment in the production account\. 

For instructions about how to use the AWS Management Console to switch roles and initiate deployments, see [Switching to a role \(AWS Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) and [Create an EC2/On\-Premises Compute Platform deployment \(console\)](deployments-create-console.md)\.

For instructions about how to use the AWS CLI to assume the cross\-account role and initiate deployments, see [Switching to an IAM role \(AWS Command Line Interface\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-cli.html) and [Create an EC2/On\-Premises Compute Platform deployment \(CLI\)](deployments-create-cli.md)\.

For more information about assuming a role through AWS STS, see [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) in the [AWS Security Token Service User Guide](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) and [assume\-role](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html) in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

**Related topic:**
+ [CodeDeploy: Deploying from a development account to a production account](http://aws.amazon.com/blogs/devops/aws-codedeploy-deploying-from-a-development-account-to-a-production-account/)
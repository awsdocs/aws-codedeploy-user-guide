# Deployment Prerequisites<a name="deployments-create-prerequisites"></a>

Make sure the following steps are complete before you start a deployment\.

## Deployment Prerequisites on an AWS Lambda Compute Platform<a name="deployment-prerequisites-lambda"></a>
+ Create an application that includes at least one deployment group\. For information, see [Create an Application with CodeDeploy](applications-create.md) and [Create a Deployment Group with CodeDeploy](deployment-groups-create.md)\.
+ Prepare the application revision, also known as the AppSpec file, that specifies the Lambda function version you want to deploy\. The AppSpec file can also specify Lambda functions to validate your deployment\. For more information see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.
+ If you want to use a custom deployment configuration for your deployment, create it before starting the deployment process\. For information, see [Create a Deployment Configuration with CodeDeploy](deployment-configurations-create.md)\.

## Deployment Prerequisites on an EC2/On\-Premises Compute Platform<a name="deployment-prerequisites-server"></a>
+ For an in\-place deployment, create or configure the instances you want to deploy to\. For information, see [Working with Instances for CodeDeploy](instances.md)\. For a blue/green deployment, you either have an existing Amazon EC2 Auto Scaling group to use as a template for your replacement environment, or you have one or more instances or Amazon EC2 Auto Scaling groups that you specify as your original environment\. For more information, see [Tutorial: Use CodeDeploy to Deploy an Application to an Amazon EC2 Auto Scaling Group](tutorials-auto-scaling-group.md) and [Integrating CodeDeploy with Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md)\. 
+ Create an application that includes at least one deployment group\. For information, see [Create an Application with CodeDeploy](applications-create.md) and [Create a Deployment Group with CodeDeploy](deployment-groups-create.md)\.
+ Prepare the application revision that you want to deploy to the instances in your deployment group\. For information, see [Working with Application Revisions for CodeDeploy](application-revisions.md)\.
+ If you want to use a custom deployment configuration for your deployment, create it before starting the deployment process\. For information, see [Create a Deployment Configuration with CodeDeploy](deployment-configurations-create.md)\.
+ If you are deploying your application revision from an Amazon S3 bucket, the bucket is in the same AWS region as the instances in your deployment group\. 
+ If you are deploying your application revision from an Amazon S3 bucket, an Amazon S3 bucket policy has been applied to the bucket\. This policy grants your instances the permissions required to download the application revision\.

  For example, the following Amazon S3 bucket policy allows any Amazon EC2 instance with an attached IAM instance profile containing the ARN `arn:aws:iam::80398EXAMPLE:role/CodeDeployDemo` to download from anywhere in the Amazon S3 bucket named `codedeploydemobucket`:

  ```
  {
      "Statement": [
          {
              "Action": [
                  "s3:Get*",
                  "s3:List*"
              ],
              "Effect": "Allow",
              "Resource": "arn:aws:s3:::codedeploydemobucket/*",
              "Principal": {
                  "AWS": [
                      "arn:aws:iam::80398EXAMPLE:role/CodeDeployDemo"
                  ]
              }
          }
      ]
  }
  ```

  The following Amazon S3 bucket policy allows any on\-premises instance with an associated IAM user containing the ARN `arn:aws:iam::80398EXAMPLE:user/CodeDeployUser` to download from anywhere in the Amazon S3 bucket named `codedeploydemobucket`:

  ```
  {
      "Statement": [
          {
              "Action": [
                  "s3:Get*",
                  "s3:List*"
              ],
              "Effect": "Allow",
              "Resource": "arn:aws:s3:::codedeploydemobucket/*",
              "Principal": {
                  "AWS": [
                      "arn:aws:iam::80398EXAMPLE:user/CodeDeployUser"
                  ]
              }
          }
      ]
  }
  ```

  To learn how to generate and attach an Amazon S3 bucket policy, see [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)\.
+ If you are creating a blue/green deployment, or you have specified an optional Classic Load Balancer, Application Load Balancer, or Network Load Balancer in the deployment group for an in\-place deployment, you have created a VPC using Amazon VPC that contains at least two subnets\. \(CodeDeploy uses Elastic Load Balancing, which requires all instances in a load balancer group to be in a single VPC\.\)

  If you have not created a VPC yet, see the [Amazon VPC Getting Started Guide](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/ExerciseOverview.html)\.
+ If you are creating a blue/green deployment, you have configured a Classic Load Balancer, Application Load Balancer, or Network Load Balancer in Elastic Load Balancing and used it to register the instances that make up your original environment\. 
**Note**  
The instances in your replacement environment will be registered with the load balancer later\.

  To configure a Classic Load Balancer, complete the steps in [Tutorial: Create a Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html) in *User Guide for Classic Load Balancers*\. As you do, make note of the following:
  + In **Step 2: Define Load Balancer**, in **Create LB Inside**, choose the same VPC you selected when you created your instances\.
  + In **Step 5: Register EC2 Instances with Your Load Balancer**, select the instances in your original environment\.
  + In **Step 7: Create and Verify Your Load Balancer**, make a note of the DNS address of your load balancer\.

    For example, if you named your load balancer `my-load-balancer`, your DNS address would appear in a format such as `my-load-balancer-1234567890.us-east-2.elb.amazonaws.com`\.

    When you paste the DNS name into the address field of an Internet\-connected web browser, you should see the application you have deployed for your original environment\.

  To configure an Application Load Balancer, follow the instructions in one of the following topics:
  + [Create an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html)
  + [Tutorials: Create an Application Load Balancer Using the AWS CLI](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-application-load-balancer-cli.html)

  To configure a Network Load Balancer, follow the instructions in one of the following topics:
  + [Create a Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html)
  + [Tutorials: Create a Network Load Balancer Using the AWS CLI](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancer-cli.html)
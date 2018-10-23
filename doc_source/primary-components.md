# AWS CodeDeploy Primary Components<a name="primary-components"></a>

Before you start working with the service, you should familiarize yourself with the major components of the AWS CodeDeploy deployment process\. 

**Application**: A name that uniquely identifies the application you want to deploy\. AWS CodeDeploy uses this name, which functions as a container, to ensure the correct combination of revision, deployment configuration, and deployment group are referenced during a deployment\.

**Compute platform**: The platform on which AWS CodeDeploy deploys an application\.
+ **EC2/On\-Premises**: Describes instances of physical servers that can be Amazon EC2 cloud instances, on\-premises servers, or both\. Applications created using the EC2/On\-Premises compute platform can be composed of executable files, configuration files, images, and more\.

  Deployments that use the EC2/On\-Premises compute platform manage the way in which traffic is directed to instances by using an in\-place or blue/green deployment type\. For more information, see [Overview of AWS CodeDeploy Deployment Types](welcome.md#welcome-deployment-overview)\.
+ **AWS Lambda**: Used to deploy applications that consist of updated versions of Lambda functions\. AWS Lambda manages the Lambda functions in a serverless compute environment made up of a high\-availability compute structure\. All administration of the compute resources is performed by AWS Lambda\. For more information, see [Serverless Computing and Applications](https://aws.amazon.com/serverless/)\. For more information about AWS Lambda and Lambda functions, see [AWS Lambda](https://aws.amazon.com/lambda/)\.

  Applications created using the AWS Lambda compute platform can manage the way in which traffic is directed to the updated Lambda function versions during a deployment by choosing a canary, linear, or all\-at\-once configuration\. 

**Deployment configuration**: A set of deployment rules and deployment success and failure conditions used by AWS CodeDeploy during a deployment\. If your deployment uses the EC2/On\-Premises compute platform, you can specify the minimum number of healthy instances for the deployment\. If your deployment uses the AWS Lambda compute platform, you can specify how traffic is routed to your updated Lambda function versions\.

For more information about specifying the minimum number of healthy hosts for a deployment that uses the EC2/On\-Premises compute platform, see [Minimum Healthy Instances and Deployments](instances-health.md#minimum-healthy-hosts)\.

These are the deployment configurations that specify how traffic is routed during a deployment that uses the AWS Lambda compute platform:
+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Lambda function to the updated Lambda function version at once\.

**Deployment group**: A set of individual instances\. A deployment group contains individually tagged instances, Amazon EC2 instances in Auto Scaling groups, or both\. For information about Amazon EC2 instance tags, see [Working with Tags Using the Console](https://docs.aws.amazon.com//AWSEC2/latest/UserGuide/Using_Tags.html#Using_Tags_Console)\. For information about on\-premises instances, see [Working with On\-Premises Instances for AWS CodeDeploy](instances-on-premises.md)\. For information about Auto Scaling, see [Integrating AWS CodeDeploy with Auto Scaling](integrations-aws-auto-scaling.md)\.

**Deployment type**: The method used to make the latest application revision available on instances in a deployment group\.
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

**IAM instance profile**: An IAM role that you attach to your Amazon EC2 instances\. This profile includes the permissions required to access the Amazon S3 buckets or GitHub repositories where the applications that will be deployed by AWS CodeDeploy are stored\. For more information, see [Step 4: Create an IAM Instance Profile for Your Amazon EC2 Instances](getting-started-create-iam-instance-profile.md)\.

**Revision**: An AWS Lambda deployment revision is a YAML\-formatted or JSON\-formatted file that specifies information about the Lambda function to deploy\. An EC2/On\-Premises deployment revision is an archive file that contains source content—source code, web pages, executable files, and deployment scripts—along with an application specification file \(AppSpec file\)\. AWS Lambda revisions can be stored in Amazon S3 buckets\. EC2/On\-Premises revisions are stored in Amazon S3 buckets or GitHub repositories\. For Amazon S3, a revision is uniquely identified by its Amazon S3 object key and its ETag, version, or both\. For GitHub, a revision is uniquely identified by its commit ID\.

**Service role**: An IAM role that grants permissions to an AWS service so it can access AWS resources\. The policies you attach to the service role determine which AWS resources the service can access and the actions it can perform with those resources\. For AWS CodeDeploy, a service role is used for the following:
+ To read either the tags applied to the instances or the Amazon EC2 Auto Scaling group names associated with the instances\. This enables AWS CodeDeploy to identify instances to which it can deploy applications\.
+ To perform operations on instances, Auto Scaling groups, and Elastic Load Balancing load balancers\.
+ To publish information to Amazon SNS topics so that notifications can be sent when specified deployment or instance events occur\.
+ To retrieve information about CloudWatch alarms in order to set up alarm monitoring for deployments\.

For more information, see [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\.

**Target revision**: The most recent version of the application revision that you have uploaded to your repository and want to deploy to the instances in a deployment group\. In other words, the application revision currently targeted for deployment is the target revision\. This is also the revision that will be pulled for automatic deployments\.

For information about other major components in the AWS CodeDeploy workflow, see the following topics:
+ [Choose an AWS CodeDeploy Repository Type](application-revisions-repository-type.md)
+  [AWS CodeDeploy Deployments](deployment-steps.md)
+  [AWS CodeDeploy Application Specification Files](application-specification-files.md)
+  [AWS CodeDeploy Instance Health](instances-health.md)
+  [Working with the AWS CodeDeploy Agent](codedeploy-agent.md)
+  [Working with On\-Premises Instances for AWS CodeDeploy](instances-on-premises.md)
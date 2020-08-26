# Create a deployment with CodeDeploy<a name="deployments-create"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to create a deployment that installs application revisions you have already pushed to Amazon S3 or, if your deployment is to an EC2/On\-Premises compute platform, GitHub, on the instances in a deployment group\.

The process for creating a deployment depends on the compute platform used by your deployment\. 

**Topics**
+ [Deployment prerequisites](deployments-create-prerequisites.md)
+ [Create an Amazon ECS Compute Platform deployment \(console\)](deployments-create-console-ecs.md)
+ [Create an AWS Lambda Compute Platform deployment \(console\)](deployments-create-console-lambda.md)
+ [Create an EC2/On\-Premises Compute Platform deployment \(console\)](deployments-create-console.md)
+ [Create an Amazon ECS Compute Platform deployment \(CLI\)](deployments-create-ecs-cli.md)
+ [Create an AWS Lambda Compute Platform deployment \(CLI\)](deployments-create-lambda-cli.md)
+ [Create an EC2/On\-Premises Compute Platform deployment \(CLI\)](deployments-create-cli.md)
+ [Create an Amazon ECS blue/green deployment through AWS CloudFormation](deployments-create-ecs-cfn.md)
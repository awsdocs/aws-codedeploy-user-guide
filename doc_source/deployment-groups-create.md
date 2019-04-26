# Create a Deployment Group with CodeDeploy<a name="deployment-groups-create"></a>

You can use the CodeDeploy console, the AWS CLI, the CodeDeploy APIs, or an AWS CloudFormation template to create deployment groups\. For information about using an AWS CloudFormation template to create a deployment group, see [AWS CloudFormation Templates for CodeDeploy Reference](reference-cloudformation-templates.md)\.

When you use the CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

As part of creating a deployment group, you must specify a service role\. For more information, see [Step 3: Create a Service Role for CodeDeploy](getting-started-create-service-role.md)\.

**Topics**
+ [Create a Deployment Group for an In\-Place Deployment \(Console\)](deployment-groups-create-in-place.md)
+ [Create a Deployment Group for an EC2/On\-Premises Blue/Green Deployment \(Console\)](deployment-groups-create-blue-green.md)
+ [Create a Deployment Group for an Amazon ECS Deployment \(Console\)](deployment-groups-create-ecs.md)
+ [Set Up a Load Balancer in Elastic Load Balancing for CodeDeploy Deployments](deployment-groups-create-load-balancer.md)
+ [Create a Deployment Group \(CLI\)](deployment-groups-create-cli.md)
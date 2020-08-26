# Create a deployment group with CodeDeploy<a name="deployment-groups-create"></a>

You can use the CodeDeploy console, the AWS CLI, the CodeDeploy APIs, or an AWS CloudFormation template to create deployment groups\. For information about using an AWS CloudFormation template to create a deployment group, see [AWS CloudFormation templates for CodeDeploy reference](reference-cloudformation-templates.md)\.

When you use the CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

As part of creating a deployment group, you must specify a service role\. For more information, see [Step 3: Create a service role for CodeDeploy](getting-started-create-service-role.md)\.

**Topics**
+ [Create a deployment group for an in\-place deployment \(console\)](deployment-groups-create-in-place.md)
+ [Create a deployment group for an EC2/On\-Premises blue/green deployment \(console\)](deployment-groups-create-blue-green.md)
+ [Create a deployment group for an Amazon ECS deployment \(console\)](deployment-groups-create-ecs.md)
+ [Set up a load balancer in Elastic Load Balancing for CodeDeploy Amazon EC2 deployments](deployment-groups-create-load-balancer.md)
+ [Set up a load balancer, target groups, and listeners for CodeDeploy Amazon ECS deployments](deployment-groups-create-load-balancer-for-ecs.md)
+ [Create a deployment group \(CLI\)](deployment-groups-create-cli.md)
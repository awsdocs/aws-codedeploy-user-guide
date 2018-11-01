--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Create a Deployment Group with AWS CodeDeploy<a name="deployment-groups-create"></a>

You can use the AWS CodeDeploy console, the AWS CLI, the AWS CodeDeploy APIs, or an AWS CloudFormation template to create deployment groups\. For information about using an AWS CloudFormation template to create a deployment group, see [AWS CloudFormation Templates for AWS CodeDeploy Reference](reference-cloudformation-templates.md)\.

When you use the AWS CodeDeploy console to create an application, you configure its first deployment group at the same time\. When you use the AWS CLI to create an application, you create its first deployment group in a separate step\.

As part of creating a deployment group, you must specify a service role\. For more information, see [Step 3: Create a Service Role for AWS CodeDeploy](getting-started-create-service-role.md)\.

**Topics**
+ [Create a Deployment Group for an In\-Place Deployment \(Console\)](deployment-groups-create-in-place.md)
+ [Create a Deployment Group for a Blue/Green Deployment \(Console\)](deployment-groups-create-blue-green.md)
+ [Set Up a Load Balancer in Elastic Load Balancing for AWS CodeDeploy Deployments](deployment-groups-create-load-balancer.md)
+ [Create a Deployment Group \(CLI\)](deployment-groups-create-cli.md)
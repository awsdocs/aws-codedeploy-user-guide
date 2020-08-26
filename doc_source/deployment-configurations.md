# Working with deployment configurations in CodeDeploy<a name="deployment-configurations"></a>

A deployment configuration is a set of rules and success and failure conditions used by CodeDeploy during a deployment\. These rules and conditions are different, depending on whether you deploy to an EC2/On\-Premises compute platform, AWS Lambda compute platform, or Amazon ECS compute platform\. 

## Deployment configurations on an EC2/on\-premises compute platform<a name="deployment-configuration-server"></a>

When you deploy to an EC2/On\-Premises compute platform, the deployment configuration specifies, through the use of a minimum healthy hosts value, the number or percentage of instances that must remain available at any time during a deployment\.

You can use one of the three predefined deployment configurations provided by AWS or create a custom deployment configuration\. If you don't specify a deployment configuration, CodeDeploy uses the CodeDeployDefault\.OneAtATime deployment configuration\.

For more information about how CodeDeploy monitors and evaluates instance health during a deployment, see [CodeDeploy instance health](instances-health.md)\. To view a list of deployment configurations already registered to your AWS account, see [View deployment configuration details with CodeDeploy](deployment-configurations-view-details.md)\. 

### Predefined deployment configurations for an EC2/on\-premises compute platform<a name="deployment-configurations-predefined"></a>

The following table lists the predefined deployment configurations\.


****  

| Deployment configuration | Description | 
| --- | --- | 
| CodeDeployDefault\.AllAtOnce | **In\-place deployments**:Attempts to deploy an application revision to as many instances as possible at once\. The status of the overall deployment is displayed as Succeeded if the application revision is deployed to one or more of the instances\. The status of the overall deployment is displayed as Failed if the application revision is not deployed to any of the instances\. Using an example of nine instances, CodeDeployDefault\.AllAtOnce attempts to deploy to all nine instances at once\. The overall deployment succeeds if deployment to even a single instance is successful\. It fails only if deployments to all nine instances fail\. **Blue/green deployments**: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html) | 
| CodeDeployDefault\.HalfAtATime |  **In\-place deployments**: Deploys to up to half of the instances at a time \(with fractions rounded down\)\. The overall deployment succeeds if the application revision is deployed to at least half of the instances \(with fractions rounded up\)\. Otherwise, the deployment fails\. In the example of nine instances, it deploys to up to four instances at a time\. The overall deployment succeeds if deployment to five or more instances succeed\. Otherwise, the deployment fails\.  **Blue/green deployments**:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html)  | 
| CodeDeployDefault\.OneAtATime |  **In\-place deployments**: Deploys the application revision to only one instance at a time\. For deployment groups that contain more than one instance: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html) For deployment groups that contain only one instance, the overall deployment is successful only if deployment to the single instance is successful\. **Blue/green deployments**: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html)  | 

## Deployment configurations on an Amazon ECS compute platform<a name="deployment-configuration-ecs"></a>

 When you deploy to an Amazon ECS compute platform, the deployment configuration specifies how traffic is shifted to the updated Amazon ECS task set\. 

There are three ways traffic can be shifted during a deployment:
+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Amazon ECS task set in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Amazon ECS task set to the updated Amazon ECS task set all at once\.

You can also create your own custom canary or linear deployment configuration\.

### Predefined deployment configurations for an Amazon ECS compute platform<a name="deployment-configurations-predefined-ecs"></a>

The following table lists the predefined configurations available for Amazon ECS deployments\.


****  

| Deployment configuration | Description | 
| --- | --- | 
|  CodeDeployDefault\.ECSLinear10PercentEvery1Minutes  |  Shifts 10 percent of traffic every minute until all traffic is shifted\.  | 
|  CodeDeployDefault\.ECSLinear10PercentEvery3Minutes   |  Shifts 10 percent of traffic every three minutes until all traffic is shifted\.  | 
|  CodeDeployDefault\.ECSCanary10Percent5Minutes  |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed five minutes later\.  | 
| CodeDeployDefault\.ECSCanary10Percent15Minutes |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed 15 minutes later\.  | 
|  CodeDeployDefault\.ECSAllAtOnce  |  Shifts all traffic to the updated Amazon ECS container at once\.  | 

## Deployment configurations for AWS CloudFormation blue/green deployments \(Amazon ECS\)<a name="deployment-configuration-cfn-bg"></a>

 When you deploy to an Amazon ECS compute platform through AWS CloudFormation blue/green deployments, the deployment configuration specifies how traffic is shifted to the updated Amazon ECS container\.

There are three ways that traffic can shift during an Amazon ECS blue/green deployment through AWS CloudFormation:
+ **Canary**: Traffic is shifted in two increments\. You can choose a predefined canary option that specifies the percentage of traffic shifted to your updated Amazon ECS task set in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\.
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Amazon ECS task set to the updated Amazon ECS task set all at once\.

For this deployment configuration, you cannot create your own custom canary or linear deployment configuration\. For step\-by\-step instructions on using AWS CloudFormation to manage your Amazon ECS blue/green deployments, see [Automate ECS blue/green deployments through CodeDeploy using AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/blue-green.html) in the *AWS CloudFormation User Guide*\. 

**Note**  
Managing Amazon ECS blue/green deployments with AWS CloudFormation is not available in the MXP, CPT, KIX, or BJS/ZHY regions\.

## Deployment configurations on an AWS Lambda compute platform<a name="deployment-configuration-lambda"></a>

When you deploy to an AWS Lambda compute platform, the deployment configuration specifies the way traffic is shifted to the new Lambda function versions in your application\.

There are three ways traffic can shift during a deployment:
+ **Canary**: Traffic is shifted in two increments\. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment\. 
+ **Linear**: Traffic is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once**: All traffic is shifted from the original Lambda function to the updated Lambda function version all at once\.

You can also create your own custom canary or linear deployment configuration\. 

### Predefined deployment configurations for an AWS Lambda compute platform<a name="deployment-configurations-predefined-lambda"></a>

The following table lists the predefined configurations available for AWS Lambda deployments\.


****  

| Deployment configuration | Description | 
| --- | --- | 
|  CodeDeployDefault\.LambdaCanary10Percent5Minutes  |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed five minutes later\.  | 
|  CodeDeployDefault\.LambdaCanary10Percent10Minutes  |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed 10 minutes later\.  | 
|  CodeDeployDefault\.LambdaCanary10Percent15Minutes  |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed 15 minutes later\.  | 
|  CodeDeployDefault\.LambdaCanary10Percent30Minutes  |  Shifts 10 percent of traffic in the first increment\. The remaining 90 percent is deployed 30 minutes later\.  | 
|  CodeDeployDefault\.LambdaLinear10PercentEvery1Minute  | Shifts 10 percent of traffic every minute until all traffic is shifted\. | 
|  CodeDeployDefault\.LambdaLinear10PercentEvery2Minutes  |  Shifts 10 percent of traffic every two minutes until all traffic is shifted\.  | 
|  CodeDeployDefault\.LambdaLinear10PercentEvery3Minutes  |  Shifts 10 percent of traffic every three minutes until all traffic is shifted\.  | 
| CodeDeployDefault\.LambdaLinear10PercentEvery10Minutes | Shifts 10 percent of traffic every 10 minutes until all traffic is shifted\. | 
|  CodeDeployDefault\.LambdaAllAtOnce  |  Shifts all traffic to the updated Lambda functions at once\.  | 

## <a name="topiclist-deployment-configurations"></a>

**Topics**
+ [Create a Deployment Configuration](deployment-configurations-create.md)
+ [View Deployment Configuration Details](deployment-configurations-view-details.md)
+ [Delete a Deployment Configuration](deployment-configurations-delete.md)
# AppSpec 'resources' Section \(Amazon ECS and AWS Lambda Deployments Only\)<a name="reference-appspec-file-structure-resources"></a>

 The content in the `'resources'` section of the AppSpec file varies, depending on the compute platform of your deployment\. The `'resources'` section for an Amazon ECS deployment contains your Amazon ECS task definition, container and port for routing traffic to your updated Amazon ECS task set, and other optional information\. The `'resources'` section for an AWS Lambda deployment contains the name, alias, current version, and target version of a Lambda function\. 

**Topics**
+ [AppSpec 'resources' Section for AWS Lambda Deployments](#reference-appspec-file-structure-resources-lambda)
+ [AppSpec 'resources' Section for Amazon ECS Deployments](#reference-appspec-file-structure-resources-ecs)

## AppSpec 'resources' Section for AWS Lambda Deployments<a name="reference-appspec-file-structure-resources-lambda"></a>

The `'resources'` section specifies the Lambda function to deploy and has the following structure:

YAML:

```
resources:
  - name-of-function-to-deploy:
      type: "AWS::Lambda::Function"
      properties:
        name: name-of-lambda-function-to-deploy
        alias: alias-of-lambda-function-to-deploy
        currentversion: version-of-the-lambda-function-traffic-currently-points-to
        targetversion: version-of-the-lambda-function-to-shift-traffic-to
```

JSON:

```
"resources": [{
    "name-of-function-to-deploy" {
        "type": "AWS::Lambda::Function",
        "properties": {
          "name": "name-of-lambda-function-to-deploy",
          "alias": "alias-of-lambda-function-to-deploy",
          "currentversion": "version-of-the-lambda-function-traffic-currently-points-to",
          "targetversion": "version-of-the-lambda-function-to-shift-traffic-to"
        }
    }
}]
```

Each property is specified with a string\. 
+ **name** – Required\. This is the name of the Lambda function to deploy\.
+ **alias** – Required\. This is the name of the alias to the Lambda function\.
+ **currentversion** – Required\. This is the version of the Lambda function traffic currently points to\.
+ **targetversion** – Required\. This is the version of the Lambda function traffic is shifted to\.

## AppSpec 'resources' Section for Amazon ECS Deployments<a name="reference-appspec-file-structure-resources-ecs"></a>

 The `'resources'` section specifies the Amazon ECS service to deploy and has the following structure: 

YAML:

```
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "task-definition-ARN"
        LoadBalancerInfo: 
          ContainerName: "ECS-container-name-for-your-ECS-application" 
          ContainerPort: port-used-by-your-ECS-application
# Optional properties
      PlatformVersion: "ecs-service-platform-version"
      NetworkConfiguration:
        AwsvpcConfiguration:
          Subnets: ["ecs-subnet-1","ecs-subnet-n"] 
          SecurityGroups: ["ecs-security-group-1","ecs-security-group-n"] 
          AssignPublicIp: "ENABLED-or-DISABLED"
```

JSON:

```
"Resources": [
		{
			"TargetService": {
				"Type": "AWS::ECS::Service",
				"Properties": {
					"TaskDefinition": "",
					"LoadBalancerInfo": {
						"ContainerName": "",
						"ContainerPort": 
					},
					"PlatformVersion": "",
					"NetworkConfiguration": {
						"AwsvpcConfiguration": {
							"Subnets": [
								"",
								""
							],
							"SecurityGroups": [
								"",
								""
							],
							"AssignPublicIp": ""
						}
					}
				}				
			}
		}
	]
```

Each property is specified with a string\. 
+ **TaskDefinition** – Required\. This is the task definition for the Amazon ECS service to deploy\. It is specified with the ARN of the task definition\. The ARN format is `arn:aws:ecs:region:account-id:task-definition/task-definition-family-name:task-definition-revision-number`\. For more information, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.
+ **ContainerName** – Required\. This is the name of the Amazon ECS container that contains your Amazon ECS application\. It must be a container specified in your Amazon ECS task definition\.
+ **Port** – Required\. This is the port where your load balancer reroutes traffic during a deployment\.
+ **PlatformVersion**: Optional\. The platform version of the Fargate tasks in the deployed Amazon ECS service\. For more information, see [AWS Fargate Platform Versions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/platform_versions.html)\.
+  **NetworkConfiguration**: Optional\. Under **AwsvpcConfiguration**, you can specify the following\. For more information, see [AwsVpcConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_AwsVpcConfiguration.html) in the *Amazon ECS Container Service API Reference*\. 
  + **Subnets**: Optional\. A comma\-separated list of one or more subnets in your Amazon ECS service\.
  + **SecurityGroups**: Optional\. A comma\-separated list of one or more security groups in your Amazon Elastic Container Service\.
  + **AssignPublicIp**: Optional\. A string that specifies whether your Amazon ECS service's elastic network interface receives a public IP address\. The valid values are `ENABLED` and `DISABLED`\.
**Note**  
 All or none of the settings under **NetworkConfiguration** must be specified\. For example, if you want to specify **Subnets**, you must also specify **SecurityGroups** and **AssignPublicIp**\. If none is specified, CodeDeploy uses the current network Amazon ECS settings\. 
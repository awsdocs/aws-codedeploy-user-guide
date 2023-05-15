# AppSpec 'resources' section \(Amazon ECS and AWS Lambda deployments only\)<a name="reference-appspec-file-structure-resources"></a>

 The content in the `'resources'` section of the AppSpec file varies, depending on the compute platform of your deployment\. The `'resources'` section for an Amazon ECS deployment contains your Amazon ECS task definition, container and port for routing traffic to your updated Amazon ECS task set, and other optional information\. The `'resources'` section for an AWS Lambda deployment contains the name, alias, current version, and target version of a Lambda function\. 

**Topics**
+ [AppSpec 'resources' section for AWS Lambda deployments](#reference-appspec-file-structure-resources-lambda)
+ [AppSpec 'resources' section for Amazon ECS deployments](#reference-appspec-file-structure-resources-ecs)

## AppSpec 'resources' section for AWS Lambda deployments<a name="reference-appspec-file-structure-resources-lambda"></a>

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
+ `name` – Required\. This is the name of the Lambda function to deploy\.
+ `alias` – Required\. This is the name of the alias to the Lambda function\.
+ `currentversion` – Required\. This is the version of the Lambda function traffic currently points to\. This value must be a valid positive integer\.
+ `targetversion` – Required\. This is the version of the Lambda function traffic is shifted to\. This value must be a valid positive integer\.

## AppSpec 'resources' section for Amazon ECS deployments<a name="reference-appspec-file-structure-resources-ecs"></a>

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
        CapacityProviderStrategy:
          - Base: Integer
            CapacityProvider: "capacityProviderA"
            Weight: Integer
          - Base: Integer
            CapacityProvider: "capacityProviderB"
            Weight: Integer
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
              },
             "CapacityProviderStrategy": [
                {
                    "Base" : Integer,
                    "CapacityProvider" : "capacityProviderA",
                    "Weight" : Integer
                },
                {
                    "Base" : Integer,
                    "CapacityProvider" : "capacityProviderB",
                    "Weight" : Integer
                }
            ]
          }             
      }
  }
]
```

Each property is specified with a string except for `ContainerPort`, which is a number\. 
+ `TaskDefinition` – Required\. This is the task definition for the Amazon ECS service to deploy\. It is specified with the ARN of the task definition\. The ARN format is `arn:aws:ecs:aws-region:account-id:task-definition/task-definition-family:task-definition-revision`\. For more information, see [Amazon Resource Names \(ARNs\) and AWS service namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.
+ `ContainerName` – Required\. This is the name of the Amazon ECS container that contains your Amazon ECS application\. It must be a container specified in your Amazon ECS task definition\.
+ `ContainerPort` – Required\. This is the port on the container where traffic will be routed to\.
+ `PlatformVersion`: Optional\. The platform version of the Fargate tasks in the deployed Amazon ECS service\. For more information, see [AWS Fargate platform versions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/platform_versions.html)\. If not specified, `LATEST` is used by default\.
+  `NetworkConfiguration`: Optional\. Under `AwsvpcConfiguration`, you can specify the following\. For more information, see [AwsVpcConfiguration](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_AwsVpcConfiguration.html) in the *Amazon ECS Container Service API Reference*\. 
  + `Subnets`: Optional\. A comma\-separated list of one or more subnets in your Amazon ECS service\.
  + `SecurityGroups`: Optional\. A comma\-separated list of one or more security groups in your Amazon Elastic Container Service\.
  + `AssignPublicIp`: Optional\. A string that specifies whether your Amazon ECS service's elastic network interface receives a public IP address\. The valid values are `ENABLED` and `DISABLED`\.
**Note**  
 All or none of the settings under `NetworkConfiguration` must be specified\. For example, if you want to specify `Subnets`, you must also specify `SecurityGroups` and `AssignPublicIp`\. If none is specified, CodeDeploy uses the current network Amazon ECS settings\. 
+ `CapacityProviderStrategy`: Optional\. A list of Amazon ECS capacity providers you want to use for your deployment\. For more information, see [Amazon ECS capacity providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html) in the *Amazon Elastic Container Service Developer Guide*\. For each capacity provider, you can specify the following settings\. For details on these settings, see [AWS::ECS::ServiceCapacityProviderStrategyItem](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-service-capacityproviderstrategyitem.html) in the *AWS CloudFormation User Guide*
  + `Base`: Optional\. The base value designates how many tasks, at a minimum, to run on the specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\. If no value is specified, the default value of 0 is used\.
  + `CapacityProvider`: Optional\. The short name of the capacity provider\. Example: *capacityProviderA*
  + `Weight`: Optional\.

    The *weight* value designates the relative percentage of the total number of tasks launched that should use the specified capacity provider\. The `weight` value is taken into consideration after the `base` value, if defined, is satisfied\.

    If no `weight` value is specified, the default value of `0` is used\. When multiple capacity providers are specified within a capacity provider strategy, at least one of the capacity providers must have a weight value greater than zero and any capacity providers with a weight of `0` will not be used to place tasks\. If you specify multiple capacity providers in a strategy that all have a weight of `0`, any `RunTask` or `CreateService` actions using the capacity provider strategy will fail\.

     An example scenario for using weights is defining a strategy that contains two capacity providers and both have a weight of `1`, then when the `base` is satisfied, the tasks will be split evenly across the two capacity providers\. Using that same logic, if you specify a weight of `1` for *capacityProviderA* and a weight of `4` for *capacityProviderB*, then for every one task that is run using *capacityProviderA*, four tasks would use *capacityProviderB*\.
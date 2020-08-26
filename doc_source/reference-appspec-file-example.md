# AppSpec File example<a name="reference-appspec-file-example"></a>

This topic provides example AppSpec files for an AWS Lambda and an EC2/On\-Premises deployment\.

**Topics**
+ [AppSpec File example for an Amazon ECS deployment](#appspec-file-example-ecs)
+ [AppSpec File example for an AWS Lambda deployment](#appspec-file-example-lambda)
+ [AppSpec File example for an EC2/On\-Premises deployment](#appspec-file-example-server)

## AppSpec File example for an Amazon ECS deployment<a name="appspec-file-example-ecs"></a>

 Here is an example of an AppSpec file written in YAML for deploying an Amazon ECS service\. 

```
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:us-east-1:111222333444:task-definition/my-task-definition-family-name:1"
        LoadBalancerInfo:
          ContainerName: "SampleApplicationName"
          ContainerPort: 80
# Optional properties
        PlatformVersion: "LATEST"
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets: ["subnet-1234abcd","subnet-5678abcd"]
            SecurityGroups: ["sg-12345678"]
            AssignPublicIp: "ENABLED"
Hooks:
  - BeforeInstall: "LambdaFunctionToValidateBeforeInstall"
  - AfterInstall: "LambdaFunctionToValidateAfterTraffic"
  - AfterAllowTestTraffic: "LambdaFunctionToValidateAfterTestTrafficStarts"
  - BeforeAllowTraffic: "LambdaFunctionToValidateBeforeAllowingProductionTraffic"
  - AfterAllowTraffic: "LambdaFunctionToValidateAfterAllowingProductionTraffic"
```

 Here is a version of the preceding example written in JSON\. 

```
{
	"version": 0.0,
	"Resources": [
		{
			"TargetService": {
				"Type": "AWS::ECS::Service",
				"Properties": {
					"TaskDefinition": "arn:aws:ecs:us-east-1:111222333444:task-definition/my-task-definition-family-name:1",
					"LoadBalancerInfo": {
						"ContainerName": "SampleApplicationName",
						"ContainerPort": 80
					},
					"PlatformVersion": "LATEST",
					"NetworkConfiguration": {
						"AwsvpcConfiguration": {
							"Subnets": [
								"subnet-1234abcd",
								"subnet-5678abcd"
							],
							"SecurityGroups": [
								"sg-12345678"
							],
							"AssignPublicIp": "ENABLED"
						}
					}
				}				
			}
		}
	],
	"Hooks": [
		{
			"BeforeInstall": "LambdaFunctionToValidateBeforeInstall"
		},
		{
			"AfterInstall": "LambdaFunctionToValidateAfterTraffic"
		},
		{
			"AfterAllowTestTraffic": "LambdaFunctionToValidateAfterTestTrafficStarts"
		},
		{
			"BeforeAllowTraffic": "LambdaFunctionToValidateBeforeAllowingProductionTraffic"
		},
		{
			"AfterAllowTraffic": "LambdaFunctionToValidateAfterAllowingProductionTraffic"
		}
	]
}
```

Here is the sequence of events during deployment:

1.  Before the updated Amazon ECS application is installed on the replacement task set, the Lambda function called `LambdaFunctionToValidateBeforeInstall` runs\. 

1.  After the updated Amazon ECS application is installed on the replacement task set, but before it receives any traffic, the Lambda function called `LambdaFunctionToValidateAfterTraffic` runs\. 

1.  After the Amazon ECS application on the replacement task set starts receiving traffic from the test listener, the Lambda function called `LambdaFunctionToValidateAfterTestTrafficStarts` runs\. This function likely runs validation tests to determine if the deployment continues\. If you do not specify a test listener in your deployment group, this hook is ignored\. 

1.  After any validation tests in the `AfterAllowTestTraffic` hook are completed, and before production traffic is served to the updated Amazon ECS application, the Lambda function called `LambdaFunctionToValidateBeforeAllowingProductionTraffic` runs\. 

1.  After production traffic is served to the updated Amazon ECS application on the replacement task set, the Lambda function called `LambdaFunctionToValidateAfterAllowingProductionTraffic` runs\. 

 The Lambda functions that run during any hook can perform validation tests or gather traffic metrics\. 

## AppSpec File example for an AWS Lambda deployment<a name="appspec-file-example-lambda"></a>

 Here is an example of an AppSpec file written in YAML for deploying a Lambda function version\. 

```
version: 0.0
Resources:
  - myLambdaFunction:
      Type: AWS::Lambda::Function
      Properties:
        Name: "myLambdaFunction"
        Alias: "myLambdaFunctionAlias"
        CurrentVersion: "1"
        TargetVersion: "2"
Hooks:
  - BeforeAllowTraffic: "LambdaFunctionToValidateBeforeTrafficShift"
  - AfterAllowTraffic: "LambdaFunctionToValidateAfterTrafficShift"
```

 Here is a version of the preceding example written in JSON\. 

```
{
 	"version": 0.0,
 	"Resources": [{
 		"myLambdaFunction": {
 			"Type": "AWS::Lambda::Function",
 			"Properties": {
 				"Name": "myLambdaFunction",
 				"Alias": "myLambdaFunctionAlias",
 				"CurrentVersion": "1",
 				"TargetVersion": "2"
 			}
 		}
 	}],
 	"Hooks": [{
 			"BeforeAllowTraffic": "LambdaFunctionToValidateBeforeTrafficShift"
      },
      {
 			"AfterAllowTraffic": "LambdaFunctionToValidateAfterTrafficShift"
 		}
 	]
 }
```

Here is the sequence of events during deployment:

1. Before shifting traffic from version 1 of a Lambda function called `myLambdaFunction` to version 2, run a Lambda function called `LambdaFunctionToValidateBeforeTrafficShift` that validates the deployment is ready to start traffic shifting\.

1. If `LambdaFunctionToValidateBeforeTrafficShift` returned an exit code of 0 \(success\), begin shifting traffic to version 2 of `myLambdaFunction`\. The deployment configuration for this deployment determines the rate at which traffic is shifted\.

1. After the shifting of traffic from version 1 of a Lambda function called `myLambdaFunction` to version 2 is complete, run a Lambda function called `LambdaFunctionToValidateAfterTrafficShift` that validates the deployment was completed successfully\.

## AppSpec File example for an EC2/On\-Premises deployment<a name="appspec-file-example-server"></a>

Here is an example of an AppSpec file for an in\-place deployment to an Amazon Linux, Ubuntu Server, or RHEL instance\. 

**Note**  
 Deployments to Windows Server instances do not support the `runas` element\. If you are deploying to Windows Server instances, do not include it in your AppSpec file\. 

```
version: 0.0
os: linux
files:
  - source: Config/config.txt
    destination: /webapps/Config
  - source: source
    destination: /webapps/myApp
hooks:
  BeforeInstall:
    - location: Scripts/UnzipResourceBundle.sh
    - location: Scripts/UnzipDataBundle.sh
  AfterInstall:
    - location: Scripts/RunResourceTests.sh
      timeout: 180
  ApplicationStart:
    - location: Scripts/RunFunctionalTests.sh
      timeout: 3600
  ValidateService:
    - location: Scripts/MonitorService.sh
      timeout: 3600
      runas: codedeployuser
```

For a Windows Server instance, change `os: linux` to `os: windows`\. Also, you must fully qualify the `destination` paths \(for example, `c:\temp\webapps\Config` and `c:\temp\webapps\myApp`\)\. Do not include the `runas` element\. 

Here is the sequence of events during deployment:

1. Run the script located at `Scripts/UnzipResourceBundle.sh`\.

1. If the previous script returned an exit code of 0 \(success\), run the script located at `Scripts/UnzipDataBundle.sh`\.

1. Copy the file from the path of `Config/config.txt` to the path `/webapps/Config/config.txt`\.

1. Recursively copy all the files in the `source` directory to the `/webapps/myApp` directory\.

1. Run the script located at `Scripts/RunResourceTests.sh` with a timeout of 180 seconds \(3 minutes\)\.

1. Run the script located at `Scripts/RunFunctionalTests.sh` with a timeout of 3600 seconds \(1 hour\)\.

1. Run the script located at `Scripts/MonitorService.sh` as the user `codedeploy` with a timeout of 3600 seconds \(1 hour\)\.
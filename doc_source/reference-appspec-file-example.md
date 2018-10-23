# AppSpec File Example<a name="reference-appspec-file-example"></a>

This topic provides example AppSpec files for an AWS Lambda and an EC2/On\-Premises deployment\.

**Topics**
+ [AppSpec File Example for an AWS Lambda Deployment](#appspec-file-example-lambda)
+ [AppSpec File Example for an EC2/On\-Premises Deployment](#appspec-file-example-server)

## AppSpec File Example for an AWS Lambda Deployment<a name="appspec-file-example-lambda"></a>

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

## AppSpec File Example for an EC2/On\-Premises Deployment<a name="appspec-file-example-server"></a>

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
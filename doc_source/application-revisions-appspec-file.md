# Add an application specification file to a revision for CodeDeploy<a name="application-revisions-appspec-file"></a>

This topic shows how to add an AppSpec file to your deployment\. It also includes templates to create an AppSpec file for an AWS Lambda and EC2/On\-Premises deployment\.

**Topics**
+ [Add an AppSpec file for an Amazon ECS deployment](#add-appspec-file-ecs)
+ [Add an AppSpec file for an AWS Lambda deployment](#add-appspec-file-lambda)
+ [Add an AppSpec file for an EC2/On\-Premises deployment](#add-appspec-file-server)

## Add an AppSpec file for an Amazon ECS deployment<a name="add-appspec-file-ecs"></a>

For a deployment to an Amazon ECS compute platform:
+ The AppSpec file specifies the Amazon ECS task definition used for the deployment, a container name and port mapping used to route traffic, and optional Lambda functions run after deployment lifecycle events\.
+ A revision is the same as an AppSpec file\.
+ An AppSpec file can be written using JSON or YAML\.
+ An AppSpec file can be saved as a text file or entered directly into a console when you create a deployment\. For more information, see [ Create an Amazon ECS Compute Platform deployment \(console\) ](deployments-create-console-ecs.md)\.

**To create an AppSpec file**

1. Copy the JSON or YAML template into a text editor or into the AppSpec editor in the console\.

1. Modify the template as needed\.

1. Use a JSON or YAML validator to validate your AppSpec file\. If you use the AppSpec editor, the file is validated when you choose **Create deployment**\.

1. If you use a text editor, save the file\. If you use the AWS CLI to create your deployment, reference the AppSpec file if it's on your hard drive or in an Amazon S3 bucket\. If you use the console, you must push your AppSpec file to Amazon S3\.

### YAML AppSpec file template for an Amazon ECS deployment with instructions<a name="app-spec-template-yaml-ecs"></a>

The following is a YAML template of an AppSpec file for an Amazon ECS deployment with all available options\. For information about lifecycle events to use in the `hooks` section, see [AppSpec 'hooks' section for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\.

```
# This is an appspec.yml template file for use with an Amazon ECS deployment in CodeDeploy.
# The lines in this template that start with the hashtag are 
#   comments that can be safely left in the file or 
#   ignored.
# For help completing this file, see the "AppSpec File Reference" in the  
#   "CodeDeploy User Guide" at
#   https://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html
version: 0.0
# In the Resources section, you must specify the following: the Amazon ECS service, task definition name, 
# and the name and port of the your load balancer to route traffic,
# target version, and (optional) the current version of your AWS Lambda function. 
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "" # Specify the ARN of your task definition (arn:aws:ecs:region:account-id:task-definition/task-definition-family-name:task-definition-revision-number)
        LoadBalancerInfo: 
          ContainerName: "" # Specify the name of your Amazon ECS application's container
          ContainerPort: "" # Specify the port for your container where traffic reroutes 
# Optional properties
        PlatformVersion: "" # Specify the version of your Amazon ECS Service
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets: ["",""] # Specify one or more comma-separated subnets in your Amazon ECS service
            SecurityGroups: ["",""] # Specify one or more comma-separated security groups in your Amazon ECS service
            AssignPublicIp: "" # Specify "ENABLED" or "DISABLED"             
# (Optional) In the Hooks section, specify a validation Lambda function to run during 
# a lifecycle event. 
Hooks:
# Hooks for Amazon ECS deployments are:
    - BeforeInstall: "" # Specify a Lambda function name or ARN
    - AfterInstall: "" # Specify a Lambda function name or ARN
    - AfterAllowTestTraffic: "" # Specify a Lambda function name or ARN
    - BeforeAllowTraffic: "" # Specify a Lambda function name or ARN
    - AfterAllowTraffic: "" # Specify a Lambda function name or ARN
```

### JSON AppSpec file for an Amazon ECS deployment template<a name="app-spec-template-json-ecs"></a>

The following is a JSON template for an AppSpec file for an Amazon ECS deployment with all available options\. For template instructions, refer to comments in the YAML version in the previous section\. For information about lifecycle events to use in the `hooks` section, see [AppSpec 'hooks' section for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\.

```
{
	"version": 0.0,
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
	],
	"Hooks": [
		{
			"BeforeInstall": ""
		},
		{
			"AfterInstall": ""
		},
		{
			"AfterAllowTestTraffic": ""
		},
		{
			"BeforeAllowTraffic": ""
		},
		{
			"AfterAllowTraffic": ""
		}
	]
}
```

## Add an AppSpec file for an AWS Lambda deployment<a name="add-appspec-file-lambda"></a>

For a deployment to an AWS Lambda compute platform:
+ The AppSpec file contains instructions about the Lambda functions to be deployed and used for deployment validation\.
+ A revision is the same as an AppSpec file\.
+ An AppSpec file can be written using JSON or YAML\.
+ An AppSpec file can be saved as a text file or entered directly into a console AppSpec editor when creating a deployment\. For more information, see [Create an AWS Lambda Compute Platform deployment \(console\)](deployments-create-console-lambda.md)\.

To create an AppSpec file:

1. Copy the JSON or YAML template into a text editor or into the AppSpec editor in the console\.

1. Modify the template as needed\.

1. Use a JSON or YAML validator to validate your AppSpec file\. If you use the AppSpec editor, the file is validated when you choose **Create deployment**\.

1. If you use a text editor, save the file\. If you use the AWS CLI to create your deployment, reference the AppSpec file if it's on your hard drive or in an Amazon S3 bucket\. If you use the console, you must push your AppSpec file to Amazon S3\.

### YAML AppSpec file template for an AWS Lambda deployment with instructions<a name="app-spec-template-yaml-lambda"></a>

For information about lifecycle events to use in the hooks section, see [AppSpec 'hooks' section for an AWS Lambda deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-lambda)\.

```
# This is an appspec.yml template file for use with an AWS Lambda deployment in CodeDeploy.
# The lines in this template starting with the hashtag symbol are 
#   instructional comments and can be safely left in the file or 
#   ignored.
# For help completing this file, see the "AppSpec File Reference" in the  
#   "CodeDeploy User Guide" at
#   https://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html
version: 0.0
# In the Resources section specify the name, alias, 
# target version, and (optional) the current version of your AWS Lambda function. 
Resources:
  - MyFunction: # Replace "MyFunction" with the name of your Lambda function 
      Type: AWS::Lambda::Function
      Properties:
        Name: "" # Specify the name of your Lambda function
        Alias: "" # Specify the alias for your Lambda function
        CurrentVersion: "" # Specify the current version of your Lambda function
        TargetVersion: "" # Specify the version of your Lambda function to deploy
# (Optional) In the Hooks section, specify a validation Lambda function to run during 
# a lifecycle event. Replace "LifeCycleEvent" with BeforeAllowTraffic
# or AfterAllowTraffic. 
Hooks:
    - LifeCycleEvent: "" # Specify a Lambda validation function between double-quotes.
```

### JSON AppSpec file for an AWS Lambda deployment template<a name="app-spec-template-json-lambda"></a>

In the following template, replace "MyFunction" with the name of your AWS Lambda function\. In the optional Hooks section, replace the lifecycle events with BeforeAllowTraffic or AfterAllowTraffic\.

For information about lifecycle events to use in the Hooks section, see [AppSpec 'hooks' section for an AWS Lambda deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-lambda)\.

```
{
 	"version": 0.0,
 	"Resources": [{
 		"MyFunction": {
 			"Type": "AWS::Lambda::Function",
 			"Properties": {
 				"Name": "",
 				"Alias": "",
 				"CurrentVersion": "",
 				"TargetVersion": ""
 			}
 		}
 	}],
 	"Hooks": [{
 			"LifeCycleEvent": ""
 		}
 	]
 }
```

## Add an AppSpec file for an EC2/On\-Premises deployment<a name="add-appspec-file-server"></a>

Without an AppSpec file, CodeDeploy cannot map the source files in your application revision to their destinations or run scripts for your deployment to an EC2/On\-Premises compute platform, \.

Each revision must contain only one AppSpec file\. 

To add an AppSpec file to a revision:

1. Copy the template into a text editor\.

1. Modify the template as needed\.

1. Use a YAML validator to check the validity of your AppSpec file\. 

1. Save the file as `appspec.yml` in the root directory of the revision\.

1. Run one of the following commands to verify that you have placed your AppSpec file in the root directory:
   + For Linux, macOS, or Unix:

     ```
     find /path/to/root/directory -name appspec.yml
     ```

     There will be no output if the AppSpec file is not found there\.
   + For Windows:

     ```
     dir path\to\root\directory\appspec.yml
     ```

     A **File Not Found** error will be displayed if the AppSpec file is not stored there\.

1. Push the revision to Amazon S3 or GitHub\. 

   For instructions, see [Push a revision for CodeDeploy to Amazon S3 \(EC2/On\-Premises deployments only\)](application-revisions-push.md)\.

### AppSpec file template for an EC2/On\-Premises deployment with instructions<a name="app-spec-template-server"></a>

**Note**  
 Deployments to Windows Server instances do not support the `runas` element\. If you are deploying to Windows Server instances, do not include it in your AppSpec file\. 

```
# This is an appspec.yml template file for use with an EC2/On-Premises deployment in CodeDeploy.
# The lines in this template starting with the hashtag symbol are 
#   instructional comments and can be safely left in the file or 
#   ignored.
# For help completing this file, see the "AppSpec File Reference" in the  
#   "CodeDeploy User Guide" at
#   https://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html
version: 0.0
# Specify "os: linux" if this revision targets Amazon Linux, 
#   Red Hat Enterprise Linux (RHEL), or Ubuntu Server  
#   instances.
# Specify "os: windows" if this revision targets Windows Server instances.
# (You cannot specify both "os: linux" and "os: windows".)
os: linux 
# os: windows
# During the Install deployment lifecycle event (which occurs between the 
#   BeforeInstall and AfterInstall events), copy the specified files 
#   in "source" starting from the root of the revision's file bundle 
#   to "destination" on the Amazon EC2 instance.
# Specify multiple "source" and "destination" pairs if you want to copy 
#   from multiple sources or to multiple destinations.
# If you are not copying any files to the Amazon EC2 instance, then remove the
#   "files" section altogether. A blank or incomplete "files" section
#   may cause associated deployments to fail.
files:
  - source: 
    destination:
  - source:
    destination:
# For deployments to Amazon Linux, Ubuntu Server, or RHEL instances,
#   you can specify a "permissions" 
#   section here that describes special permissions to apply to the files 
#   in the "files" section as they are being copied over to 
#   the Amazon EC2 instance.
#   For more information, see the documentation.
# If you are deploying to Windows Server instances,
#   then remove the 
#   "permissions" section altogether. A blank or incomplete "permissions"
#   section may cause associated deployments to fail.
permissions:
  - object:
    pattern:
    except:
    owner:
    group:
    mode: 
    acls:
      -
    context:
      user:
      type:
      range:
    type:
      -
# If you are not running any commands on the Amazon EC2 instance, then remove 
#   the "hooks" section altogether. A blank or incomplete "hooks" section
#   may cause associated deployments to fail.
hooks:
# For each deployment lifecycle event, specify multiple "location" entries 
#   if you want to run multiple scripts during that event.
# You can specify "timeout" as the number of seconds to wait until failing the deployment 
#   if the specified scripts do not run within the specified time limit for the 
#   specified event. For example, 900 seconds is 15 minutes. If not specified, 
#   the default is 1800 seconds (30 minutes).
#   Note that the maximum amount of time that all scripts must finish executing 
#   for each individual deployment lifecycle event is 3600 seconds (1 hour). 
#   Otherwise, the deployment will stop and CodeDeploy will consider the deployment
#   to have failed to the Amazon EC2 instance. Make sure that the total number of seconds 
#   that are specified in "timeout" for all scripts in each individual deployment 
#   lifecycle event does not exceed a combined 3600 seconds (1 hour).
# For deployments to Amazon Linux, Ubuntu Server, or RHEL instances,
#   you can specify "runas" in an event to
#   run as the specified user. For more information, see the documentation.
#   If you are deploying to Windows Server instances,
#   remove "runas" altogether.
# If you do not want to run any commands during a particular deployment
#   lifecycle event, remove that event declaration altogether. Blank or 
#   incomplete event declarations may cause associated deployments to fail.
# During the ApplicationStop deployment lifecycle event, run the commands 
#   in the script specified in "location" starting from the root of the 
#   revision's file bundle.
  ApplicationStop:
    - location: 
      timeout:
      runas:
    - location: 
      timeout:
      runas: 
# During the BeforeInstall deployment lifecycle event, run the commands 
#   in the script specified in "location".
  BeforeInstall:
    - location: 
      timeout:
      runas: 
    - location: 
      timeout:
      runas:
# During the AfterInstall deployment lifecycle event, run the commands 
#   in the script specified in "location".
  AfterInstall:
    - location:     
      timeout: 
      runas:
    - location: 
      timeout:
      runas:
# During the ApplicationStart deployment lifecycle event, run the commands 
#   in the script specified in "location".
  ApplicationStart:
    - location:     
      timeout: 
      runas:
    - location: 
      timeout:
      runas:
# During the ValidateService deployment lifecycle event, run the commands 
#   in the script specified in "location".
  ValidateService:
    - location:     
      timeout: 
      runas:
    - location: 
      timeout:
      runas:
```
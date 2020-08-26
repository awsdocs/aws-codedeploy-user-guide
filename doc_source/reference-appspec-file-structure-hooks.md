# AppSpec 'hooks' section<a name="reference-appspec-file-structure-hooks"></a>

The content in the `'hooks'` section of the AppSpec file varies, depending on the compute platform for your deployment\. The `'hooks'` section for an EC2/On\-Premises deployment contains mappings that link deployment lifecycle event hooks to one or more scripts\. The `'hooks'` section for a Lambda or an Amazon ECS deployment specifies Lambda validation functions to run during a deployment lifecycle event\. If an event hook is not present, no operation is executed for that event\. This section is required only if you are running scripts or Lambda validation functions as part of the deployment\.

**Topics**
+ [AppSpec 'hooks' section for an Amazon ECS deployment](#appspec-hooks-ecs)
+ [AppSpec 'hooks' section for an AWS Lambda deployment](#appspec-hooks-lambda)
+ [AppSpec 'hooks' section for an EC2/On\-Premises deployment](#appspec-hooks-server)

## AppSpec 'hooks' section for an Amazon ECS deployment<a name="appspec-hooks-ecs"></a>

**Topics**
+ [List of lifecycle event hooks for an Amazon ECS deployment](#reference-appspec-file-structure-hooks-list-ecs)
+ [Run order of hooks in an Amazon ECS deployment](#reference-appspec-file-structure-hooks-run-order-ecs)
+ [Structure of 'hooks' section](#reference-appspec-file-structure-hooks-section-structure-ecs)
+ [Sample Lambda 'hooks' function](#reference-appspec-file-structure-hooks-section-structure-ecs-sample-function)

### List of lifecycle event hooks for an Amazon ECS deployment<a name="reference-appspec-file-structure-hooks-list-ecs"></a>

An AWS Lambda hook is one Lambda function specified with a string on a new line after the name of the lifecycle event\. Each hook is executed once per deployment\. Following are descriptions of the lifecycle events where you can run a hook during an Amazon ECS deployment\. 
+  `BeforeInstall` – Use to run tasks before the replacement task set is created\. One target group is associated with the original task set\. If an optional test listener is specified, it is associated with the original task set\. A rollback is not possible at this point\. 
+  `AfterInstall` – Use to run tasks after the replacement task set is created and one of the target groups is associated with it\. If an optional test listener is specified, it is associated with the original task set\. The results of a hook function at this lifecycle event can trigger a rollback\.
+  `AfterAllowTestTraffic` – Use to run tasks after the test listener serves traffic to the replacement task set\. The results of a hook function at this point can trigger a rollback\.
+  `BeforeAllowTraffic` – Use to run tasks after the second target group is associated with the replacement task set, but before traffic is shifted to the replacement task set\. The results of a hook function at this lifecycle event can trigger a rollback\. 
+  `AfterAllowTraffic` – Use to run tasks after the second target group serves traffic to the replacement task set\. The results of a hook function at this lifecycle event can trigger a rollback\. 

For more information, see [What happens during an Amazon ECS deployment](deployment-steps-ecs.md#deployment-steps-what-happens) and [Tutorial: Deploy an Amazon ECS service with a validation test](tutorial-ecs-deployment-with-hooks.md)\.

### Run order of hooks in an Amazon ECS deployment<a name="reference-appspec-file-structure-hooks-run-order-ecs"></a>

In an Amazon ECS deployment, event hooks run in the following order:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lifecycle-event-order-ecs.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**Note**  
The **Start**, **Install**, **TestTraffic**, **AllowTraffic**, and **End** events in the deployment cannot be scripted, which is why they appear in gray in this diagram\.

### Structure of 'hooks' section<a name="reference-appspec-file-structure-hooks-section-structure-ecs"></a>

The following are examples of the structure of the `'hooks'` section\.

Using YAML:

```
Hooks:
  - BeforeInstall: "BeforeInstallHookFunctionName"
  - AfterInstall: "AfterInstallHookFunctionName"
  - AfterAllowTestTraffic: "AfterAllowTestTrafficHookFunctionName"
  - BeforeAllowTraffic: "BeforeAllowTrafficHookFunctionName"
  - AfterAllowTraffic: "AfterAllowTrafficHookFunctionName"
```

Using JSON:

```
"Hooks": [
		{
			"BeforeInstall": "BeforeInstallHookFunctionName"
		},
		{
			"AfterInstall": "AfterInstallHookFunctionName"
		},
		{
			"AfterAllowTestTraffic": "AfterAllowTestTrafficHookFunctionName"
		},
		{
			"BeforeAllowTraffic": "BeforeAllowTrafficHookFunctionName"
		},
		{
			"AfterAllowTraffic": "AfterAllowTrafficHookFunctionName"
		}
	]
}
```

### Sample Lambda 'hooks' function<a name="reference-appspec-file-structure-hooks-section-structure-ecs-sample-function"></a>

Use the `'hooks'` section to specify a Lambda function that CodeDeploy can call to validate a Lambda deployment\. You can use the same function or a different one for the `BeforeInstall`, `AfterInstall`, `AfterAllowTestTraffic`, `BeforeAllowTraffic`, and `AfterAllowTraffic` deployment lifecyle events\. Following completion of the validation tests, the Lambda `AfterAllowTraffic` function calls back CodeDeploy and delivers a result of `Succeeded` or `Failed`\. 

**Important**  
The deployment is considered to have failed if CodeDeploy is not notified by the Lambda validation function within one hour\.

 Before invoking a Lambda hook function, the server must be notified of the deployment ID and the lifecycle event hook execution ID using the `putLifecycleEventHookExecutionStatus` command\.

 The following is a sample Lambda hook function written in Node\.js\. 

```
'use strict';

const aws = require('aws-sdk');
const codedeploy = new aws.CodeDeploy({apiVersion: '2014-10-06'});

exports.handler = (event, context, callback) => {
    //Read the DeploymentId from the event payload.
    var deploymentId = event.DeploymentId;

    //Read the LifecycleEventHookExecutionId from the event payload
    var lifecycleEventHookExecutionId = event.LifecycleEventHookExecutionId;

    /*
     Enter validation tests here.
    */

    // Prepare the validation test results with the deploymentId and
    // the lifecycleEventHookExecutionId for AWS CodeDeploy.
    var params = {
        deploymentId: deploymentId,
        lifecycleEventHookExecutionId: lifecycleEventHookExecutionId,
        status: 'Succeeded' // status can be 'Succeeded' or 'Failed'
    };
    
    // Pass AWS CodeDeploy the prepared validation test results.
    codedeploy.putLifecycleEventHookExecutionStatus(params, function(err, data) {
        if (err) {
            // Validation failed.
            callback('Validation test failed');
        } else {
            // Validation succeeded.
            callback(null, 'Validation test succeeded');
        }
    });
};
```

## AppSpec 'hooks' section for an AWS Lambda deployment<a name="appspec-hooks-lambda"></a>

**Topics**
+ [List of lifecycle event hooks for an AWS Lambda deployment](#reference-appspec-file-structure-hooks-list-lambda)
+ [Run order of hooks in a Lambda function version deployment](#reference-appspec-file-structure-hooks-run-order-lambda)
+ [Structure of 'hooks' section](#reference-appspec-file-structure-hooks-section-structure-lambda)
+ [Sample Lambda 'hooks' function](#reference-appspec-file-structure-hooks-section-structure-lambda-sample-function)

### List of lifecycle event hooks for an AWS Lambda deployment<a name="reference-appspec-file-structure-hooks-list-lambda"></a>

An AWS Lambda hook is one Lambda function specified with a string on a new line after the name of the lifecycle event\. Each hook is executed once per deployment\. Here are descriptions of the hooks available for use in your AppSpec file\. 
+ **BeforeAllowTraffic** – Use to run tasks before traffic is shifted to the deployed Lambda function version\.
+ **AfterAllowTraffic** – Use to run tasks after all traffic is shifted to the deployed Lambda function version\.

### Run order of hooks in a Lambda function version deployment<a name="reference-appspec-file-structure-hooks-run-order-lambda"></a>

In a serverless Lambda function version deployment, event hooks run in the following order:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lifecycle-event-order-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**Note**  
The **Start**, **AllowTraffic**, and **End** events in the deployment cannot be scripted, which is why they appear in gray in this diagram\.

### Structure of 'hooks' section<a name="reference-appspec-file-structure-hooks-section-structure-lambda"></a>

The following are examples of the structure of the 'hooks' section\.

Using YAML:

```
hooks:
   - BeforeAllowTraffic: BeforeAllowTrafficHookFunctionName
   - AfterAllowTraffic: AfterAllowTrafficHookFunctionName
```

Using JSON:

```
"hooks": [{
    "BeforeAllowTraffic": "BeforeAllowTrafficHookFunctionName"
    },
    {
    "AfterAllowTraffic": "AfterAllowTrafficHookFunctionName"
}]
```

### Sample Lambda 'hooks' function<a name="reference-appspec-file-structure-hooks-section-structure-lambda-sample-function"></a>

Use the 'hooks' section to specify a Lambda function that CodeDeploy can call to validate a Lambda deployment\. You can use the same function or a different one for the `BeforeAllowTraffic` and `AfterAllowTraffic` deployment lifecyle events\. Following completion of the validation tests, the Lambda validation function calls back CodeDeploy and delivers a result of `Succeeded` or `Failed`\. 

**Important**  
The deployment is considered to have failed if CodeDeploy is not notified by the Lambda validation function within one hour\.

 Before invoking a Lambda hook function, the server must be notified of the deployment ID and the lifecycle event hook execution ID using the `putLifecycleEventHookExecutionStatus` command\.

 The following is a sample Lambda hook function written in Node\.js\. 

```
'use strict';

const aws = require('aws-sdk');
const codedeploy = new aws.CodeDeploy({apiVersion: '2014-10-06'});

exports.handler = (event, context, callback) => {
    //Read the DeploymentId from the event payload.
    var deploymentId = event.DeploymentId;

    //Read the LifecycleEventHookExecutionId from the event payload
    var lifecycleEventHookExecutionId = event.LifecycleEventHookExecutionId;

    /*
     Enter validation tests here.
    */

    // Prepare the validation test results with the deploymentId and
    // the lifecycleEventHookExecutionId for AWS CodeDeploy.
    var params = {
        deploymentId: deploymentId,
        lifecycleEventHookExecutionId: lifecycleEventHookExecutionId,
        status: 'Succeeded' // status can be 'Succeeded' or 'Failed'
    };
    
    // Pass AWS CodeDeploy the prepared validation test results.
    codedeploy.putLifecycleEventHookExecutionStatus(params, function(err, data) {
        if (err) {
            // Validation failed.
            callback('Validation test failed');
        } else {
            // Validation succeeded.
            callback(null, 'Validation test succeeded');
        }
    });
};
```

## AppSpec 'hooks' section for an EC2/On\-Premises deployment<a name="appspec-hooks-server"></a>

**Topics**
+ [List of lifecycle event hooks](#reference-appspec-file-structure-hooks-list)
+ [Lifecycle event hook availability](#reference-appspec-file-structure-hooks-availability)
+ [Run order of hooks in a deployment](#reference-appspec-file-structure-hooks-run-order)
+ [Structure of 'hooks' section](#reference-appspec-file-structure-hooks-section-structure)
+ [Environment variable availability for hooks](#reference-appspec-file-structure-environment-variable-availability)
+ [Hooks example](#reference-appspec-file-structure-hooks-example)

### List of lifecycle event hooks<a name="reference-appspec-file-structure-hooks-list"></a>

An EC2/On\-Premises deployment hook is executed once per deployment to an instance\. You can specify one or more scripts to run in a hook\. Each hook for a lifecycle event is specified with a string on a separate line\. Here are descriptions of the hooks available for use in your AppSpec file\. 

For information about which lifecycle event hooks are valid for which deployment and rollback types, see [Lifecycle event hook availability](#reference-appspec-file-structure-hooks-availability)\.
+ `ApplicationStop` – This deployment lifecycle event occurs even before the application revision is downloaded\. You can specify scripts for this event to gracefully stop the application or remove currently installed packages in preparation for a deployment\. The AppSpec file and scripts used for this deployment lifecycle event are from the previous successfully deployed application revision\.
**Note**  
An AppSpec file does not exist on an instance before you deploy to it\. For this reason, the `ApplicationStop` hook does not run the first time you deploy to the instance\. You can use the `ApplicationStop` hook the second time you deploy to an instance\.

   To determine the location of the last successfully deployed application revision, the CodeDeploy agent looks up the location listed in the `deployment-group-id_last_successful_install` file\. This file is located in:

   `/opt/codedeploy-agent/deployment-root/deployment-instructions` folder on Amazon Linux, Ubuntu Server, and RHEL Amazon EC2 instances\. 

  `C:\ProgramData\Amazon\CodeDeploy\deployment-instructions` folder on Windows Server Amazon EC2 instances\.

  To troubleshoot a deployment that fails during the `ApplicationStop` deployment lifecycle event, see [Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events](troubleshooting-deployments.md#troubleshooting-deployments-lifecycle-event-failures)\.
+ `DownloadBundle` – During this deployment lifecycle event, the CodeDeploy agent copies the application revision files to a temporary location: 

  `/opt/codedeploy-agent/deployment-root/deployment-group-id/deployment-id/deployment-archive` folder on Amazon Linux, Ubuntu Server, and RHEL Amazon EC2 instances\. 

  `C:\ProgramData\Amazon\CodeDeploy\deployment-group-id\deployment-id\deployment-archive` folder on Windows Server Amazon EC2 instances\. 

  This event is reserved for the CodeDeploy agent and cannot be used to run scripts\.

  To troubleshoot a deployment that fails during the `DownloadBundle` deployment lifecycle event, see [Troubleshooting a failed DownloadBundle deployment lifecycle event with UnknownError: not opened for reading](troubleshooting-deployments.md#troubleshooting-deployments-downloadbundle)\.
+ `BeforeInstall` – You can use this deployment lifecycle event for preinstall tasks, such as decrypting files and creating a backup of the current version\.
+ `Install` – During this deployment lifecycle event, the CodeDeploy agent copies the revision files from the temporary location to the final destination folder\. This event is reserved for the CodeDeploy agent and cannot be used to run scripts\.
+ `AfterInstall` – You can use this deployment lifecycle event for tasks such as configuring your application or changing file permissions\.
+ `ApplicationStart` – You typically use this deployment lifecycle event to restart services that were stopped during `ApplicationStop`\.
+ `ValidateService` – This is the last deployment lifecycle event\. It is used to verify the deployment was completed successfully\.
+ `BeforeBlockTraffic` – You can use this deployment lifecycle event to run tasks on instances before they are deregistered from a load balancer\.

  To troubleshoot a deployment that fails during the `BeforeBlockTraffic` deployment lifecycle event, see [Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events](troubleshooting-deployments.md#troubleshooting-deployments-lifecycle-event-failures)\.
+ `BlockTraffic` – During this deployment lifecycle event, internet traffic is blocked from accessing instances that are currently serving traffic\. This event is reserved for the CodeDeploy agent and cannot be used to run scripts\. 
+ `AfterBlockTraffic` – You can use this deployment lifecycle event to run tasks on instances after they are deregistered from a load balancer\. 

  To troubleshoot a deployment that fails during the `AfterBlockTraffic` deployment lifecycle event, see [Troubleshooting failed ApplicationStop, BeforeBlockTraffic, and AfterBlockTraffic deployment lifecycle events](troubleshooting-deployments.md#troubleshooting-deployments-lifecycle-event-failures)\.
+ `BeforeAllowTraffic` – You can use this deployment lifecycle event to run tasks on instances before they are registered with a load balancer\.
+ `AllowTraffic` – During this deployment lifecycle event, internet traffic is allowed to access instances after a deployment\. This event is reserved for the CodeDeploy agent and cannot be used to run scripts\.
+ `AfterAllowTraffic` – You can use this deployment lifecycle event to run tasks on instances after they are registered with a load balancer\.

### Lifecycle event hook availability<a name="reference-appspec-file-structure-hooks-availability"></a>

The following table lists the lifecycle event hooks available for each deployment and rollback scenario\.


| Lifecycle event name | In\-place deployment¹ | Blue/green deployment: Original instances | Blue/green deployment: Replacement instances | Blue/green deployment rollback: Original instances | Blue/green deployment rollback: Replacement instances | 
| --- | --- | --- | --- | --- | --- | 
| ApplicationStop | ✓ |  | ✓ |  |  | 
| DownloadBundle² | ✓ |  | ✓ |  |  | 
| BeforeInstall | ✓ |  | ✓ |  |  | 
| Install² | ✓ |  | ✓ |  |  | 
| AfterInstall | ✓ |  | ✓ |  |  | 
| ApplicationStart | ✓ |  | ✓ |  |  | 
| ValidateService | ✓ |  | ✓ |  |  | 
| BeforeBlockTraffic | ✓ | ✓ |  |  | ✓ | 
| BlockTraffic² | ✓ | ✓ |  |  | ✓ | 
| AfterBlockTraffic | ✓ | ✓ |  |  | ✓ | 
| BeforeAllowTraffic | ✓ |  | ✓ | ✓ |  | 
| AllowTraffic² | ✓ |  | ✓ | ✓ |  | 
| AfterAllowTraffic | ✓ |  | ✓ | ✓ |  | 
|  ¹Also applies to the rollback of an in\-place deployment\. ² Reserved for CodeDeploy operations\. Cannot be used to run scripts\.  | 

### Run order of hooks in a deployment<a name="reference-appspec-file-structure-hooks-run-order"></a>

**In\-place deployments**

In an in\-place deployment, including the rollback of an in\-place deployment, event hooks are run in the following order:

**Note**  
For in\-place deployments, the six hooks related to blocking and allowing traffic apply only if you specify a Classic Load Balancer, Application Load Balancer, or Network Load Balancer from Elastic Load Balancing in the deployment group\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lifecycle-event-order-in-place.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**Note**  
The **Start**, **DownloadBundle**, **Install**, and **End** events in the deployment cannot be scripted, which is why they appear in gray in this diagram\. However, you can edit the `'files'` section of the AppSpec file to specify what's installed during the **Install** event\.

**Blue/green deployments**

In a blue/green deployment, event hooks are run in the following order:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lifecycle-event-order-blue-green.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**Note**  
The **Start**, **DownloadBundle**, **Install**, **BlockTraffic**, **AllowTraffic**, and **End** events in the deployment cannot be scripted, which is why they appear in gray in this diagram\. However, you can edit the 'files' section of the AppSpec file to specify what's installed during the **Install** event\.

### Structure of 'hooks' section<a name="reference-appspec-file-structure-hooks-section-structure"></a>

The `'hooks'` section has the following structure:

```
hooks:
   deployment-lifecycle-event-name:
     - location: script-location
       timeout: timeout-in-seconds
       runas: user-name
```

You can include the following elements in a **hook** entry after the deployment lifecycle event name:

** location **  
Required\. The location in the bundle of the script file for the revision\.

** timeout **  
Optional\. The number of seconds to allow the script to execute before it is considered to have failed\. The default is 3600 seconds \(1 hour\)\.  
3600 seconds \(1 hour\) is the maximum amount of time allowed for script execution for each deployment lifecycle event\. If scripts exceed this limit, the deployment stops and the deployment to the instance fails\. Make sure the total number of seconds specified in **timeout** for all scripts in each deployment lifecycle event does not exceed this limit\.

** runas **  
Optional\. The user to impersonate when running the script\. By default, this is the CodeDeploy agent running on the instance\. CodeDeploy does not store passwords, so the user cannot be impersonated if the **runas** user needs a password\. This element applies to Amazon Linux and Ubuntu Server instances only\.

### Environment variable availability for hooks<a name="reference-appspec-file-structure-environment-variable-availability"></a>

During each deployment lifecycle event, hook scripts can access the following environment variables:

** APPLICATION\_NAME **  
The name of the application in CodeDeploy that is part of the current deployment \(for example, `WordPress_App`\)\.

** DEPLOYMENT\_ID **  
The ID CodeDeploy has assigned to the current deployment \(for example, `d-AB1CDEF23`\)\.

** DEPLOYMENT\_GROUP\_NAME **  
The name of the deployment group in CodeDeploy that is part of the current deployment \(for example, `WordPress_DepGroup`\)\.

** DEPLOYMENT\_GROUP\_ID **  
The ID of the deployment group in CodeDeploy that is part of the current deployment \(for example, `b1a2189b-dd90-4ef5-8f40-4c1c5EXAMPLE`\)\.

** LIFECYCLE\_EVENT **  
The name of the current deployment lifecycle event \(for example, `AfterInstall`\)\.

These environment variables are local to each deployment lifecycle event\.

The following script changes the listening port on an Apache HTTP server to 9090 instead of 80 if the value of **DEPLOYMENT\_GROUP\_NAME** is equal to `Staging`\. This script must be invoked during the `BeforeInstall` deployment lifecycle event:

```
if [ "$DEPLOYMENT_GROUP_NAME" == "Staging" ]
then
    sed -i -e 's/Listen 80/Listen 9090/g' /etc/httpd/conf/httpd.conf
fi
```

The following script example changes the verbosity level of messages recorded in its error log from warning to debug if the value of the **DEPLOYMENT\_GROUP\_NAME** environment variable is equal to `Staging`\. This script must be invoked during the `BeforeInstall` deployment lifecycle event:

```
if [ "$DEPLOYMENT_GROUP_NAME" == "Staging" ]
then
    sed -i -e 's/LogLevel warn/LogLevel debug/g' /etc/httpd/conf/httpd.conf
fi
```

The following script example replaces the text in the specified webpage with text that displays the value of these environment variables\. This script must be invoked during the `AfterInstall` deployment lifecycle event:

```
#!/usr/bin/python

import os
 
strToSearch="<h2>This application was deployed using CodeDeploy.</h2>"
strToReplace="<h2>This page for "+os.environ['APPLICATION_NAME']+" application and "+os.environ['DEPLOYMENT_GROUP_NAME']+" deployment group with "+os.environ['DEPLOYMENT_GROUP_ID']+" deployment group ID was generated by a "+os.environ['LIFECYCLE_EVENT']+" script during "+os.environ['DEPLOYMENT_ID']+" deployment.</h2>"
 
fp=open("/var/www/html/index.html","r")
buffer=fp.read()
fp.close()
 
fp=open("/var/www/html/index.html","w")
fp.write(buffer.replace(strToSearch,strToReplace))
fp.close()
```

### Hooks example<a name="reference-appspec-file-structure-hooks-example"></a>

Here is an example of a **hooks** entry that specifies two hooks for the `AfterInstall` lifecycle event:

```
hooks:
   AfterInstall:
     - location: Scripts/RunResourceTests.sh
       timeout: 180
     - location: Scripts/PostDeploy.sh
       timeout: 180
```

The `Scripts/RunResourceTests.sh` script runs during the `AfterInstall` stage of the deployment process\. The deployment is unsuccessful if it takes the script more than 180 seconds \(3 minutes\) to run\.

The location of scripts you specify in the 'hooks' section is relative to the root of the application revision bundle\. In the preceding example, a file named `RunResourceTests.sh` is in a directory named `Scripts`\. The `Scripts` directory is at the root level of the bundle\. For more information, see [Plan a revision for CodeDeploy](application-revisions-plan.md)\.
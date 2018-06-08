# AWS CodeDeploy Application Specification Files<a name="application-specification-files"></a>

An application specification file \(AppSpec file\), which is unique to AWS CodeDeploy, is a [YAML](http://www.yaml.org)\-formatted or [JSON](http://www.json.org)\-formatted file\. The AppSpec file is used to manage each deployment as a series of lifecycle event hooks, which are defined in the file\.

For information about how to create a well\-formed AppSpec file, see [AWS CodeDeploy AppSpec File Reference](reference-appspec-file.md)\.

**Topics**
+ [AppSpec Files on an AWS Lambda Compute Platform](#appspec-files-on-lambda-compute-platform)
+ [AppSpec Files on an EC2/On\-Premises Compute Platform](#appspec-files-on-server-compute-platform)
+ [How the AWS CodeDeploy Agent Uses the AppSpec File](#application-specification-files-agent-usage)

## AppSpec Files on an AWS Lambda Compute Platform<a name="appspec-files-on-lambda-compute-platform"></a>

If your application uses the AWS Lambda compute platform, the AppSpec file can be formatted with either YAML or JSON\. It can also be typed directly into an editor in the console\. The AppSpec file is used to specify:
+ The AWS Lambda function version to deploy\.
+ The functions to be used as validation tests\.

You can run validation Lambda functions after deployment lifecycle events\. For more information, see [AppSpec 'hooks' Section for an AWS Lambda Deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-lambda)\.

## AppSpec Files on an EC2/On\-Premises Compute Platform<a name="appspec-files-on-server-compute-platform"></a>

If your application uses the EC2/On\-Premises compute platform, the AppSpec file is always YAML\-formatted\. The AppSpec file is used to:
+ Map the source files in your application revision to their destinations on the instance\.
+ Specify custom permissions for deployed files\.
+ Specify scripts to be run on each instance at various stages of the deployment process\.

You can run scripts on an instance after many of the individual deployment lifecycle events\. AWS CodeDeploy runs only those scripts specified in the file, but those scripts can call other scripts on the instance\. You can run any type of script as long as it is supported by the operating system running on the instances\. For more information, see [AppSpec 'hooks' Section for an EC2/On\-Premises Deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-server)\. 

## How the AWS CodeDeploy Agent Uses the AppSpec File<a name="application-specification-files-agent-usage"></a>

During deployment, the AWS CodeDeploy agent looks up the name of the current event in the **hooks** section of the AppSpec file\. If the event is not found, the AWS CodeDeploy agent moves on to the next step\. If the event is found, the AWS CodeDeploy agent retrieves the list of scripts to execute\. The scripts are run sequentially, in the order in which they appear in the file\. The status of each script is logged in the AWS CodeDeploy agent log file on the instance\. 

If a script runs successfully, it returns an exit code of 0 \(zero\)\.

**Note**  
 The AWS CodeDeploy agent is not used in an AWS Lambda deployment\. 

During the **Install** event, the AWS CodeDeploy agent uses the mappings defined in the **files** section of the AppSpec file to determine which folders or files to copy from the revision to the instance\.

If the AWS CodeDeploy agent installed on the operating system doesn't match what's listed in the AppSpec file, the deployment fails\.

For information about AWS CodeDeploy agent log files, see [Working with the AWS CodeDeploy Agent](codedeploy-agent.md)\.
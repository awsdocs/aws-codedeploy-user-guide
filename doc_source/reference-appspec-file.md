# CodeDeploy AppSpec File reference<a name="reference-appspec-file"></a>

This section is a reference only\. For a conceptual overview of the AppSpec file, see [CodeDeploy application specification \(AppSpec\) files](application-specification-files.md)\.

The application specification file \(AppSpec file\) is a [YAML](http://www.yaml.org)\-formatted or JSON\-formatted file used by CodeDeploy to manage a deployment\.

**Note**  
 The name of the AppSpec file for an EC2/On\-Premises deployment must be `appspec.yml` or `appspec.json`\.

**Topics**
+ [AppSpec files on an Amazon ECS compute platform](#appspec-reference-ecs)
+ [AppSpec files on an AWS Lambda compute platform](#appspec-reference-lambda)
+ [AppSpec files on an EC2/on\-premises compute platform](#appspec-reference-server)
+ [AppSpec File structure](reference-appspec-file-structure.md)
+ [AppSpec File example](reference-appspec-file-example.md)
+ [AppSpec File spacing](#reference-appspec-file-spacing)
+ [Validate your AppSpec File and file location](reference-appspec-file-validate.md)

## AppSpec files on an Amazon ECS compute platform<a name="appspec-reference-ecs"></a>

For Amazon ECS compute platform applications, the AppSpec file is used by CodeDeploy to determine: 
+  Your Amazon ECS task definition file\. This is specified with its ARN in the `TaskDefinition` instruction in the AppSpec file\. 
+  The container and port in your replacement task set where your Application Load Balancer or Network Load Balancer reroutes traffic during a deployment\. This is specified with the `LoadBalancerInfo` instruction in the AppSpec file\. 
+  Optional information about your Amazon ECS service, such the platform version on which it runs, its subnets, and its security groups\. 
+  Optional Lambda functions to run during hooks that correspond with lifecycle events during an Amazon ECS deployment\. For more information, see [AppSpec 'hooks' section for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-ecs)\. 

## AppSpec files on an AWS Lambda compute platform<a name="appspec-reference-lambda"></a>

For AWS Lambda compute platform applications, the AppSpec file is used by CodeDeploy to determine: 
+ Which Lambda function version to deploy\.
+ Which Lambda functions to use as validation tests\.

An AppSpec file can be YAML\-formatted or JSON\-formatted\. You can also enter the contents of an AppSpec file directly into CodeDeploy console when you create a deployment\.

## AppSpec files on an EC2/on\-premises compute platform<a name="appspec-reference-server"></a>

 If your application uses the EC2/On\-Premises compute platform, the AppSpec file is named appspec\.yml\. It is used by CodeDeploy to determine:
+ What it should install onto your instances from your application revision in Amazon S3 or GitHub\.
+ Which lifecycle event hooks to run in response to deployment lifecycle events\.

An AppSpec file must be a YAML\-formatted file named `appspec.yml` and it must be placed in the root of the directory structure of an application's source code\. Otherwise, deployments fail\.

After you have a completed AppSpec file, you bundle it, along with the content to deploy, into an archive file \(zip, tar, or compressed tar\)\. For more information, see [Working with application revisions for CodeDeploy](application-revisions.md)\.

**Note**  
The tar and compressed tar archive file formats \(\.tar and \.tar\.gz\) are not supported for Windows Server instances\.

After you have a bundled archive file \(known in CodeDeploy as a *revision*\), you upload it to an Amazon S3 bucket or Git repository\. Then you use CodeDeploy to deploy the revision\. For instructions, see [Create a deployment with CodeDeploy](deployments-create.md)\.

The appspec\.yml for an EC2/On\-Premises compute platform deployment is saved in the root directory of your revision\. For more information, see [Add an AppSpec file for an EC2/On\-Premises deployment](application-revisions-appspec-file.md#add-appspec-file-server) and [Plan a revision for CodeDeploy](application-revisions-plan.md)\. 

## AppSpec File spacing<a name="reference-appspec-file-spacing"></a>

The following is the correct format for AppSpec file spacing\. The numbers in square brackets indicate the number of spaces that must occur between items\. For example, `[4]` means to insert four spaces between the items\. CodeDeploy raises an error that might be difficult to debug if the locations and number of spaces in an AppSpec file are not correct\.

```
version:[1]version-number
os:[1]operating-system-name
files:
[2]-[1]source:[1]source-files-location
[4]destination:[1]destination-files-location
permissions:
[2]-[1]object:[1]object-specification
[4]pattern:[1]pattern-specification
[4]except:[1]exception-specification
[4]owner:[1]owner-account-name
[4]group:[1]group-name
[4]mode:[1]mode-specification
[4]acls: 
[6]-[1]acls-specification 
[4]context:
[6]user:[1]user-specification
[6]type:[1]type-specification
[6]range:[1]range-specification
[4]type:
[6]-[1]object-type
hooks:
[2]deployment-lifecycle-event-name:
[4]-[1]location:[1]script-location
[6]timeout:[1]timeout-in-seconds
[6]runas:[1]user-name
```

Here is an example of a correctly spaced AppSpec file:

```
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html/WordPress
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
    - location: scripts/create_test_db.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
```

For more information about spacing, see the [YAML](http://www.yaml.org) specification\.
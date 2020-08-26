# Create an EC2/On\-Premises Compute Platform deployment \(CLI\)<a name="deployments-create-cli"></a>

To use the AWS CLI to deploy a revision to the EC2/On\-Premises compute platform:

1. After you have prepared the instances, created the application, and pushed the revision, do one of the following: 
   + If you want to deploy a revision from an Amazon S3 bucket, continue to step 2 now\.
   + If you want to deploy a revision from a GitHub repository, first complete the steps in [Connect a CodeDeploy application to a GitHub repository](deployments-create-cli-github.md), and then continue to step 2\. 

1. Call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command, specifying:
   + An application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
   + An Amazon EC2 deployment group name\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
   + Information about the revision to be deployed:

     For revisions stored in Amazon S3:
     + The Amazon S3 bucket name that contains the revision\.
     + The name and file type of the uploaded revision\.
**Note**  
The tar and compressed tar archive file formats \(\.tar and \.tar\.gz\) are not supported for Windows Server instances\.
     + \(Optional\) The Amazon S3 version identifier for the revision\. \(If the version identifier is not specified, CodeDeploy uses the most recent version\.\)
     + \(Optional\) The ETag for the revision\. \(If the ETag is not specified, CodeDeploy skips object validation\.\)

     For revisions stored in GitHub:
     + The GitHub user or group name assigned to the repository that contains the revision, followed by a forward slash \(`/`\), followed by the repository name\.
     + The commit ID for the revision\.
   + \(Optional\) The name of a deployment configuration to use\. To view a list of deployment configurations, call the [list\-deployment\-configs](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-configs.html) command\. \(If not specified, CodeDeploy uses a specific default deployment configuration\.\)
   + \(Optional\) Whether you want the deployment to an instance to continue to the `BeforeInstall` deployment lifecycle event if the `ApplicationStop` deployment lifecycle event fails\. 
   + \(Optional\) A description for the deployment\.
   + For blue/green deployments, information about the instances that belong to the replacement environment in a blue/green deployment, including the names of one or more Amazon EC2 Auto Scaling groups, or the tag filter key, type, and value used to identify Amazon EC2 instances\.

**Note**  
Use this syntax as part of the create\-deployment call to specify information about a revision in Amazon S3 directly on the command line\. \(The `version` and `eTag` are optional\.\)  

```
--s3-location bucket=string,key=string,bundleType=tar|tgz|zip,version=string,eTag=string
```
Use this syntax as part of the create\-deployment call to specify information about a revision in GitHub directly on the command line:  

```
--github-location repository=string,commitId=string
```
To get information about revisions that have been pushed already, call the [list\-application\-revisions](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-application-revisions.html) command\.

To track the status of your deployment, see [View CodeDeploydeployment details ](deployments-view-details.md)\.

**Topics**
+ [Connect a CodeDeploy application to a GitHub repository](deployments-create-cli-github.md)
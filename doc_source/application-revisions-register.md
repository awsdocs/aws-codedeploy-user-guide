# Register an application revision in Amazon S3 with CodeDeploy<a name="application-revisions-register"></a>

If you've already called the [push](https://docs.aws.amazon.com/cli/latest/reference/deploy/push.html) command to push an application revision to Amazon S3, you don't need to register the revision\. However, if you upload a revision to Amazon S3 through other means and want the revision to appear in the CodeDeploy console or through the AWS CLI, follow these steps to register the revision first\.

If you've pushed an application revision to a GitHub repository and want the revision to appear in the CodeDeploy console or through the AWS CLI, you must also follow these steps\.

You can use only the AWS CLI or the CodeDeploy APIs to register application revisions in Amazon S3 or GitHub\.

**Topics**
+ [Register a revision in Amazon S3 with CodeDeploy \(CLI\)](#application-revisions-register-s3)
+ [Register a revision in GitHub with CodeDeploy \(CLI\)](#application-revisions-register-github)

## Register a revision in Amazon S3 with CodeDeploy \(CLI\)<a name="application-revisions-register-s3"></a>

1. Upload the revision to Amazon S3\.

1. Call the [register\-application\-revision](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-application-revision.html) command, specifying:
   + The application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
   + Information about the revision to be registered:
     + The name of the Amazon S3 bucket that contains the revision\.
     + The name and file type of the uploaded revision\. For AWS Lambda deployments, the revision is an AppSpec file written in JSON or YAML\. For EC2/On\-Premises deployments, the revision contains a version of the source files that CodeDeploy will deploy to your instances or scripts that CodeDeploy will run on your instances\.
**Note**  
The tar and compressed tar archive file formats \(\.tar and \.tar\.gz\) are not supported for Windows Server instances\.
     + \(Optional\) The revision's Amazon S3 version identifier\. \(If the version identifier is not specified, CodeDeploy will use the most recent version\.\)
     + \(Optional\) The revision's ETag\. \(If the ETag is not specified, CodeDeploy will skip object validation\.\)
   + \(Optional\) Any description you want to associate with the revision\.

Information about a revision in Amazon S3 can be specified on the command line, using this syntax as part of the register\-application\-revision call\. \(`version` and `eTag` are optional\.\)

For a revision file for an EC2/On\-Premises deployment:

```
--s3-location bucket=string,key=string,bundleType=tar|tgz|zip,version=string,eTag=string
```

For a revision file for an AWS Lambda deployment:

```
--s3-location bucket=string,key=string,bundleType=JSON|YAML,version=string,eTag=string
```

## Register a revision in GitHub with CodeDeploy \(CLI\)<a name="application-revisions-register-github"></a>

**Note**  
AWS Lambda deployments do not work with GitHub\. 

1. Upload the revision to your GitHub repository\.

1. Call the [register\-application\-revision](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-application-revision.html) command, specifying:
   + The application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
   + Information about the revision to be registered:
     + The GitHub user or group name assigned to the repository that contains the revision, followed by a forward slash \(`/`\), followed by the repository name\.
     + The ID of the commit that references the revision in the repository\.
   + \(Optional\) Any description you want to associate with the revision\.

Information about a revision in GitHub can be specified on the command line, using this syntax as part of the register\-application\-revision call:

```
--github-location repository=string,commitId=string
```
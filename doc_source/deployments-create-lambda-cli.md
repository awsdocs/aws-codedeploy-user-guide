[Back to contents](index.md)

# Create an AWS Lambda Compute Platform Deployment \(CLI\)<a name="deployments-create-lambda-cli"></a>

After you have created the application and revision \(in AWS Lambda deployments, this is the AppSpec file\):

Call the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command, specifying:
+ An application name\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ A deployment group name\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
+ Information about the revision to be deployed:

  For revisions stored in Amazon S3:
  + The Amazon S3 bucket name that contains the revision\.
  + The name of the uploaded revision\.
  + \(Optional\) The Amazon S3 version identifier for the revision\. \(If the version identifier is not specified, CodeDeploy uses the most recent version\.\)
  + \(Optional\) The ETag for the revision\. \(If the ETag is not specified, CodeDeploy skips object validation\.\)

  For revisions stored in a file that is not in Amazon S3, you need the file name and its path\. Your revision file is written using JSON or YAML, so it most likely has a \.json or \.yaml extension\.
+ \(Optional\) The name of a deployment configuration to use\. To view a list of deployment configurations, call the [list\-deployment\-configs](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-configs.html) command\. \(If not specified, CodeDeploy uses a specific default deployment configuration\.\)
+ \(Optional\) A description for the deployment\.

The revision file can be specified as a file uploaded to an Amazon S3 bucket or as a string\. The syntax for each when used as part of the create\-deployment command is:
+ Amazon S3 bucket:

  The `version` and `eTag` are optional\.

  ```
  --s3-location bucket=string,key=string,bundleType=JSON|YAML,version=string,eTag=string
  ```
+ String:

  ```
  --revision '{"revisionType": "String", "string": {"content":"revision-as-string"}}'
  ```

**Note**  
The create\-deployment command can load a revision from a file\. For more information, see [Loading Parameters from a File](https://docs.aws.amazon.com/cli/latest/userguide/cli-using-param.html#cli-using-param-file)\. 

For AWS Lambda deployment revision templates, see [Add an AppSpec File for an AWS Lambda Deployment](application-revisions-appspec-file.md#add-appspec-file-lambda)\. For an example revision, see [ AppSpec File Example for an AWS Lambda Deployment ](reference-appspec-file-example.md#appspec-file-example-lambda)\.

To track the status of your deployment, see [View CodeDeployDeployment Details ](deployments-view-details.md)\.
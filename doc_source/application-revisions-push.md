# Push a revision for CodeDeploy to Amazon S3 \(EC2/On\-Premises deployments only\)<a name="application-revisions-push"></a>

After you plan your revision as described in [Plan a revision for CodeDeploy](application-revisions-plan.md) and add an AppSpec file to the revision as described in [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md), you are ready to bundle the component files and push the revision to Amazon S3\. For deployments to Amazon EC2 instances, after you push the revision, you can use CodeDeploy to deploy the revision from Amazon S3 to the instances\.

**Note**  
CodeDeploy can also be used to deploy revisions that have been pushed to GitHub\. For more information, see your GitHub documentation\.

We assume you have already followed the instructions in [Getting started with CodeDeploy](getting-started-codedeploy.md) to set up the AWS CLI\. This is especially important for calling the push command described later\.

Be sure you have an Amazon S3 bucket\. Follow the instructions in [Create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

If your deployment is to Amazon EC2 instances, then the target Amazon S3 bucket must be created or exist in the same region as the target instances\. For example, if you want to deploy a revision to some instances in the US East \(N\. Virginia\) Region and other instances in the US West \(Oregon\) Region, then you must have one bucket in the US East \(N\. Virginia\) Region with one copy of the revision and another bucket in the US West \(Oregon\) Region with another copy of the same revision\. In this scenario, you would then need to create two separate deployments, one in the US East \(N\. Virginia\) Region and another in the US West \(Oregon\) Region, even though the revision is the same in both regions and buckets\.

You must have permissions to upload to the Amazon S3 bucket\. You can specify these permissions through an Amazon S3 bucket policy\. For example, in the following Amazon S3 bucket policy, using the wildcard character \(\*\) allows AWS account `111122223333` to upload files to any directory in the Amazon S3 bucket named `codedeploydemobucket`:

```
{
    "Statement": [
        {
            "Action": [
                "s3:PutObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::codedeploydemobucket/*",
            "Principal": {
                "AWS": [
                    "111122223333"
                ]
            }
        }
    ]
}
```

To view your AWS account ID, see [Finding Your AWS Account ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html#FindingYourAWSId)\.

To learn how to generate and attach an Amazon S3 bucket policy, see [Bucket policy examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)\.

The IAM user who is calling the push command must have, at minimum, permissions to upload the revision to each target Amazon S3 bucket\. For example, the following policy allows the IAM user to upload revisions anywhere in the Amazon S3 bucket named `codedeploydemobucket`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::codedeploydemobucket/*"
        }
    ]
}
```

To learn how to create and attach an IAM policy, see [Working with policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html#AddingPermissions_Console)\.

## Push a revision using the AWS CLI<a name="push-with-cli"></a>

**Note**  
 The `push` command bundles application artifacts and an AppSpec file into a revision\. The file format of this revision is a compressed ZIP file\. The command cannot be used with an AWS Lambda or an Amazon ECS deployment because each expects a revision that is a JSON\-formatted or YAML\-formatted AppSpec file\. 

Call the push command to bundle and push the revision for a deployment\. Its parameters are:
+  \-\-application\-name: \(string\) Required\. The name of the CodeDeploy application to be associated with the application revision\. 
+  \-\-s3\-location: \(string\) Required\. Information about the location of the application revision to be uploaded to Amazon S3\. You must specify an Amazon S3 bucket and a key\. The key is the name of the revision\. CodeDeploy zips the content before it is uploaded\. Use the format `s3://your-S3-bucket-name/your-key.zip`\. 
+  \-\-ignore\-hidden\-files or \-\-no\-ignore\-hidden\-files: \(boolean\) Optional\. Use the `--no-ignore-hidden-files` flag \(the default\) to bundle and upload hidden files to Amazon S3\. Use the `--ignore-hidden-files` flag to not bundle and upload hidden files to Amazon S3\. 
+  \-\-source \(string\) Optional\. The location of the content to be deployed and the AppSpec file on the development machine to be zipped and uploaded to Amazon S3\. The location is specified as a path relative to the current directory\. If the relative path is not specified or if a single period is used for the path \("\."\), the current directory is used\. 
+  \-\-description \(string\) Optional\. A comment that summarizes the application revision\. If not specified, the default string "Uploaded by AWS CLI 'time' UTC" is used, where 'time' is the current system time in Coordinated Universal Time \(UTC\)\. 

You can use the AWS CLI to push a revision for an Amazon EC2 deployment\. An example push command looks like this: 

In Linux, macOS, or Unix:

```
aws deploy push \
  --application-name WordPress_App \
  --description "This is a revision for the application WordPress_App" \
  --ignore-hidden-files \
  --s3-location s3://codedeploydemobucket/WordPressApp.zip \
  --source .
```

 In Windows: 

```
aws deploy push --application-name WordPress_App --description "This is a revision for the application WordPress_App" --ignore-hidden-files --s3-location s3://codedeploydemobucket/WordPressApp.zip --source .
```

 This command does the following: 
+  Associates the bundled files with an application named `WordPress_App`\. 
+  Attaches a description to the revision\. 
+  Ignores hidden files\. 
+  Names the revision `WordPressApp.zip` and pushes it to a bucket named `codedeploydemobucket`\. 
+  Bundles all files in the root directory into the revision\. 

After the push is successful, you can use the AWS CLI or the CodeDeploy console to deploy the revision from Amazon S3\. To deploy this revision with the AWS CLI: 

 In Linux, macOS, or Unix: 

```
aws deploy create-deployment \
  --application-name WordPress_App \ 
  --deployment-config-name your-deployment-config-name \ 
  --deployment-group-name your-deployment-group-name \ 
  --s3-location bucket=codedeploydemobucket,key=WordPressApp.zip,bundleType=zip
```

 In Windows: 

```
aws deploy create-deployment --application-name WordPress_App --deployment-config-name your-deployment-config-name --your-deployment-group-name your-deployment-group-name --s3-location bucket=codedeploydemobucket,key=WordPressApp.zip,bundleType=zip
```

 For more information, see [Create a deployment with CodeDeploy](deployments-create.md)\. 
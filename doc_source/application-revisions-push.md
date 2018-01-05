# Push a Revision for AWS CodeDeploy to Amazon S3<a name="application-revisions-push"></a>

After you plan your revision as described in [Plan a Revision for AWS CodeDeploy](application-revisions-plan.md) and add an AppSpec file to the revision as described in [Add an Application Specification File to a Revision for AWS CodeDeploy](application-revisions-appspec-file.md), you are ready to bundle the component files and push the revision to Amazon S3\. For deployments to Amazon EC2 instances, after you push the revision, you can use AWS CodeDeploy to deploy the revision from Amazon S3 to the instances\.

**Note**  
AWS CodeDeploy can also be used to deploy revisions that have been pushed to GitHub\. For more information, see your GitHub documentation\.

We assume you have already followed the instructions in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md) to set up the AWS CLI\. This is especially important for calling the push command described later\.

Be sure you have an Amazon S3 bucket\. Follow the instructions in [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

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

To view your AWS account ID, see [Finding Your AWS Account ID](http://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html#FindingYourAWSId)\.

To learn how to generate and attach an Amazon S3 bucket policy, see [Bucket Policy Examples](http://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)\.

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

To learn how to create and attach an IAM policy, see [Working with Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html#AddingPermissions_Console)\.

## Push a Revision for an Amazon EC2 Deployment<a name="push-server-revision"></a>

To bundle and push the revision for a deployment to Amazon EC2 instances in a single command, from the command line, switch to the root directory \(folder\) of the revision, and then call the push command\.

For example, to bundle the component files into a revision starting from the current directory, associated with the application named `WordPress_App`, to an Amazon S3 bucket named `codedeploydemobucket`, with a revision name of `WordPressApp.zip`, call the push command as follows:

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

## Push a Revision for an Amazon EC2 Deployment<a name="push-lambda-revision"></a>

To push the revision, which, for an Lambda deployment is the same as the AppSpec file, call the push command as follows:

**Note**  
The AppSpec file in the following examples is a YAML file, but it can also be a JSON file\.

In Linux, macOS, or Unix:

```
aws deploy push \
  --application-name MyLambdaApplicationName \
  --description "This is a revision for the application MyLambdaApplicationName" \
  --ignore-hidden-files \
  --s3-location s3://codedeploydemobucket/MyAppSpecFile.yaml; \
  --source .
```

In Windows:

```
aws deploy push --application-name MyLambdaApplicationName --description "This is a revision for the application MyLambdaApplicationName" --ignore-hidden-files --s3-location s3://codedeploydemobucket/MyAppSpecFile.yaml --source .
```

After the push is successful, you can use the AWS CLI or the AWS CodeDeploy console to deploy the revision from Amazon S3\. For Amazon EC2 deployments, the revision is deployed to the instances\. For AWS Lambda deployments, traffic starts to be shifted to the new Lambda function version\. For instructions, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.
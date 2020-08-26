# Step 3: Upload a sample application to your GitHub repository<a name="tutorials-github-upload-sample-revision"></a>

In this step, you will copy a sample revision from a public Amazon S3 bucket to your GitHub repository\. \(For simplicity, the sample revisions provided for this tutorial are single web pages\.\)

**Note**  
If you use one of your revisions instead of our sample revision, your revision must:   
Follow the guidelines in [Plan a revision for CodeDeploy](application-revisions-plan.md) and [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md)\.
Work with the corresponding instance type\.
Be accessible from your GitHub dashboard\.
If your revision meets these requirements, skip ahead to [Step 5: Create an application and deployment group](tutorials-github-create-application.md)\.  
If you're deploying to an Ubuntu Server instance, you'll need to upload to your GitHub repository a revision compatible with an Ubuntu Server instance and CodeDeploy\. For more information, see [Plan a revision for CodeDeploy](application-revisions-plan.md) and [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md)\.

**Topics**
+ [Push a sample revision from a local Linux, macOS, or Unix machine](#tutorials-github-upload-sample-revision-unixes)
+ [Push a sample revision from a local Windows machine](#tutorials-github-upload-sample-revision-windows)

## Push a sample revision from a local Linux, macOS, or Unix machine<a name="tutorials-github-upload-sample-revision-unixes"></a>

With your terminal still open in, for example, the `/tmp/CodeDeployGitHubDemo` location, run the following commands one at a time: 

**Note**  
If you plan to deploy to a Windows Server instance, substitute `SampleApp_Windows.zip` for `SampleApp_Linux.zip` in the commands\.

```
(Amazon S3 copy command)
```

```
unzip SampleApp_Linux.zip
```

```
rm SampleApp_Linux.zip
```

```
git add .
```

```
git commit -m "Added sample app"
```

```
git push
```

Where *\(Amazon S3 copy command\)* is one of the following: 
+ `aws s3 cp s3://aws-codedeploy-us-east-2/samples/latest/SampleApp_Linux.zip . --region us-east-2` for the US East \(Ohio\) region
+ `aws s3 cp s3://aws-codedeploy-us-east-1/samples/latest/SampleApp_Linux.zip . --region us-east-1` for the US East \(N\. Virginia\) region
+ `aws s3 cp s3://aws-codedeploy-us-west-1/samples/latest/SampleApp_Linux.zip . --region us-west-1` for the US West \(N\. California\) Region
+ `aws s3 cp s3://aws-codedeploy-us-west-2/samples/latest/SampleApp_Linux.zip . --region us-west-2` for the US West \(Oregon\) region
+ `aws s3 cp s3://aws-codedeploy-ca-central-1/samples/latest/SampleApp_Linux.zip . --region ca-central-1` for the Canada \(Central\) Region
+ `aws s3 cp s3://aws-codedeploy-eu-west-1/samples/latest/SampleApp_Linux.zip . --region eu-west-1` for the Europe \(Ireland\) region 
+ `aws s3 cp s3://aws-codedeploy-eu-west-2/samples/latest/SampleApp_Linux.zip . --region eu-west-2` for the Europe \(London\) region 
+ `aws s3 cp s3://aws-codedeploy-eu-west-3/samples/latest/SampleApp_Linux.zip . --region eu-west-3` for the Europe \(Paris\) region 
+ `aws s3 cp s3://aws-codedeploy-eu-central-1/samples/latest/SampleApp_Linux.zip . --region eu-central-1` for the Europe \(Frankfurt\) Region
+ `aws s3 cp s3://aws-codedeploy-ap-east-1/samples/latest/SampleApp_Linux.zip . --region ap-east-1` for the Asia Pacific \(Hong Kong\) region
+ `aws s3 cp s3://aws-codedeploy-ap-northeast-1/samples/latest/SampleApp_Linux.zip . --region ap-northeast-1` for the Asia Pacific \(Tokyo\) region
+ `aws s3 cp s3://aws-codedeploy-ap-northeast-2/samples/latest/SampleApp_Linux.zip . --region ap-northeast-2` for the Asia Pacific \(Seoul\) region
+ `aws s3 cp s3://aws-codedeploy-ap-southeast-1/samples/latest/SampleApp_Linux.zip . --region ap-southeast-1` for the Asia Pacific \(Singapore\) Region
+ `aws s3 cp s3://aws-codedeploy-ap-southeast-2/samples/latest/SampleApp_Linux.zip . --region ap-southeast-2` for the Asia Pacific \(Sydney\) region
+ `aws s3 cp s3://aws-codedeploy-ap-south-1/samples/latest/SampleApp_Linux.zip . --region ap-south-1` for the Asia Pacific \(Mumbai\) region
+ `aws s3 cp s3://aws-codedeploy-sa-east-1/samples/latest/SampleApp_Linux.zip . --region sa-east-1` for the South America \(São Paulo\) Region

## Push a sample revision from a local Windows machine<a name="tutorials-github-upload-sample-revision-windows"></a>

 With your command prompt still open in, for example, the `c:\temp\CodeDeployGitHubDemo` location , run the following commands one at a time:

**Note**  
If you plan to deploy to an Amazon Linux or RHEL instance, substitute `SampleApp_Linux.zip` for `SampleApp_Windows.zip` in the commands\.

```
(Amazon S3 copy command)
```

Unzip the contents of `the` the ZIP file directly into the local directory \(for example `c:\temp\CodeDeployGitHubDemo`\), not into a new subdirectory\.

```
git add .
```

```
git commit -m "Added sample app"
```

```
git push
```

Where *\(Amazon S3 copy command\)* is one of the following: 
+ `aws s3 cp s3://aws-codedeploy-us-east-2/samples/latest/SampleApp_Windows.zip . --region us-east-2` for the US East \(Ohio\) region
+ `aws s3 cp s3://aws-codedeploy-us-east-1/samples/latest/SampleApp_Windows.zip . --region us-east-1` for the US East \(N\. Virginia\) region
+ `aws s3 cp s3://aws-codedeploy-us-west-1/samples/latest/SampleApp_Windows.zip . --region us-west-1` for the US West \(N\. California\) Region
+ `aws s3 cp s3://aws-codedeploy-us-west-2/samples/latest/SampleApp_Windows.zip . --region us-west-2` for the US West \(Oregon\) region
+ `aws s3 cp s3://aws-codedeploy-ca-central-1/samples/latest/SampleApp_Windows.zip . --region ca-central-1` for the Canada \(Central\) Region
+ `aws s3 cp s3://aws-codedeploy-eu-west-1/samples/latest/SampleApp_Windows.zip . --region eu-west-1` for the Europe \(Ireland\) region
+ `aws s3 cp s3://aws-codedeploy-eu-west-2/samples/latest/SampleApp_Windows.zip . --region eu-west-2` for the Europe \(London\) region
+ `aws s3 cp s3://aws-codedeploy-eu-west-3/samples/latest/SampleApp_Windows.zip . --region eu-west-3` for the Europe \(Paris\) region
+ `aws s3 cp s3://aws-codedeploy-eu-central-1/samples/latest/SampleApp_Windows.zip . --region eu-central-1` for the Europe \(Frankfurt\) Region
+ `aws s3 cp s3://aws-codedeploy-ap-east-1/samples/latest/SampleApp_Windows.zip . --region ap-east-1` for the Asia Pacific \(Hong Kong\) region
+ `aws s3 cp s3://aws-codedeploy-ap-northeast-1/samples/latest/SampleApp_Windows.zip . --region ap-northeast-1` for the Asia Pacific \(Tokyo\) region
+ `aws s3 cp s3://aws-codedeploy-ap-northeast-2/samples/latest/SampleApp_Windows.zip . --region ap-northeast-2` for the Asia Pacific \(Seoul\) region
+ `aws s3 cp s3://aws-codedeploy-ap-southeast-1/samples/latest/SampleApp_Windows.zip . --region ap-southeast-1` for the Asia Pacific \(Singapore\) Region
+ `aws s3 cp s3://aws-codedeploy-ap-southeast-2/samples/latest/SampleApp_Windows.zip . --region ap-southeast-2` for the Asia Pacific \(Sydney\) region
+ `aws s3 cp s3://aws-codedeploy-ap-south-1/samples/latest/SampleApp_Windows.zip . --region ap-south-1` for the Asia Pacific \(Mumbai\) region
+ `aws s3 cp s3://aws-codedeploy-sa-east-1/samples/latest/SampleApp_Windows.zip . --region sa-east-1` for the South America \(São Paulo\) Region

To push your own revision to an Ubuntu Server instance, copy your revision into your local repo, and then call the following:

```
git add .
git commit -m "Added Ubuntu app"
git push
```
# CodeDeploy resource kit reference<a name="resource-kit"></a>

Many of the files CodeDeploy relies on are stored in publicly available, AWS region\-specific Amazon S3 buckets\. These files include installation files for the CodeDeploy agent, templates, and sample application files\. We call this collection of files the CodeDeploy Resource Kit\. 

**Topics**
+ [Resource kit bucket names by Region](#resource-kit-bucket-names)
+ [Resource kit contents](#resource-kit-file-list)
+ [Display a list of the resource kit files](#resource-kit-list-files)
+ [Download the resource kit files](#resource-kit-download-file)

## Resource kit bucket names by Region<a name="resource-kit-bucket-names"></a>

This table lists the names of *bucket\-name * replacements required for some procedures in the guide\. These are the names of the Amazon S3 buckets that contain the CodeDeploy Resource Kit files\.

**Note**  
 To access the Amazon S3 bucket in the Asia Pacific \(Hong Kong\) Region, you must enable the region in your AWS account\. For more information, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html#rande-manage-enable)\. 


****  

| Region name | *Bucket\-name* replacement | Region identifier | 
| --- | --- | --- | 
| US East \(Ohio\) | aws\-codedeploy\-us\-east\-2 | us\-east\-2 | 
| US East \(N\. Virginia\) | aws\-codedeploy\-us\-east\-1 | us\-east\-1 | 
| US West \(N\. California\) | aws\-codedeploy\-us\-west\-1 | us\-west\-1 | 
| US West \(Oregon\) | aws\-codedeploy\-us\-west\-2 | us\-west\-2 | 
| Canada \(Central\) | aws\-codedeploy\-ca\-central\-1 | ca\-central\-1 | 
| Europe \(Ireland\) | aws\-codedeploy\-eu\-west\-1 | eu\-west\-1 | 
| Europe \(London\) | aws\-codedeploy\-eu\-west\-2 | eu\-west\-2 | 
| Europe \(Paris\) | aws\-codedeploy\-eu\-west\-3 | eu\-west\-3 | 
| Europe \(Frankfurt\) | aws\-codedeploy\-eu\-central\-1 | eu\-central\-1 | 
| Europe \(Stockholm\) | aws\-codedeploy\-eu\-north\-1 | eu\-north\-1 | 
| Europe \(Milan\) | aws\-codedeploy\-eu\-south\-1 | eu\-south\-1 | 
| Asia Pacific \(Hong Kong\) | aws\-codedeploy\-ap\-east\-1 | ap\-east\-1 | 
| Asia Pacific \(Tokyo\) | aws\-codedeploy\-ap\-northeast\-1 | ap\-northeast\-1 | 
| Asia Pacific \(Seoul\) | aws\-codedeploy\-ap\-northeast\-2 | ap\-northeast\-2 | 
| Asia Pacific \(Singapore\) | aws\-codedeploy\-ap\-southeast\-1 | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | aws\-codedeploy\-ap\-southeast\-2 | ap\-southeast\-2 | 
| Asia Pacific \(Mumbai\) | aws\-codedeploy\-ap\-south\-1 | ap\-south\-1 | 
| South America \(São Paulo\) | aws\-codedeploy\-sa\-east\-1 | sa\-east\-1 | 
| Middle East \(Bahrain\) | aws\-codedeploy\-me\-south\-1 | me\-south\-1 | 
| Africa \(Cape Town\) | aws\-codedeploy\-af\-south\-1 | af\-south\-1 | 
| AWS GovCloud \(US\-East\) | aws\-codedeploy\-us\-gov\-east\-1 | us\-gov\-east\-1 | 
| AWS GovCloud \(US\-West\) | aws\-codedeploy\-us\-gov\-west\-1 | us\-gov\-west\-1 | 

## Resource kit contents<a name="resource-kit-file-list"></a>

The following table lists the files in the CodeDeploy Resource Kit\.


| File | Description | 
| --- | --- | 
| LATEST\_VERSION | A file used by update mechanisms like Amazon EC2 Systems Manager to determine the latest version of the CodeDeploy agent\. | 
| VERSION | The auto\-update mechanism was removed in CodeDeploy agent version 1\.1\.0 and this file is no longer used\. A file used by CodeDeploy agents to update themselves as they are running on instances\. | 
| codedeploy\-agent\.noarch\.rpm | The CodeDeploy agent for Amazon Linux and Red Hat Enterprise Linux \(RHEL\)\. There may be several files with the same base file name, but different versions \(such as \-1\.0\-0\)\. | 
| codedeploy\-agent\_all\.deb | The CodeDeploy agent for Ubuntu Server\. There may be several files with the same base file name, but different versions \(such as \_1\.0\-0\)\. | 
| codedeploy\-agent\.msi | The CodeDeploy agent for Windows Server\. There may be several files with the same base file name, but different versions \(such as \-1\.0\-0\)\. | 
| install | A file you can use to more easily install the CodeDeploy agent\. | 
|  `CodeDeploy_SampleCF_Template.json`  |  An AWS CloudFormation template you can use to launch from one to three Amazon EC2 instances running Amazon Linux or Windows Server\. There may be several files with the same base file name, but different versions \(such as `-1.0.0`\)\.  | 
| CodeDeploy\_SampleCF\_ELB\_Integration\.json | An AWS CloudFormation template you can use to create a load\-balanced sample Web site running on an Apache Web Server\. The application is configured to span all Availability Zones in the region you create it in\. This template creates three Amazon EC2 instances and IAM instance profile to grant the instances access to the resources in Amazon S3, Amazon EC2 Auto Scaling, AWS CloudFormation, and Elastic Load Balancing\. It also creates the load balancer and a CodeDeploy service role\. | 
| SampleApp\_ELB\_Integration\.zip | A sample application revision you can deploy to an Amazon EC2 instance that is registered to an Elastic Load Balancing load balancer\. | 
| SampleApp\_Linux\.zip |  A sample application revision you can deploy to an Amazon EC2 instance running Amazon Linux or to a Ubuntu Server or RHEL instance\. There may be several files with the same base file name, but different versions \(such as `-1.0`\)\.  | 
| SampleApp\_Windows\.zip | A sample application revision you can deploy to a Windows Server instance\. There may be several files with the same base file name, but different versions \(such as \-1\.0\)\. | 

## Display a list of the resource kit files<a name="resource-kit-list-files"></a>

To view a list of files, use the aws s3 ls command for your region\.

**Note**  
The files in each bucket are designed to work with resources in the corresponding region\.
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-us-east-2 --region us-east-2
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-us-east-1 --region us-east-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-us-west-1 --region us-west-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-us-west-2 --region us-west-2
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ca-central-1 --region ca-central-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-eu-west-1 --region eu-west-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-eu-west-2 --region eu-west-2
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-eu-west-3 --region eu-west-3
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-eu-central-1 --region eu-central-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-east-1 --region ap-east-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-northeast-1 --region ap-northeast-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-northeast-2 --region ap-northeast-2
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-southeast-1 --region ap-southeast-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-southeast-2 --region ap-southeast-2
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-ap-south-1 --region ap-south-1
  ```
+ 

  ```
  aws s3 ls --recursive s3://aws-codedeploy-sa-east-1 --region sa-east-1
  ```

## Download the resource kit files<a name="resource-kit-download-file"></a>

To download a file, use the aws s3 cp command for your region\. 

**Note**  
Be sure to use the period \(`.`\) near the end\. This downloads the file to your current directory\.

For example, the following commands download a single file named `SampleApp_Linux.zip` from one of the buckets' `/samples/latest/` folders:
+ 

  ```
  aws s3 cp s3://aws-codedeploy-us-east-2/samples/latest/SampleApp_Linux.zip . --region us-east-2
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-us-east-1/samples/latest/SampleApp_Linux.zip . --region us-east-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-us-west-1/samples/latest/SampleApp_Linux.zip . --region us-west-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-us-west-2/samples/latest/SampleApp_Linux.zip . --region us-west-2
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ca-central-1/samples/latest/SampleApp_Linux.zip . --region ca-central-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-eu-west-1/samples/latest/SampleApp_Linux.zip . --region eu-west-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-eu-west-2/samples/latest/SampleApp_Linux.zip . --region eu-west-2
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-eu-west-3/samples/latest/SampleApp_Linux.zip . --region eu-west-3
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-eu-central-1/samples/latest/SampleApp_Linux.zip . --region eu-central-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-east-1/samples/latest/SampleApp_Linux.zip . --region ap-east-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-northeast-1/samples/latest/SampleApp_Linux.zip . --region ap-northeast-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-northeast-2/samples/latest/SampleApp_Linux.zip . --region ap-northeast-2
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-southeast-1/samples/latest/SampleApp_Linux.zip . --region ap-southeast-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-southeast-2/samples/latest/SampleApp_Linux.zip . --region ap-southeast-2
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-ap-south-1/samples/latest/SampleApp_Linux.zip . --region ap-south-1
  ```
+ 

  ```
  aws s3 cp s3://aws-codedeploy-sa-east-1/samples/latest/SampleApp_Linux.zip . --region sa-east-1
  ```

To download all of the files, use one of the following commands for your region:
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-us-east-2 . --region us-east-2
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-us-east-1 . --region us-east-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-us-west-1 . --region us-west-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-us-west-2 . --region us-west-2
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ca-central-1 . --region ca-central-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-eu-west-1 . --region eu-west-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-eu-west-2 . --region eu-west-2
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-eu-west-3 . --region eu-west-3
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-eu-central-1 . --region eu-central-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-east-1 . --region ap-east-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-northeast-1 . --region ap-northeast-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-northeast-2 . --region ap-northeast-2
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-southeast-1 . --region ap-southeast-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-southeast-2 . --region ap-southeast-2
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-ap-south-1 . --region ap-south-1
  ```
+ 

  ```
  aws s3 cp --recursive s3://aws-codedeploy-sa-east-1 . --region sa-east-1
  ```
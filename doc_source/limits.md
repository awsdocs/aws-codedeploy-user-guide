# AWS CodeDeploy Limits<a name="limits"></a>

The following tables describe limits in AWS CodeDeploy\.

**Note**  
You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create%3FissueType=service-limit-increase) for the AWS CodeDeploy limits listed in [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_codedeploy) in the *Amazon Web Services General Reference* \. You cannot increase the limit on the number of hours a deployment can run\.

**Topics**
+ [Applications](#limits-applications)
+ [Application Revisions](#limits-revisions)
+ [Deployments](#limits-deployments)
+ [Deployment Configurations](#limits-deployment-configurations)
+ [Deployment Groups](#limits-deployment-groups)
+ [Instances](#limits-instances)

## Applications<a name="limits-applications"></a>


|  |  | 
| --- |--- |
|  Maximum number of applications associated with an AWS account in a single region  |  100  | 
|  Maximum number of characters in an application name  |  100  | 
| Characters allowed in an application name |  Letters \(a\-z, A\-Z\), numbers \(0\-9\), periods \(\.\), underscores \(\_\), `+` \(plus signs\), `=` \(equals signs\), `,` \(commas\), `@` \(at signs\), `-` \(minus signs\)\.  | 
| Maximum number of applications that can be passed to the [BatchGetApplications](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_BatchGetApplications.html) API action | 100 | 
| Maximum number of GitHub connection tokens for a single AWS account | 25 | 

## Application Revisions<a name="limits-revisions"></a>


|  |  | 
| --- |--- |
|  Maximum number of characters in an application revision name  |  100  | 
|  Allowed file types for application revisions  |  Archive files with the extension `.zip` or `.tar` and compressed archive files with the extension `.tar.gz`\. An archive or compressed archive file that is compatible with AWS CodeDeploy must contain a single application specification file \(AppSpec file\) with the file name `appspec.yml`\.  | 

## Deployments<a name="limits-deployments"></a>


|  |  | 
| --- |--- |
|  Maximum number of concurrent deployments to a deployment group¹  |  1  | 
| Maximum number of concurrent deployments associated with an AWS account²  | 100 | 
|  Maximum number of hours an EC2/On\-Premises in\-place deployment can run  |  8  | 
| Maximum number of hours between the deployment of a revision and the shifting of traffic to the replacement environment during an EC2/On\-Premises blue/green deployment | 48 | 
| Maximum number of hours between the completion of a deployment and the termination of the original environment during an EC2/On\-Premises blue/green deployment | 48 | 
| Maximum number of hours an EC2/On\-Premises blue/green deployment can run | 109 \(48 for each of the above two limits\) plus one hour for each of 13 possible lifecycle events | 
| Maximum number of hours an AWS Lambda deployment can run³ | 50 \(48 hours for the maximum time between the first and last traffic shift plus one hour for each of two possible lifecycle hooks\) | 
| Maximum number of seconds until a deployment lifecycle event fails if not completed | 3600 | 
| Maximum number of characters in a deployment description | 256 | 
| Maximum number of deployments that can be passed to the [BatchGetDeployments](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_BatchGetDeployments.html) API action | 100 | 
|  Maximum number of minutes until a deployment fails if a lifecycle event doesn't start after: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/limits.html)  | 5 | 
| Maximum number of minutes a blue/green deployment can wait after a successful deployment before terminating instances from the original deployment | 2800 | 
|  ¹ This limit is intended to prevent accidental, concurrent deployments of the same application to the same deployment group\. ² Each deployment to a scaled\-up Amazon EC2 instance in an Auto Scaling group counts as a single concurrent deployment\. If the scaled\-up Amazon EC2 instance is associated with multiple applications, then additional concurrent deployment for each application would be generated\. For example, an Auto Scaling group that scales up by five Amazon EC2 instances and is associated with a single application would generate five concurrent deployments\. If the same five scaled\-up Amazon EC2 instances are associated with two additional applications, this would generate ten additional concurrent deployments\.   | 

## Deployment Configurations<a name="limits-deployment-configurations"></a>


|  |  | 
| --- |--- |
|  Maximum number of custom deployment configurations associated with an AWS account  |  25  | 
| Allowed values for a minimum healthy instances setting of HOST\_COUNT | Any positive integer or 0 \(zero\)\. Zero \(0\) results in deployment to all instances at once\. | 
| Allowed values for a minimum healthy instances setting of FLEET\_PERCENT | Any positive integer less than 100 or 0 \(zero\)\. Zero \(0\) results in deployment to all instances at once\. | 
|  Maximum number of characters in a custom deployment configuration name  |  100  | 
| Characters allowed in a custom deployment configuration name |  Letters \(a\-z, A\-Z\), numbers \(0\-9\), periods \(\.\), underscores \(\_\), `+` \(plus signs\), `=` \(equals signs\), `,` \(commas\), `@` \(at signs\), `-` \(minus signs\)\.  | 
| Disallowed prefixes in a custom deployment configuration name | CodeDeployDefault\. | 
| Maximum number of minutes between the first and last traffic shift during an AWS Lambda canary or linear deployment | 2880 | 
| Maximum percentage of traffic that can be shifted in one increment during an AWS Lambda deployment | 99 | 

## Deployment Groups<a name="limits-deployment-groups"></a>


|  |  | 
| --- |--- |
|  Maximum number of deployment groups associated with a single application  |  100  | 
|  Maximum number of tags in a deployment group  |  10  | 
|  Maximum number of Auto Scaling groups in a deployment group  |  10  | 
| Maximum number of characters in a deployment group name  | 100 | 
| Characters allowed in a deployment group name | Letters \(a\-z, A\-Z\), numbers \(0\-9\), periods \(\.\), underscores \(\_\), \+ \(plus signs\), = \(equals signs\), , \(commas\), @ \(at signs\), \- \(minus signs\)\. | 
| Maximum number of event notification triggers in a deployment group | 10 | 

## Instances<a name="limits-instances"></a>


|  |  | 
| --- |--- |
|  Maximum number of instances in a single deployment  |  500  | 
| Maximum number of characters in a tag key | 128 | 
|  Maximum number of characters in a tag value  |  256  | 
| Maximum number of instances that can be passed to the [BatchGetOnPremisesInstances](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_BatchGetOnPremisesInstances.html) API action | 100 | 
| Maximum number of instances that can be used by concurrent deployments that are in progress and associated with one account  | 500 | 
|  Required version of AWS SDK for Ruby \(aws\-sdk\-core\)  |  **2\.1\.2 **or earlier for AWS CodeDeploy agent versions earlier than 1\.0\.1\.880\. **2\.2** or earlier for AWS CodeDeploy agent version 1\.0\.1\.880 and later\.  | 
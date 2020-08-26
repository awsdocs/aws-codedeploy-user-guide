# Choose a CodeDeploy repository type<a name="application-revisions-repository-type"></a>

The storage location for files required by CodeDeploy is called a *repository*\. Use of a repository depends on which compute platform your deployment uses\.
+ **EC2/On\-Premises**: To deploy your application code to one or more instances, your code must be bundled into an archive file and placed in a repository where CodeDeploy can access it during the deployment process\. You bundle your deployable content and an AppSpec file into an archive file, and then upload it to one of the repository types supported by CodeDeploy\.
+ **AWS Lambda** and **Amazon ECS**: Deployments require an AppSpec file, which can be accessed during a deployment in one of the following ways: 
  +  From an Amazon S3 bucket\. 
  +  From text typed directly into the AppSpec editor in the console\. For more information, see [Create an AWS Lambda Compute Platform deployment \(console\)](deployments-create-console-lambda.md) and [ Create an Amazon ECS Compute Platform deployment \(console\) ](deployments-create-console-ecs.md)\. 
  +  If you use the AWS CLI, you can reference an AppSpec file that is on your hard drive or on a network drive\. For more information, see [ Create an AWS Lambda Compute Platform deployment \(CLI\) ](deployments-create-lambda-cli.md) and [ Create an Amazon ECS Compute Platform deployment \(CLI\) ](deployments-create-ecs-cli.md)\. 

CodeDeploy currently supports the following repository types: 


|  |  |  | 
| --- |--- |--- |
| Repository Type | Repository Details | Supported Compute Platform | 
| Amazon S3 | [Amazon Simple Storage Service](https://docs.aws.amazon.com/AmazonS3/latest/gsg/) \(Amazon S3\) is the AWS solution for secure, scalable object storage\. Amazon S3 stores data as objects in buckets\. An object consists of a file and, optionally, any metadata that describes that file\. To store an object in Amazon S3, you upload the file to a bucket\. When you upload a file, you can set permissions and metadata on the object\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html) | Deployments that use the following compute platforms can store the revision in an Amazon S3 bucket\.[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html) | 
| GitHub | You can store your application revisions in [GitHub](http://www.github.com) repositories\. You can trigger a deployment from a GitHub repository whenever the source code in that repository is changed\.Learn more:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html) | Only EC2/On\-Premises deployments can store the revision in a GitHub repository\. | 
| Bitbucket |  You can deploy code to deployment groups of EC2 instances by using the [CodeDeploy pipe](https://bitbucket.org/product/features/pipelines/integrations?p=atlassian/aws-code-deploy) in [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines)\. Bitbucket Pipelines offers continuous integration and continuous deployment \(CI/CD\) features, including [Bitbucket Deployments](https://confluence.atlassian.com/bitbucket/bitbucket-deployments-940695276.html)\. The CodeDeploy pipe first pushes the artifact to an S3 bucket that you have specified, and then deploys the code artifact from the bucket\. Learn more:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html)  | Only EC2/On\-Premises deployments can store the revision in a BitBucket repository\. | 

**Note**  
An AWS Lambda deployment works with an Amazon S3 repository only\.
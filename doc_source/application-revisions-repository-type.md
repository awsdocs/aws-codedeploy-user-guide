--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Choose an AWS CodeDeploy Repository Type<a name="application-revisions-repository-type"></a>

The storage location for files required by AWS CodeDeploy is called a *repository*\. Use of a repository depends on which compute platform your deployment uses\.
+ **EC2/On\-Premises**: To deploy your application code to one or more instances, your code must be bundled into an archive file and placed in a repository where AWS CodeDeploy can access it during the deployment process\. You bundle your deployable content and an AppSpec file into an archive file, and then upload it to one of the repository types supported by AWS CodeDeploy\.
+ **AWS Lambda**: Deployments require an AppSpec file, which can be accessed during a deployment in one of the following ways: 
  +  From an Amazon S3 bucket\. 
  +  From text typed directly into the AppSpec editor in the console\. For more information, see [Create an AWS Lambda Compute Platform Deployment \(Console\)](deployments-create-console-lambda.md)\. 
  +  If you use the AWS CLI, you can reference an AppSpec file that is on your hard drive or on a network drive\. For more information, see [ Create an AWS Lambda Compute Platform Deployment \(CLI\) ](deployments-create-lambda-cli.md)\. 

AWS CodeDeploy currently supports the following repository types: 


|  |  | 
| --- |--- |
| Amazon S3 | [Amazon Simple Storage Service](https://docs.aws.amazon.com/AmazonS3/latest/gsg/) \(Amazon S3\) is the AWS solution for secure, scalable object storage\. Amazon S3 stores data as objects in buckets\. An object consists of a file and, optionally, any metadata that describes that file\. To store an object in Amazon S3, you upload the file you want to store to a bucket\. When you upload a file, you can set permissions and metadata on the object\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html) | 
| GitHub | \(EC2/On\-Premises deployment only\) You can store your application revisions in [GitHub](http://www.github.com) repositories\. You can trigger a deployment from a GitHub repository whenever the source code in that repository is changed\.Learn more:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html) | 
| Bitbucket |  \(EC2/On\-Premises deployment only\) You can push code to Amazon EC2 instances directly from the Bitbucket UI to any of your deployment groups without having to sign in to your continuous integration \(CI\) platform or Amazon EC2 instances to run a manual deployment process\. Bitbucket first pushes the code to an Amazon S3 bucket you have specified, and from there deploys the code\. After the initial setup to support this process is complete, however, the code you push from Bitbucket is automatically deployed to your instances without any intermediate steps\. Learn more:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions-repository-type.html)  | 

**Note**  
An AWS Lambda deployment works with an Amazon S3 repository only\.
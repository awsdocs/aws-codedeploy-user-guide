# CodeDeploy updates to AWS managed policies<a name="managed-policies-updates"></a>

View details about updates to AWS managed policies for CodeDeploy since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CodeDeploy [Document history](document-history.md)\. 


| Change | Description | Date | 
| --- | --- | --- | 
|  `AmazonEC2RoleforAWSCodeDeployLimited` managed policy – Updates to existing policy  |  Removed the `s3:ListBucket` action from the policy statement that includes the `s3:ExistingObjectTag/UseWithCodeDeploy` condition\. For more information on this policy, see [AmazonEC2RoleforAWSCodeDeployLimited](managed-policies.md#EC2-policy)\.  |  November 22, 2021  | 
|  `AWSCodeDeployRole` managed policy – Updates to existing policy  |  Added the `autoscaling:PutWarmPool` action to support [adding warm pools to Amazon EC2 Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-warm-pools.html#add-warm-pool-console/ec2/userguide/ec2-auto-scaling-warm-pools.html#add-warm-pool-console) for blue/green deployments\. Removed needless duplicate actions\.  |  May 18, 2021  | 
|  CodeDeploy started tracking changes  |  CodeDeploy started tracking changes for its AWS managed policies\.  |  May 18, 2021  | 
# Data protection in AWS CodeDeploy<a name="data-protection"></a>

AWS CodeDeploy conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls in AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with CodeDeploy or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into CodeDeploy or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

## Internetwork traffic privacy<a name="inter-network-traffic-privacy"></a>

CodeDeploy is a fully managed deployment service that supports EC2 instances, Lambda functions, Amazon ECS, and on\-premises servers\. For EC2 instances and on\-premises servers, a host\-based agent communicates with CodeDeploy using TLS\.

Currently, communication from the agent to the service requires an outbound internet connection so the agent can communicate with the public CodeDeploy and Amazon S3 service endpoints\. In a virtual private cloud, this can be accomplished with an internet gateway, site to site VPN connection to your corporate network, or direct connect\.

The CodeDeploy agent supports HTTP proxies\.

AWS PrivateLink for CodeDeploy is not currently available\. If this is a feature you are interested in, ask your account team to open a product feature request on your behalf\.

**Note**  
The CodeDeploy agent is required only if you deploy to an Amazon EC2/On\-premises compute platform\. The agent is not required for deployments that use the Amazon ECS or AWS Lambda compute platform\.

## Encryption at rest<a name="encryption-at-rest"></a>

Customer code is not stored in CodeDeploy\. As a deployment service, CodeDeploy is dispatching commands to the CodeDeploy agent running on EC2 instances or on\-premises servers\. The CodeDeploy agent then executes the commands using TLS\. Service model data for deployments, deployment configuration, deployment groups, applications, and application revisions are stored in Amazon DynamoDB and encrypted at rest using an AWS KMS key managed by Amazon DynamoDB\. For more information, see [Amazon DynamoDB encryption at rest](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/EncryptionAtRest.html)\.

## Encryption in transit<a name="encryption-in-transit"></a>

The CodeDeploy agent initiates all communication with CodeDeploy over port 443\. The agent polls CodeDeploy and listens for a command\. The CodeDeploy agent is open source\. All service\-to\-service and client\-to\-service communication is encrypted in transit using TLS\. This protects customer data in transit between CodeDeploy and other services such as Amazon S3\.

## Encryption key management<a name="key-management"></a>

There are no encryption keys for customers to manage\. The CodeDeploy service model data is encrypted using an AWS KMS key managed by Amazon DynamoDB\.
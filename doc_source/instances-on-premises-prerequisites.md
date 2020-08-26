# Prerequisites for configuring an on\-premises instance<a name="instances-on-premises-prerequisites"></a>

The following prerequisites must be met before you can register an on\-premises instance\.

**Important**  
If you are using the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command and periodically refreshed temporary credentials generated with the AWS Security Token Service \(AWS STS\), there are other prerequisites\. For information, see [IAM session ARN registration prerequisites](register-on-premises-instance-iam-session-arn.md#register-on-premises-instance-iam-session-arn-prerequisites)\.

**Device requirements**

The device you want to prepare, register, and tag as an on\-premises instance with CodeDeploy must be running a supported operating system\. For a list, see [Operating systems supported by the CodeDeploy agent](codedeploy-agent.md#codedeploy-agent-supported-operating-systems)\.

If your operating system is not supported, the CodeDeploy agent is available as open source for you to adapt to your needs\. For more information, see the [CodeDeploy agent](https://github.com/aws/aws-codedeploy-agent) repository in GitHub\.

**Outbound communication**

The on\-premises instance must be able to connect to public AWS service endpoints to communicate with CodeDeploy\.

The CodeDeploy agent communicates outbound using HTTPS over port 443\.

**Administrative control**

The local or network account used on the on\-premises instance to configure the on\-premises instance must be able to run either as `sudo` or `root` \(for Ubuntu Server\) or as an administrator \(for Windows Server\)\.

**IAM permissions**

The IAM identity you use to register the on\-premises instance must be granted permissions to complete the registration \(and to deregister the on\-premises instance, as needed\)\. 

In addition to the policy described in [Getting started with CodeDeploy](getting-started-codedeploy.md), make sure the calling IAM identity also has the following additional policy attached\. For information on how to attach IAM policies, see [Managing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow", 
      "Action": [
        "iam:CreateAccessKey",
        "iam:CreateUser",
        "iam:DeleteAccessKey",
        "iam:DeleteUser",
        "iam:DeleteUserPolicy",
        "iam:ListAccessKeys",
        "iam:ListUserPolicies",
        "iam:PutUserPolicy",
        "iam:GetUser"
      ],
      "Resource": "*"
    }
  ]
}
```
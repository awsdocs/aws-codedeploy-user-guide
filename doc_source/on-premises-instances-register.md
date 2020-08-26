# Register an on\-premises instance with CodeDeploy<a name="on-premises-instances-register"></a>

To register an on\-premises instance, you must use an IAM identity to authenticate your requests\. You can choose from the following options for the IAM identity and registration method you use:
+ Use an IAM user ARN to authenticate requests\.
  + Use the [register](https://docs.aws.amazon.com/cli/latest/reference/deploy/register.html) command for the most automated registration process\. Best for registering a single on\-premises instance\. For information, see [Use the register command \(IAM user ARN\) to register an on\-premises instance](instances-on-premises-register-instance.md)\. 
  + Use the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command to manually configure most registration options\. Suitable for registering a small number of on\-premises instances\. For information, see [Use the register\-on\-premises\-instance command \(IAM user ARN\) to register an on\-premises instance](register-on-premises-instance-iam-user-arn.md)\. 
+ Use an IAM role ARN to authenticate requests\. 
  + Use the [register\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/register-on-premises-instance.html) command and periodically refreshed temporary credentials generated with the AWS Security Token Service \(AWS STS\) to manually configure most registration options\. Best for registering a large number of on\-premises instances\. For information, see [Use the register\-on\-premises\-instance command \(IAM Session ARN\) to register an on\-premises instance ](register-on-premises-instance-iam-session-arn.md)\.

**Topics**
+ [Use the register command \(IAM user ARN\) to register an on\-premises instance](instances-on-premises-register-instance.md)
+ [Use the register\-on\-premises\-instance command \(IAM user ARN\) to register an on\-premises instance](register-on-premises-instance-iam-user-arn.md)
+ [Use the register\-on\-premises\-instance command \(IAM Session ARN\) to register an on\-premises instance](register-on-premises-instance-iam-session-arn.md)
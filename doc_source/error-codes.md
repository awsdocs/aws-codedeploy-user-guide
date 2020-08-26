# Error codes for AWS CodeDeploy<a name="error-codes"></a>

This topic provides reference information about CodeDeploy errors\.


****  

| Error Code | Description | 
| --- | --- | 
| `AGENT_ISSUE` |  The deployment failed because of a problem with the CodeDeploy agent\. Make sure the agent is installed and running on all instances in this deployment group\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `AUTO_SCALING_IAM_ROLE_PERMISSIONS` |  The service role associated with your deployment group does not have the permission required to perform operations in the following AWS service\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `HEALTH_CONSTRAINTS` |  The overall deployment failed because too many individual instances failed deployment, too few healthy instances are available for deployment, or some instances in your deployment group are experiencing problems\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `HEALTH_CONSTRAINTS_INVALID` |  The deployment can’t start because the minimum number of healthy instances, as defined by your deployment configuration, are not available\. You can reduce the required number of healthy instances by updating your deployment configuration or increase the number of instances in this deployment group\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `IAM_ROLE_MISSING` |  The deployment failed because no service role exists with the service role name specified for the deployment group\. Make sure you are using the correct service role name\.  Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `IAM_ROLE_PERMISSIONS` |  CodeDeploy does not have the permissions required to assume a role, or the IAM role you're using does't give you permission to perform operations in an AWS service\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `NO_INSTANCES` |   This might be caused by one of the following\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html) Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `OVER_MAX_INSTANCES` |  The deployment failed because more instances are targeted for deployment than are allowed for your account\. To reduce the number of instances targeted for this deployment, update the tag settings for this deployment group or delete some of the targeted instances\. Alternatively, you can contact AWS Support to request a limit increase\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `THROTTLED` |  The deployment failed because more requests were made than are permitted for AWS CodeDeploy by an IAM role\. Try reducing the number of requests\. Learn more:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 
| `UNABLE_TO_SEND_ASG` |  The deployment failed because the deployment group isn’t configured correctly with its Amazon EC2 Auto Scaling group\. In the CodeDeploy console, delete the Amazon EC2 Auto Scaling group from the deployment group, and then add it again\. Learn more: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/error-codes.html)  | 

## Related topics<a name="error-codes-related-topics"></a>

[Troubleshooting CodeDeploy](troubleshooting.md)
# Monitoring deployments with AWS CloudTrail<a name="monitoring-cloudtrail"></a>

CodeDeploy is integrated with CloudTrail, a service that captures API calls made by or on behalf of CodeDeploy in your AWS account and delivers the log files to an Amazon S3 bucket you specify\. CloudTrail captures API calls from the CodeDeploy console, from CodeDeploy commands through the AWS CLI, or from the CodeDeploy APIs directly\. Using the information collected by CloudTrail, you can determine which request was made to CodeDeploy, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## CodeDeploy information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to CodeDeploy actions are tracked in log files\. CodeDeploy records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All of the CodeDeploy actions are logged and documented in the [AWS CodeDeploy Command Line Reference](https://docs.aws.amazon.com/cli/latest/reference/deploy/index.html) and the [AWS CodeDeploy API Reference](https://docs.aws.amazon.com/codedeploy/latest/APIReference/)\. For example, calls to create deployments, delete applications, and register application revisions generate entries in CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the **userIdentity** field in the [CloudTrail event reference](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, Amazon S3 server\-side encryption \(SSE\) is used to encrypt your log files\.

You can have CloudTrail publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate CodeDeploy log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Receiving CloudTrail log files from multiple regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding CodeDeploy log file entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public API calls\.

The following example shows a CloudTrail log entry that demonstrates the CodeDeploy create deployment group action:

```
{
	"Records": [{
		"eventVersion": "1.02",
		"userIdentity": {
			"type": "AssumedRole",
			"principalId": "AKIAI44QH8DHBEXAMPLE:203.0.113.11",
			"arn": "arn:aws:sts::123456789012:assumed-role/example-role/203.0.113.11",
			"accountId": "123456789012",
			"accessKeyId": "AKIAIOSFODNN7EXAMPLE",
			"sessionContext": {
				"attributes": {
					"mfaAuthenticated": "false",
					"creationDate": "2014-11-27T03:57:36Z"
				},
				"sessionIssuer": {
					"type": "Role",
					"principalId": "AKIAI44QH8DHBEXAMPLE",
					"arn": "arn:aws:iam::123456789012:role/example-role",
					"accountId": "123456789012",
					"userName": "example-role"
				}
			}
		},
		"eventTime": "2014-11-27T03:57:36Z",
		"eventSource": "codedeploy.amazonaws.com",
		"eventName": "CreateDeploymentGroup",
		"awsRegion": "us-west-2",
		"sourceIPAddress": "203.0.113.11",
		"userAgent": "example-user-agent-string",
		"requestParameters": {
			"applicationName": "ExampleApplication",
			"serviceRoleArn": "arn:aws:iam::123456789012:role/example-instance-group-role",
			"deploymentGroupName": "ExampleDeploymentGroup",
			"ec2TagFilters": [{
                "value": "CodeDeployDemo",
				"type": "KEY_AND_VALUE",
				"key": "Name"
            }],
            "deploymentConfigName": "CodeDeployDefault.HalfAtATime"
		},
		"responseElements": {
			"deploymentGroupId": "7d64e680-e6f4-4c07-b10a-9e117EXAMPLE"
		},
		"requestID": "86168559-75e9-11e4-8cf8-75d18EXAMPLE",
		"eventID": "832b82d5-d474-44e8-a51d-093ccEXAMPLE",
		"eventType": "AwsApiCall",
		"recipientAccountId": "123456789012"
	},
    ... additional entries ...
    ]
}
```
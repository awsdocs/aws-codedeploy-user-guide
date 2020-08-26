# View log data for CodeDeploy EC2/On\-Premises deployments<a name="deployments-view-logs"></a>

You can view the log data created by a CodeDeploy deployment by setting up the Amazon CloudWatch Logs agent to view aggregated data in the CloudWatch console or by logging into an individual instance to review the log file\.

**Note**  
 Logs are not supported for AWS Lambda or Amazon ECS deployments\. They can be created for EC2/On\-Premises deployments only\. 

**Topics**
+ [View log file data in the Amazon CloudWatch console](#deployments-view-logs-cloudwatch)
+ [View log files on an instance](#deployments-view-logs-instance)

## View log file data in the Amazon CloudWatch console<a name="deployments-view-logs-cloudwatch"></a>

When the Amazon CloudWatch Logs agent is installed on an instance, deployment log data for all deployments to that instance becomes available for viewing in the CloudWatch console\. For simplicity, we recommend using Amazon CloudWatch Logs to centrally monitor log files instead of viewing them instance by instance\. For information about setting up the Amazon CloudWatch Logs agent, see [View CodeDeploy logs in CloudWatch Logs console](http://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)\.

## View log files on an instance<a name="deployments-view-logs-instance"></a>

To view deployment log data for an individual instance, you can sign in to the instance and browse for information about errors or other deployment events\.

**Topics**
+ [To view deployment log files on Amazon Linux, RHEL, and Ubuntu Server instances](#deployments-view-logs-instance-unix)
+ [To view deployment logs files on Windows Server instances](#deployments-view-logs-instance-windows)

### To view deployment log files on Amazon Linux, RHEL, and Ubuntu Server instances<a name="deployments-view-logs-instance-unix"></a>

On Amazon Linux, RHEL, and Ubuntu Server instances, deployment logs are stored in the following location:

 `/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log`

To view or analyze deployment logs on Amazon Linux, RHEL, and Ubuntu Server instances, sign in to the instance, and then type the following command to open the CodeDeploy agent log file:

```
less /var/log/aws/codedeploy-agent/codedeploy-agent.log
```

Type the following commands to browse the log file for error messages:


| Command | Result | 
| --- | --- | 
| & ERROR  | Show just the error messages in the log file\. Use a single space before and after the word ERROR\. | 
| / ERROR  | Search for the next error message\.¹  | 
| ? ERROR  | Search for the previous error message\.² Use a single space before and after the word ERROR\. | 
| G | Go to the end of the log file\. | 
| g | Go to the start of the log file\. | 
| q | Exit the log file\. | 
| h | Learn about additional commands\. | 
|  ¹ After you type **/ ERROR **, type **n** for the next error message\. Type **N** for the previous error message\.  ² After you type **? ERROR **, type **n** for the next error message, or type **N** for the previous error message\.  | 

You can also type the following command to open a CodeDeploy scripts log file:

```
less /opt/codedeploy-agent/deployment-root/deployment-group-ID/deployment-ID/logs/scripts.log
```

Type the following commands to browse the log file for error messages:


| Command | Result | 
| --- | --- | 
| &stderr | Show just the error messages in the log file\.  | 
| /stderr | Search for the next error message\.¹ | 
| ?stderr | Search for the previous error message\.² | 
| G | Go to the end of the log file\. | 
| g | Go to the start of the log file\. | 
| q | Exit the log file\. | 
| h | Learn about additional commands\. | 
|  ¹After you type **/stderr**, type **n** for the next error message forward\. Type **N** for the previous error message backward\. ² After you type **?stderr**, type **n** for the next error message backward\. Type **N** for the previous error message forward\.  | 

### To view deployment logs files on Windows Server instances<a name="deployments-view-logs-instance-windows"></a>

**CodeDeploy agent log file**: On Windows Server instances, the CodeDeploy agent log file is stored at the following location:

`C:\ProgramData\Amazon\CodeDeploy\log\codedeploy-agent-log.txt`

To view or analyze the CodeDeploy agent log file on a Windows Server instance, sign in to the instance, and then type the following command to open the file:

```
notepad C:\ProgramData\Amazon\CodeDeploy\log\codedeploy-agent-log.txt
```

To browse the log file for error messages, press CTRL\+F, type **ERROR \[**, and then press Enter to find the first error\. 

**CodeDeploy scripts log files**: On Windows Server instances, deployment logs are stored at the following location:

`C:\ProgramData\Amazon\CodeDeploy\deployment-group-id\deployment-id\logs\scripts.log`

Where:
+ *deployment\-group\-id* is a string such as `examplebf3a9c7a-7c19-4657-8684-b0c68d0cd3c4`
+ *deployment\-id* is an identifier such as `d-12EXAMPLE`

Type the following command to open a CodeDeploy scripts log file:

```
notepad C:\ProgramData\Amazon\CodeDeploy\deployment-group-ID\deployment-ID\logs\scripts.log
```

To browse the log file for error messages, press CTRL\+F, type **stderr**, and then press Enter to find the first error\. 
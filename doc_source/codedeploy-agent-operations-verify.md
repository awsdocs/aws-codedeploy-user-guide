# Verify the CodeDeploy agent is running<a name="codedeploy-agent-operations-verify"></a>

This section describes commands to run if you suspect the CodeDeploy agent has stopped running on an instance\.

**Topics**
+ [Verify the CodeDeploy agent for Amazon Linux or RHEL is running](#codedeploy-agent-operations-verify-linux)
+ [Verify the CodeDeploy agent for Ubuntu Server is running](#codedeploy-agent-operations-verify-ubuntu)
+ [Verify the CodeDeploy agent for Windows Server is running](#codedeploy-agent-operations-verify-windows)

## Verify the CodeDeploy agent for Amazon Linux or RHEL is running<a name="codedeploy-agent-operations-verify-linux"></a>

To see if the CodeDeploy agent is installed and running, sign in to the instance, and run the following command:

```
sudo service codedeploy-agent status
```

If the command returns an error, the CodeDeploy agent is not installed\. Install it as described in [Install the CodeDeploy agent for Amazon Linux or RHEL](codedeploy-agent-operations-install-linux.md)\.

If the CodeDeploy agent is installed and running, you should see a message like `The AWS CodeDeploy agent is running`\.

If you see a message like `error: No AWS CodeDeploy agent running`, start the service and run the following two commands, one at a time:

```
sudo service codedeploy-agent start
```

```
sudo service codedeploy-agent status
```

## Verify the CodeDeploy agent for Ubuntu Server is running<a name="codedeploy-agent-operations-verify-ubuntu"></a>

To see if the CodeDeploy agent is installed and running, sign in to the instance, and run the following command:

```
sudo service codedeploy-agent status
```

If the command returns an error, the CodeDeploy agent is not installed\. Install it as described in [Install the CodeDeploy agent for Ubuntu Server](codedeploy-agent-operations-install-ubuntu.md)\.

If the CodeDeploy agent is installed and running, you should see a message like `The AWS CodeDeploy agent is running`\.

If you see a message like `error: No AWS CodeDeploy agent running`, start the service and run the following two commands, one at a time:

```
sudo service codedeploy-agent start
```

```
sudo service codedeploy-agent status
```

## Verify the CodeDeploy agent for Windows Server is running<a name="codedeploy-agent-operations-verify-windows"></a>

To see if the CodeDeploy agent is installed and running, sign in to the instance, and run the following command:

```
powershell.exe -Command Get-Service -Name codedeployagent
```

You should see output similar to the following:

```
Status   Name               DisplayName
------   ----               -----------
Running codedeployagent    CodeDeploy Host Agent Service
```

If the command returns an error, the CodeDeploy agent is not installed\. Install it as described in [Install the CodeDeploy agent for Windows Server](codedeploy-agent-operations-install-windows.md)\.

If `Status` shows anything other than `Running`, start the service with the following command:

```
powershell.exe -Command Start-Service -Name codedeployagent
```

You can restart the service with the following command:

```
powershell.exe -Command Restart-Service -Name codedeployagent
```

You can stop the service with the following command:

```
powershell.exe -Command Stop-Service -Name codedeployagent
```
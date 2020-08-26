# Update the CodeDeploy agent on Ubuntu Server<a name="codedeploy-agent-operations-update-ubuntu"></a>

To configure automatic, scheduled updates of the CodeDeploy agent using AWS Systems Manager, follow the steps in [Install the CodeDeploy agent with AWS Systems Manager](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ssm.html)\.

If you want to force an update of the CodeDeploy agent, sign in to the instance, and run the following command:

```
sudo /opt/codedeploy-agent/bin/install auto
```
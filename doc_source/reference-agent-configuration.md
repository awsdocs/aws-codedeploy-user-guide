# CodeDeploy agent configuration reference<a name="reference-agent-configuration"></a>

When the CodeDeploy agent is installed, a configuration file is placed on the instance\. This configuration file specifies directory paths and other settings for CodeDeploy to use as it interacts with the instance\. You can change some of the configuration options in the file\.

For Amazon Linux, Ubuntu Server, and Red Hat Enterprise Linux \(RHEL\) instances, the configuration file is named `codedeployagent.yml`\. It is placed in the `/etc/codedeploy-agent/conf` directory\.

For Windows Server instances, the configuration file is named `conf.yml`\. It is placed in the `C:\ProgramData\Amazon\CodeDeploy` directory\.

The configuration settings include:


****  

|  |  | 
| --- |--- |
|  **:log\_aws\_wire:**  |  Set to `true` for the CodeDeploy agent to capture wire logs from Amazon S3 and write them to a file named `codedeploy-agent.wire.log` in the location pointed to by the **:log\_dir:** setting\.   You should set **:log\_aws\_wire:** to `true` only for the amount of time required to capture wire logs\. The `codedeploy-agent.wire.log` file can grow to a very large size quickly\. The wire log output in this file might contain sensitive information, including the plain\-text contents of files transferred into, or out of, Amazon S3 while this setting was set to `true`\. The wire logs contain information about all Amazon S3 activity associated with the AWS account while this setting was set to `true`, not just activity related to CodeDeploy deployments\.  The default setting is `false`\. This setting applies to all instance types\. You must add this configuration setting to Windows Server instances to be able to use it\.  | 
|  **:log\_dir:**  | The folder on the instance where log files related to CodeDeploy agent operations are stored\. The default setting is `'/var/log/aws/codedeploy-agent'` for Amazon Linux, Ubuntu Server, and RHEL instances and `C:\ProgramData\Amazon\CodeDeploy\log` for Windows Server instances\. | 
|  **:pid\_dir:**  | The folder where `codedeploy-agent.pid` is stored\. This file contains the process ID \(PID\) of the CodeDeploy agent\. The default setting is `'/opt/codedeploy-agent/state/.pid'`\. This setting applies to Amazon Linux, Ubuntu Server, and RHEL instances only\. | 
|  **:program\_name:**  | The CodeDeploy agent program name\. The default setting is `codedeploy-agent`\.This setting applies to Amazon Linux, Ubuntu Server, and RHEL instances only\. | 
|  **:root\_dir:**  | The folder where related revisions, deployment history, and deployment scripts on the instance are stored\. The default setting is `'/opt/codedeploy-agent/deployment-root'` for Amazon Linux, Ubuntu Server, and RHEL instances and `C:\ProgramData\Amazon\CodeDeploy` for Windows Server instances\. | 
|  **:verbose:**  | Set to `true` for the CodeDeploy agent to print debug messages log files on the instance\. The default setting is `false` for Amazon Linux, Ubuntu Server, and RHEL instances and `true` for Windows Server instances\. | 
|  **:wait\_between\_runs:**  | The interval, in seconds, between CodeDeploy agent polling of CodeDeploy for pending deployments\. The default setting is `1`\. | 
|  **:on\_premises\_config\_file:**  | For on\-premises instances, the path to an alternate location for the configuration file named `codedeploy.onpremises.yml` \(for Ubuntu Server and RHEL\) or `conf.onpremises.yml` \(for Windows Server\)\. By default, these files are stored in `/etc/codedeploy-agent/conf`/`codedeploy.onpremises.yml` for Ubuntu Server and RHEL and `C:\ProgramData\Amazon\CodeDeploy`\\`conf.onpremises.yml` for Windows Server\. Available in version 1\.0\.1\.686 and later versions of the CodeDeploy agent\.  | 
|  **:proxy\_uri:**  |  \(Optional\) The HTTP proxy through which you want the CodeDeploy agent to connect to AWS for your CodeDeploy operations\. Use a format similar to `https://user:password@my.proxy:443/path?query`\. Available in version 1\.0\.1\.824 and later versions of the CodeDeploy agent\.  | 
|  **:max\_revisions:**  |  \(Optional\) The number of application revisions for a deployment group that you want the CodeDeploy agent to archive\. Any revisions that exceed the number specified are deleted\.  Enter any positive integer\. If no value is specified, CodeDeploy will retain the five most recent revisions in addition to the currently deployed revision\.  Supported in version 1\.0\.1\.966 and later versions of the CodeDeploy agent\.  | 
|  **:enable\_auth\_policy:**  |  \(Optional\) Set to `true` if you want to use [ IAM authorization](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html#intro-structure-authorization) to configure access control and limit permission of the IAM role or user that the CodeDeploy Agent is using\. To [Use CodeDeploy with Amazon Virtual Private Cloud](vpc-endpoints.md), this value must be `true`\.  The default setting is `false`\.  | 

## Related topics<a name="reference-agent-configuration-related-topics"></a>

[Working with the CodeDeploy agent](codedeploy-agent.md)

[Managing CodeDeploy agent operations](codedeploy-agent-operations.md)
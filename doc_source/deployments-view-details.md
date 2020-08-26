# View CodeDeploydeployment details<a name="deployments-view-details"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to view details about deployments associated with your AWS account\.

**Note**  
You can view EC2/On\-Premises deployment logs on your instances in the following locations:  
Amazon Linux, RHEL, and Ubuntu Server: `/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log`
Windows Server: C:\\ProgramData\\Amazon\\CodeDeploy<DEPLOYMENT\-GROUP\-ID><DEPLOYMENT\-ID>\\logs\\scripts\.log
For more information, see [Analyzing log files to investigate deployment failures on instances](troubleshooting-ec2-instances.md#troubleshooting-deploy-failures)\.

**Topics**
+ [View deployment details \(console\)](#deployments-view-details-console)
+ [View deployment details \(CLI\)](#deployments-view-details-cli)

## View deployment details \(console\)<a name="deployments-view-details-console"></a>

To use the CodeDeploy console to view deployment details:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Deployments**\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. CodeDeploy is supported in these regions only\.

1. To see more details for a single deployment, in **Deployment history**, choose the deployment ID or choose the button next to the deployment ID, and then choose **View**\.

## View deployment details \(CLI\)<a name="deployments-view-details-cli"></a>

To use the AWS CLI to view deployment details, call the `get-deployment` command or the `batch-get-deployments` command\. You can call the `list-deployments` command to get a list of unique deployment IDs to use as inputs to the `get-deployment` command and the `batch-get-deployments` command\.

To view details about a single deployment, call the [get\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment.html) command, specifying the unique deployment identifier\. To get the deployment ID, call the [list\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.

To view details about multiple deployments, call the [batch\-get\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/batch-get-deployments.html) command, specifying multiple unique deployment identifiers\. To get the deployment IDs, call the [list\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.

To view a list of deployment IDs, call the [list\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command, specifying:
+ The name of the application associated with the deployment\. To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The name of the deployment group associated with the deployment\. To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.
+ Optionally, whether to include details about deployments by their deployment status\. \(If not specified, all matching deployments will be listed, regardless of their deployment status\.\)
+ Optionally, whether to include details about deployments by their deployment creation start times or end times, or both\. \(If not specified, all matching deployments will be listed, regardless of their creation times\.\)
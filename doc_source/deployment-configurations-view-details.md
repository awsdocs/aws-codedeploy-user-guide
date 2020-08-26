# View deployment configuration details with CodeDeploy<a name="deployment-configurations-view-details"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to view details about deployment configurations associated with your AWS account\. For descriptions of the predefined CodeDeploy deployment configurations, see [Predefined deployment configurations for an EC2/on\-premises compute platform ](deployment-configurations.md#deployment-configurations-predefined)\.

**Topics**
+ [View deployment configuration details \(console\)](#deployment-configurations-view-details-console)
+ [View deployment configuration \(CLI\)](#deployment-configurations-view-details-cli)

## View deployment configuration details \(console\)<a name="deployment-configurations-view-details-console"></a>

To use the CodeDeploy console to view a list of deployment configuration names:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and choose **Deployment configurations**\.

    Here you can see the deployment configuration names and criteria for each deployment configuration\. 
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. CodeDeploy is supported in these regions only\.

## View deployment configuration \(CLI\)<a name="deployment-configurations-view-details-cli"></a>

To use the AWS CLI to view deployment configuration details, call either the `get-deployment-config` command or the `list-deployment-configs` command\.

To view details about a single deployment configuration, call the [get\-deployment\-config](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-config.html) command, specifying the unique deployment configuration name\.

To view details about multiple deployment configurations, call the [list\-deployments](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployments.html) command\.
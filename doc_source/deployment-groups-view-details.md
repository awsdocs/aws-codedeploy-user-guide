# View deployment group details with CodeDeploy<a name="deployment-groups-view-details"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to view details about all deployment groups associated with an application\.

**Topics**
+ [View deployment group details \(console\)](#deployment-groups-view-details-console)
+ [View deployment group details \(CLI\)](#deployment-groups-view-details-cli)

## View deployment group details \(console\)<a name="deployment-groups-view-details-console"></a>

To use the CodeDeploy console to view deployment group details:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. On the **Applications** page, choose the application name associated with the deployment group\. 
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. CodeDeploy is supported in these regions only\.

1. To view details about an individual deployment group, on the **Deployment groups** tab, choose the name of the deployment group\.

## View deployment group details \(CLI\)<a name="deployment-groups-view-details-cli"></a>

To use the AWS CLI to view deployment group details, call either the `get-deployment-group` command or the `list-deployment-groups` command\.

To view details about a single deployment group, call the [get\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-group.html) command, specifying: 
+ The application name associated with the deployment group\. To get the application name, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ The deployment group name\. To get the deployment group name, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command\.

To view a list of deployment group names, call the [list\-deployment\-groups](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-groups.html) command, specifying the application name associated with the deployment groups\. To get the application name, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\. 
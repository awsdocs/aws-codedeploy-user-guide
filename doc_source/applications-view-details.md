# View application details with CodeDeploy<a name="applications-view-details"></a>

You can use the CodeDeploy console, the AWS CLI, or the CodeDeploy APIs to view details about all applications associated with your AWS account\.

**Topics**
+ [View application details \(console\)](#applications-view-details-console)
+ [View application details \(CLI\)](#applications-view-details-cli)

## View application details \(console\)<a name="applications-view-details-console"></a>

To use the CodeDeploy console to view application details:

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and choose **Getting started**\.

1. To view additional application details, choose the application name in the list\.

## View application details \(CLI\)<a name="applications-view-details-cli"></a>

To use the AWS CLI to view application details, call the get\-application command, the batch\-get\-application command, or the list\-applications command\.

To view details about a single application, call the [get\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-application.html) command, specifying the application name\.

To view details about multiple applications, call the [batch\-get\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/batch-get-applications.html) command, specifying multiple application names\.

To view a list of application names, call the [list\-applications](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
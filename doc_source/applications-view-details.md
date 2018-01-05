# View Application Details with AWS CodeDeploy<a name="applications-view-details"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to view details about all applications associated with your AWS account\.


+ [View Application Details \(Console\)](#applications-view-details-console)
+ [View Application Details \(CLI\)](#applications-view-details-cli)

## View Application Details \(Console\)<a name="applications-view-details-console"></a>

To use the AWS CodeDeploy console to view application details:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Applications**\. 
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy supports these regions only\.

1. To view additional application details, choose the application name in the list\.

## View Application Details \(CLI\)<a name="applications-view-details-cli"></a>

To use the AWS CLI to view application details, call the get\-application command, the batch\-get\-application command, or the list\-applications command\.

To view details about a single application, call the [get\-application](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-application.html) command, specifying the application name\.

To view details about multiple applications, call the [batch\-get\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/batch-get-applications.html) command, specifying multiple application names\.

To view a list of application names, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
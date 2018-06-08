# View Application Revision Details with AWS CodeDeploy<a name="application-revisions-view-details"></a>

You can use the AWS CodeDeploy console, the AWS CLI, or the AWS CodeDeploy APIs to view details about all application revisions that are registered to your AWS account for a specified application\.

For information about registering a revision, see [Register an Application Revision in Amazon S3 with AWS CodeDeploy](application-revisions-register.md)\.

**Topics**
+ [View Application Revision Details \(Console\)](#application-revisions-view-details-console)
+ [View Application Revision Details \(CLI\)](#application-revisions-view-details-cli)

## View Application Revision Details \(Console\)<a name="application-revisions-view-details-console"></a>

To view application revision details:

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Applications**\.

1. On the **Applications** page, choose the name of the application with revision details you want to view\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy is supported in these regions only\.

1. On the **Application details** page, under **Revisions**, review the list of revisions that are registered for the application\. Choose the arrow next to a revision for more details\.

## View Application Revision Details \(CLI\)<a name="application-revisions-view-details-cli"></a>

To use the AWS CLI to view an application revision, call either the **get\-application\-revision** command or the **list\-application\-revisions** command\.

**Note**  
 References to GitHub apply to deployments to EC2/On\-Premises deployments only\. Revisions for AWS Lambda deployments do not work with GitHub\. 

To view details about a single application revision, call the [get\-application\-revision](http://docs.aws.amazon.com/cli/latest/reference/deploy/get-application-revision.html) command, specifying: 
+ The application name\. To get the application name, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ For a revision stored in GitHub, the GitHub repository name and the ID of the commit that references the application revision that was pushed to the repository\.
+ For a revision stored in Amazon S3, the Amazon S3 bucket name containing the revision; the name and file type of the uploaded archive file; and, optionally, the archive file's Amazon S3 version identifier and ETag\. If the version identifier, ETag, or both were specified during a call to [register\-application\-revision](http://docs.aws.amazon.com/cli/latest/reference/deploy/register-application-revision.html), they must be specified here\.

To view details about multiple application revisions, call the [list\-application\-revisions](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-application-revisions.html) command, specifying:
+ The application name\. To get the application name, call the [list\-applications](http://docs.aws.amazon.com/cli/latest/reference/deploy/list-applications.html) command\.
+ Optionally, to view details for Amazon S3 application revisions only, the Amazon S3 bucket name containing the revisions\.
+ Optionally, to view details for Amazon S3 application revisions only, a prefix string to limit the search to Amazon S3 application revisions\. \(If not specified, AWS CodeDeploy will list all matching Amazon S3 application revisions\.\)
+ Optionally, whether to list revision details based on whether each revision is the target revision of a deployment group\. \(If not specified, AWS CodeDeploy will list all matching revisions\.\)
+ Optionally, the column name and order by which to sort the list of revision details\. \(If not specified, AWS CodeDeploy will list results in an arbitrary order\.\)

You can list all revisions or only those revisions stored in Amazon S3\. You cannot list only those revisions stored in GitHub\.
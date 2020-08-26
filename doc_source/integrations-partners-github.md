# Integrating CodeDeploy with GitHub<a name="integrations-partners-github"></a>

CodeDeploy supports [GitHub](https://github.com/about), a web\-based code hosting and sharing service\. CodeDeploy can deploy application revisions stored in GitHub repositories or Amazon S3 buckets to instances\. CodeDeploy supports GitHub for EC2/On\-Premises deployments only\.

**Topics**
+ [Video introduction to CodeDeploy integration with GitHub](#video-introduction)
+ [Deploying CodeDeploy revisions from GitHub](#github-deployment-steps)
+ [GitHub behaviors with CodeDeploy](#github-behaviors)

## Video introduction to CodeDeploy integration with GitHub<a name="video-introduction"></a>

This short video \(5:20\) demonstrates how to automate application deployments with CodeDeploy from your existing GitHub workflows\.

[![AWS Videos](http://img.youtube.com/vi/N3saR9D7hq8/0.jpg)](http://www.youtube.com/watch?v=N3saR9D7hq8)

## Deploying CodeDeploy revisions from GitHub<a name="github-deployment-steps"></a>

To deploy an application revision from a GitHub repository to instances:

1. Create a revision that's compatible with CodeDeploy and the Amazon EC2 instance type to which you will deploy\.

   To create a compatible revision, follow the instructions in [Plan a revision for CodeDeploy](application-revisions-plan.md) and [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md)\. 

1. Use a GitHub account to add your revision to a GitHub repository\.

   To create a GitHub account, see [Join GitHub](https://github.com/join)\. To create a GitHub repository, see [Create a repo](https://help.github.com/articles/create-a-repo/)\.

1. Use the **Create deployment** page in the CodeDeploy console or the AWS CLI create\-deployment command to deploy your revision from your GitHub repository to target instances configured for use in CodeDeploy deployments\.

   If you want to call the create\-deployment command, you must first use the **Create deployment** page of the console to give CodeDeploy permission to interact with GitHub on behalf of your preferred GitHub account for the specified application\. You only need to do this once per application\.

   To learn how to use the **Create deployment** page to deploy from a GitHub repository, see [Create a deployment with CodeDeploy](deployments-create.md)\.

   To learn how to call the create\-deployment command to deploy from a GitHub repository, see [Create an EC2/On\-Premises Compute Platform deployment \(CLI\)](deployments-create-cli.md)\.

   To learn how to prepare instances for use in CodeDeploy deployments, see [Working with instances for CodeDeploy](instances.md)\.

For more information, see [Tutorial: Use CodeDeploy to deploy an application from GitHub](tutorials-github.md)\.

## GitHub behaviors with CodeDeploy<a name="github-behaviors"></a>

**Topics**
+ [GitHub authentication with applications in CodeDeploy](#behaviors-authentication)
+ [CodeDeploy interaction with private and public GitHub repositories](#behaviors-interactions-private-and-public)
+ [CodeDeploy interaction with organization\-managed GitHub repositories](#behaviors-interactions-organization-managed)
+ [Automatically deploy from CodePipeline with CodeDeploy](#behaviors-deploy-automatically)

### GitHub authentication with applications in CodeDeploy<a name="behaviors-authentication"></a>

After you give CodeDeploy permission to interact with GitHub, the association between that GitHub account and application is stored in CodeDeploy\. You can link the application to a different GitHub account\. You can also revoke permission for CodeDeploy to interact with GitHub\.

**To link a GitHub account to an application in CodeDeploy**

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. Choose the application you want to link to a different GitHub account\.

1. If your application does not have a deloyment group, choose **Create deployment group** to create one\. For more information, see [Create a deployment group with CodeDeploy](deployment-groups-create.md)\. A deployment group is required to choose **Create deployment** in the next step\.

1.  From **Deployments**, choose **Create deployment**\. 
**Note**  
You don't have to create a new deployment\. This is currently the only way to link a different GitHub account to an application\.

1.  In **Deployment settings**, for **Revision type**, choose **My application is stored in GitHub**\. 

1. Do one of the following:
   + To create a connection for AWS CodeDeploy applications to a GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub token name**, type a name to identify this connection, and then choose **Connect to GitHub**\. The web page prompts you to authorize CodeDeploy to interact with GitHub for your application\. Continue to step 10\.
   + To use a connection you have already created, in **GitHub token name**, select its name, and then choose **Connect to GitHub**\. Continue to step 8\.
   + To create a connection to a different GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub token name**, type a name to identify the connection, and then choose **Connect to GitHub**\. The web page prompts you to authorize CodeDeploy to interact with GitHub for your application\. Continue to step 10\.

1. If you are not already signed in to GitHub, follow the instructions on the **Sign in** page to sign in with the GitHub account to which you want to link the application\.

1. Choose **Authorize application**\. GitHub gives CodeDeploy permission to interact with GitHub on behalf of the signed\-in GitHub account for the selected application\. 

1. If you do not want to create a deployment, choose **Cancel**\.

**To revoke permission for CodeDeploy to interact with GitHub**

1. Sign in to [GitHub ](https://github.com/dashboard) using credentials for the GitHub account in which you want to revoke AWS CodeDeploy permission\.

1. Open the GitHub [Applications](https://github.com/settings/applications) page, locate **CodeDeploy** in the list of authorized applications, and then follow the GitHub procedure for revoking authorization for an application\.

### CodeDeploy interaction with private and public GitHub repositories<a name="behaviors-interactions-private-and-public"></a>

CodeDeploy supports the deployment of applications from private and public GitHub repositories\. When you give CodeDeploy permission to access GitHub on your behalf, CodeDeploy will have read\-write access to all of the private GitHub repositories to which your GitHub account has access\. However, CodeDeploy only reads from GitHub repositories\. It will not write to any of your private GitHub repositories\.

### CodeDeploy interaction with organization\-managed GitHub repositories<a name="behaviors-interactions-organization-managed"></a>

By default, GitHub repositories that are managed by an organization \(as opposed to your account's own private or public repositories\) do not grant access to third\-party applications, including CodeDeploy\. Your deployment will fail if an organization's third\-party application restrictions are enabled in GitHub and you attempt to deploy code from its GitHub repository\. There are two ways to resolve this issue\. 
+ As an organization member, you can ask the organization owner to approve access to CodeDeploy\. The steps for requesting this access depend on whether you have already authorized CodeDeploy for your individual account:
  + If you have authorized access to CodeDeploy in your account, see [Requesting organization approval for your authorized applications](https://help.github.com/articles/requesting-organization-approval-for-your-authorized-applications/)\.
  + If you have not yet authorized access to CodeDeploy in your account, see [Requesting organization approval for third\-party applications](https://help.github.com/articles/requesting-organization-approval-for-third-party-applications/)\.
+ The organization owner can disable all third\-party application restrictions for the organization\. For information, see [Disabling third\-party application restrictions for your organization](https://help.github.com/articles/disabling-third-party-application-restrictions-for-your-organization/)\.

For more information, see [About third\-party application restrictions](https://help.github.com/articles/about-third-party-application-restrictions/)\.

### Automatically deploy from CodePipeline with CodeDeploy<a name="behaviors-deploy-automatically"></a>

You can trigger a deployment from a CodePipeline whenever the source code changes\. For more infomation, see [CodePipeline](https://aws.amazon.com/codepipeline/)\.
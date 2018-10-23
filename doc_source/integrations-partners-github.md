# Integrating AWS CodeDeploy with GitHub<a name="integrations-partners-github"></a>

AWS CodeDeploy supports [GitHub](https://github.com/about), a web\-based code hosting and sharing service\. AWS CodeDeploy can deploy application revisions stored in GitHub repositories or Amazon S3 buckets to instances\. 

**Topics**
+ [Video Introduction to AWS CodeDeploy Integration with GitHub](#video-introduction)
+ [Deploying AWS CodeDeploy Revisions from GitHub](#github-deployment-steps)
+ [GitHub Behaviors with AWS CodeDeploy](#github-behaviors)

## Video Introduction to AWS CodeDeploy Integration with GitHub<a name="video-introduction"></a>

This short video \(5:20\) demonstrates how to automate application deployments with AWS CodeDeploy from your existing GitHub workflows\.

[![AWS Videos](http://img.youtube.com/vi/N3saR9D7hq8/0.jpg)](http://www.youtube.com/watch?v=N3saR9D7hq8)

## Deploying AWS CodeDeploy Revisions from GitHub<a name="github-deployment-steps"></a>

To deploy an application revision from a GitHub repository to instances:

1. Create a revision that's compatible with AWS CodeDeploy and the Amazon EC2 instance type to which you will deploy\.

   To create a compatible revision, follow the instructions in [Plan a Revision for AWS CodeDeploy](application-revisions-plan.md) and [Add an Application Specification File to a Revision for AWS CodeDeploy](application-revisions-appspec-file.md)\. 

1. Use a GitHub account to add your revision to a GitHub repository\.

   To create a GitHub account, see [Join GitHub](https://github.com/join)\. To create a GitHub repository, see [Create a Repo](https://help.github.com/articles/create-a-repo/)\.

1. Use the **Create deployment** page in the AWS CodeDeploy console or the AWS CLI create\-deployment command to deploy your revision from your GitHub repository to target instances configured for use in AWS CodeDeploy deployments\.

   If you want to call the create\-deployment command, you must first use the **Create deployment** page of the console to give AWS CodeDeploy permission to interact with GitHub on behalf of your preferred GitHub account for the specified application\. You only need to do this once per application\.

   To learn how to use the **Create deployment** page to deploy from a GitHub repository, see [Create a Deployment with AWS CodeDeploy](deployments-create.md)\.

   To learn how to call the create\-deployment command to deploy from a GitHub repository, see [Create an EC2/On\-Premises Compute Platform Deployment \(CLI\)](deployments-create-cli.md)\.

   To learn how to prepare instances for use in AWS CodeDeploy deployments, see [Working with Instances for AWS CodeDeploy](instances.md)\.

For more information, see [Tutorial: Use AWS CodeDeploy to Deploy an Application from GitHub](tutorials-github.md)\.

## GitHub Behaviors with AWS CodeDeploy<a name="github-behaviors"></a>

**Topics**
+ [GitHub Authentication with Applications in AWS CodeDeploy](#behaviors-authentication)
+ [AWS CodeDeploy Interaction with Private and Public GitHub Repositories](#behaviors-interactions-private-and-public)
+ [AWS CodeDeploy Interaction with Organization\-Managed GitHub Repositories](#behaviors-interactions-organization-managed)
+ [Automatically Deploy from AWS CodePipeline with AWS CodeDeploy](#behaviors-deploy-automatically)

### GitHub Authentication with Applications in AWS CodeDeploy<a name="behaviors-authentication"></a>

After you give AWS CodeDeploy permission to interact with GitHub, the association between that GitHub account and application is stored in AWS CodeDeploy\. You can link the application to a different GitHub account\. You can also revoke permission for AWS CodeDeploy to interact with GitHub\.

**To link a GitHub account to an application in AWS CodeDeploy**

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Deployments**\.

1. Choose **Create deployment**\.
**Note**  
You don't have to create a new deployment\. This is currently the only way to link a different GitHub account to an application\.

1. From the **Application** drop\-down list, choose the application you want to link to a different GitHub account\.

1. Next to **Repository type**, choose **My application is stored in GitHub**\.

1. In **Connect to GitHub**, do one of the following:
   + To create a connection for AWS CodeDeploy applications to a GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub account**, type a name to identify this connection, and then choose **Connect to GitHub**\. The web page prompts you to authorize AWS CodeDeploy to interact with GitHub for your application\. Continue to step 2\.
   + To use a connection you have already created, in **GitHub account**, select its name, and then choose **Connect to GitHub**\. Continue to step 4\.
   + To create a connection to a different GitHub account, sign out of GitHub in a separate web browser tab\. Choose **Connect to a different GitHub account**, and then choose **Connect to GitHub**\. Continue to step 2\.

1. If you are not already signed in to GitHub, follow the instructions on the **Sign in** page to sign in with the GitHub account to which you want to link the application\.

1. Choose **Authorize application**\. GitHub gives AWS CodeDeploy permission to interact with GitHub on behalf of the signed\-in GitHub account for the selected application\. 

1. If you do not want to create a deployment, choose **Cancel**\.

**To revoke permission for AWS CodeDeploy to interact with GitHub**

1. Sign in to [GitHub ](https://github.com/dashboard) using credentials for the GitHub account in which you want to revoke AWS CodeDeploy permission\.

1. Open the GitHub [Applications](https://github.com/settings/applications) page, locate **AWS CodeDeploy** in the list of authorized applications, and then follow the GitHub procedure for revoking authorization for an application\.

### AWS CodeDeploy Interaction with Private and Public GitHub Repositories<a name="behaviors-interactions-private-and-public"></a>

AWS CodeDeploy supports the deployment of applications from private and public GitHub repositories\. When you give AWS CodeDeploy permission to access GitHub on your behalf, AWS CodeDeploy will have read\-write access to all of the private GitHub repositories to which your GitHub account has access\. However, AWS CodeDeploy only reads from GitHub repositories\. It will not write to any of your private GitHub repositories\.

### AWS CodeDeploy Interaction with Organization\-Managed GitHub Repositories<a name="behaviors-interactions-organization-managed"></a>

By default, GitHub repositories that are managed by an organization \(as opposed to your account's own private or public repositories\) do not grant access to third\-party applications, including AWS CodeDeploy\. Your deployment will fail if an organization's third\-party application restrictions are enabled in GitHub and you attempt to deploy code from its GitHub repository\. There are two ways to resolve this issue\. 
+ As an organization member, you can ask the organization owner to approve access to AWS CodeDeploy\. The steps for requesting this access depend on whether you have already authorized AWS CodeDeploy for your individual account:
  + If you have authorized access to AWS CodeDeploy in your account, see [Requesting Organization Approval for Your Authorized Applications](https://help.github.com/articles/requesting-organization-approval-for-your-authorized-applications/)\.
  + If you have not yet authorized access to AWS CodeDeploy in your account, see [Requesting Organization Approval for Third\-Party Applications](https://help.github.com/articles/requesting-organization-approval-for-third-party-applications/)\.
+ The organization owner can disable all third\-party application restrictions for the organization\. For information, see [Disabling Third\-Party Application Restrictions for Your Organization](https://help.github.com/articles/disabling-third-party-application-restrictions-for-your-organization/)\.

For more information, see [About Third\-Party Application Restrictions](https://help.github.com/articles/about-third-party-application-restrictions/)\.

### Automatically Deploy from AWS CodePipeline with AWS CodeDeploy<a name="behaviors-deploy-automatically"></a>

You can trigger a deployment from a AWS CodePipeline whenever the source code changes\. For more infomation, see [AWS CodePipeline](https://aws.amazon.com//codepipeline/)\.
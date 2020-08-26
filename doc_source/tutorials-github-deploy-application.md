# Step 6: Deploy the application to the instance<a name="tutorials-github-deploy-application"></a>

In this step, you use the CodeDeploy console or the AWS CLI to deploy the sample revision from your GitHub repository to your instance\. 

## To deploy the revision \(console\)<a name="tutorials-github-deploy-application-console"></a>

1. On the **Deployment group details** page, choose **Create deployment**\.

1. In **Deployment group**, choose **`CodeDeployGitHubDemo-DepGrp`**\.

1. In **Revision type**, choose **GitHub**\.

1. In **Connect to GitHub**, do one of the following:
   + To create a connection for CodeDeploy applications to a GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub account**, enter a name to identify this connection, and then choose **Connect to GitHub**\. The webpage prompts you to authorize CodeDeploy to interact with GitHub for the application named `CodeDeployGitHubDemo-App`\. Continue to step 5\.
   + To use a connection you have already created, in **GitHub account**, select its name, and then choose **Connect to GitHub**\. Continue to step 7\.
   + To create a connection to a different GitHub account, sign out of GitHub in a separate web browser tab\. Choose **Connect to a different GitHub account**, and then choose **Connect to GitHub**\. Continue to step 5\.

1. Follow the instructions on the **Sign in** page to sign in with your GitHub account\.

1. On the **Authorize application** page, choose **Authorize application**\. 

1. On the CodeDeploy **Create deployment** page, in **Repository name**, enter the GitHub user name you used to sign in, followed by a forward slash \(`/`\), followed by the name of the repository where you pushed your application revision \(for example, ***my\-github\-user\-name*/CodeDeployGitHubDemo**\)\.

   If you are unsure of the value to enter, or if you want to specify a different repository:

   1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

   1. In **Your repositories**, hover your mouse pointer over the target repository name\. A tooltip appears, displaying the GitHub user or organization name, followed by a forward slash \(`/`\), followed by the name of the repository\. Enter this value into **Repository name**\.
**Note**  
If the target repository name is not displayed in **Your repositories**, use the **Search GitHub** box to find the target repository and GitHub user or organization name\.

1. In the **Commit ID** box, enter the ID of the commit associated with the push of your application revision to GitHub\.

   If you are unsure of the value to enter:

   1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

   1. In **Your repositories**, choose **CodeDeployGitHubDemo**\.

   1. In the list of commits, find and copy the commit ID associated with the push of your application revision to GitHub\. This ID is typically 40 characters in length and consists of both letters and numbers\. \(Do not use the shorter version of the commit ID, which is typically the first 10 characters of the longer version\.\)

   1. Paste the commit ID into the **Commit ID** box\.

1. Choose **Deploy**, and continue to the next step\. 

## To deploy the revision \(CLI\)<a name="tutorials-github-deploy-application-cli"></a>

Before you can call any AWS CLI commands that interact with GitHub \(such as the create\-deployment command, which you will call next\), you must give CodeDeploy permission to use your GitHub user account to interact with GitHub for the `CodeDeployGitHubDemo-App` application\. Currently, you must use the CodeDeploy console to do this\.

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. Choose **CodeDeployGitHubDemo\-App**\.

1. On the **Deployments** tab, choose **Create deployment**\.
**Note**  
You will not be creating a new deployment\. This is currently the only way to give CodeDeploy permission to interact with GitHub on behalf of your GitHub user account\.

1. From **Deployment group**, choose **CodeDeployGitHubDemo\-DepGrp**\.

1. In **Revision type**, choose **GitHub**\.

1. In **Connect to GitHub**, do one of the following:
   + To create a connection for CodeDeploy applications to a GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub account**, type a name to identify this connection, and then choose **Connect to GitHub**\. The web page prompts you to authorize CodeDeploy to interact with GitHub for the application named `CodeDeployGitHubDemo-App`\. Continue to step 8\.
   + To use a connection you have already created, in **GitHub account**, select its name, and then choose **Connect to GitHub**\. Continue to step 10\.
   + To create a connection to a different GitHub account, sign out of GitHub in a separate web browser tab\. Choose **Connect to a different GitHub account**, and then choose **Connect to GitHub**\. Continue to step 8\.

1. Follow the instructions on the **Sign in** page to sign in with your GitHub user name or email and password\.

1. On the **Authorize application** page, choose **Authorize application**\. 

1. On the CodeDeploy **Create deployment** page, choose **Cancel**\.

1. Call the create\-deployment command to deploy the revision from your GitHub repository to the instance, where:
   + *repository* is your GitHub account name, followed by a forward\-slash \(`/`\), followed by the name of your repository \(`CodeDeployGitHubDemo`\), for example, `MyGitHubUserName/CodeDeployGitHubDemo`\.

     If you are unsure of the value to use, or if you want to specify a different repository:

     1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

     1. In **Your repositories**, hover your mouse pointer over the target repository name\. A tooltip appears, displaying the GitHub user or organization name, followed by a forward slash \(`/`\), followed by the name of the repository\. This is the value to use\.
**Note**  
If the target repository name does not appear in **Your repositories**, use the **Search GitHub** box to find the target repository and corresponding GitHub user or organization name\.
   + *commit\-id* is the commit associated with the version of the application revision you pushed to your repository \(for example, `f835159a...528eb76f`\)\. 

     If you are unsure of the value to use:

     1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

     1. In **Your repositories**, choose **CodeDeployGitHubDemo**\.

     1. In the list of commits, find the commit ID associated with the push of your application revision to GitHub\. This ID is typically 40 characters in length and consists of both letters and numbers\. \(Do not use the shorter version of the commit ID, which is typically the first 10 characters of the longer version\.\) Use this value\.

   If you are working on a local Linux, macOS, or Unix machine:

   ```
   aws deploy create-deployment \
     --application-name CodeDeployGitHubDemo-App \
     --deployment-config-name CodeDeployDefault.OneAtATime \
     --deployment-group-name CodeDeployGitHubDemo-DepGrp \
     --description "My GitHub deployment demo" \
     --github-location repository=repository,commitId=commit-id
   ```

   If you are working on a local Windows machine:

   ```
   aws deploy create-deployment --application-name CodeDeployGitHubDemo-App --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name CodeDeployGitHubDemo-DepGrp --description "My GitHub deployment demo" --github-location repository=repository,commitId=commit-id
   ```
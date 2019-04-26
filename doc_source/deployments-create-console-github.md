# Specify Information About a Revision Stored in a GitHub Repository<a name="deployments-create-console-github"></a>

If you are follow the steps in [Create an EC2/On\-Premises Compute Platform Deployment \(Console\)](deployments-create-console.md), follow these steps to add details about an application revision stored in a GitHub repository\.

1. In **Connect to GitHub**, do one of the following:
   + To create a connection for CodeDeploy applications to a GitHub account, sign out of GitHub in a separate web browser tab\. In **GitHub account**, type a name to identify this connection, and then choose **Connect to GitHub**\. The web page prompts you to authorize CodeDeploy to interact with GitHub for your application\. Continue to step 2\.
   + To use a connection you have already created, in **GitHub account**, select its name, and then choose **Connect to GitHub**\. Continue to step 4\.
   + To create a connection to a different GitHub account, sign out of GitHub in a separate web browser tab\. Choose **Connect to a different GitHub account**, and then choose **Connect to GitHub**\. Continue to step 2\.

1. If you are prompted to sign in to GitHub, follow the instructions on the **Sign in** page\. Sign in with your GitHub user name or email and password\.

1. If an **Authorize application** page appears, choose **Authorize application**\. 

1. On the **Create deployment** page, in the **Repository name** box, type the GitHub user or organization name that contains the revision, followed by a forward slash \(`/`\), followed by the name of the repository that contains the revision\. If you are unsure of the value to type:

   1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

   1. In **Your repositories**, hover your mouse pointer over the target repository name\. A tooltip appears, displaying the GitHub user or organization name, followed by a forward slash \(`/`\), followed by the name of the repository\. Type this displayed value into the **Repository name** box\.
**Note**  
If the target repository name is not visible in **Your repositories**, use the **Search GitHub** box to find the target repository name and GitHub user or organization name\.

1. In the **Commit ID** box, type the ID of the commit that refers to the revision in the repository\. If you are unsure of the value to type:

   1. In a separate web browser tab, go to your [GitHub dashboard](https://github.com/dashboard)\.

   1. In **Your repositories**, choose the repository name that contains the target commit\.

   1. In the list of commits, find and copy the commit ID that refers to the revision in the repository\. This ID is typically 40 characters in length and consists of both letters and numbers\. \(Do not use the shorter version of the commit ID, which is typically the first 10 characters of the longer version of the commit ID\.\)

   1. Paste the commit ID into the **Commit ID** box\.
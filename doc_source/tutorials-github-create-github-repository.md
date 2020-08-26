# Step 2: Create a GitHub repository<a name="tutorials-github-create-github-repository"></a>

You will need a GitHub repository to store the revision\.

If you already have a GitHub repository, be sure to substitute its name for **CodeDeployGitHubDemo** throughout this tutorial, and then skip ahead to [Step 3: Upload a sample application to your GitHub repository](tutorials-github-upload-sample-revision.md)\. 

1. On the [GitHub home page](https://github.com/dashboard), do one of the following:
   + In **Your repositories**, choose **New repository**\.
   + On the navigation bar, choose **Create new** \(**\+**\), and then choose **New repository**\.

1. In the **Create a new repository** page, do the following:
   + In the **Repository name** box, enter **CodeDeployGitHubDemo**\.
   + Select **Public**\.
**Note**  
Selecting the default **Public** option means that anyone can see this repository\. You can select the **Private** option to limit who can see and commit to the repository\. 
   + Clear the **Initialize this repository with a README** check box\. You will create a `README.md` file manually in the next step instead\.
   + Choose **Create repository**\.

1. Follow the instructions for your local machine type to use the command line to create the repository\.
**Note**  
If you have enabled two\-factor authentication on GitHub, make sure you enter your personal access token instead of your GitHub login password if prompted for a password\. For information, see [Providing your 2FA authentication code](https://help.github.com/articles/providing-your-2fa-authentication-code/)\.

   **On local Linux, macOS, or Unix machines:**

   1. From the terminal, run the following commands, one at a time, where *user\-name* is your GitHub user name:

      ```
      mkdir /tmp/CodeDeployGitHubDemo
      ```

      ```
      cd /tmp/CodeDeployGitHubDemo
      ```

      ```
      touch README.md
      ```

      ```
      git init
      ```

      ```
      git add README.md
      ```

      ```
      git commit -m "My first commit"
      ```

      ```
      git remote add origin https://github.com/user-name/CodeDeployGitHubDemo.git
      ```

      ```
      git push -u origin master
      ```

   1. Leave the terminal open in the `/tmp/CodeDeployGitHubDemo` location\.

   **On local Windows machines:**

   1. From a command prompt running as an administrator, run the following commands, one at a time:

      ```
      mkdir c:\temp\CodeDeployGitHubDemo
      ```

      ```
      cd c:\temp\CodeDeployGitHubDemo
      ```

      ```
      notepad README.md
      ```

   1. In Notepad, save the `README.md` file\. Close Notepad\. Run the following commands, one at a time, where *user\-name* is your GitHub user name:

      ```
      git init
      ```

      ```
      git add README.md
      ```

      ```
      git commit -m "My first commit"
      ```

      ```
      git remote add origin https://github.com/user-name/CodeDeployGitHubDemo.git
      ```

      ```
      git push -u origin master
      ```

   1. Leave the command prompt open in the `c:\temp\CodeDeployGitHubDemo` location\.
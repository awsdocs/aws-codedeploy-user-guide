# Troubleshoot GitHub token issues<a name="troubleshooting-github-token-issues"></a>

## Invalid GitHub OAuth token<a name="troubleshooting-invalid-github-token"></a>

 CodeDeploy applications created after June 2017 use GitHub OAuth tokens for each AWS Region\. The use of tokens tied to specific AWS Regions gives you more control over which CodeDeploy applications have access to a GitHub repository\. 

 If you receive a GitHub token error, you might have an older token that is now invalid\. 

**To fix an invalid GitHub OAuth token**

1.  Use [ DeleteGitHubAccountToken](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteGitHubAccountToken.html) to remove the old token\. 

1.  Add a new OAuth token\. For more information, see [Integrating CodeDeploy with GitHub](integrations-partners-github.md)\. 

## Maximum number of GitHub OAuth tokens exceeded<a name="troubleshooting-too-many-github-tokens"></a>

When you create a CodeDeploy deployment, the maximum number of allowed GitHub tokens is 10\. If you receive an error about GitHub OAuth tokens, make sure you have 10 or fewer tokens\. If you have more than 10 tokens, the first tokens that were created are invalid\. For example, if you have 11 tokens, the first token you created is invalid\. If you have 12 tokens, the first two tokens you created are invalid\. For information about using the CodeDeploy API to remove old tokens, see [ DeleteGitHubAccountToken](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteGitHubAccountToken.html)\. 
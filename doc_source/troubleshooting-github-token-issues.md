# Troubleshoot GitHub Token Issues<a name="troubleshooting-github-token-issues"></a>

## Invalid GitHub OAuth token<a name="troubleshooting-invalid-github-token"></a>

 AWS CodeDeploy applications created after June 2017 use GitHub OAuth tokens for each AWS region\. The use of tokens tied to specific regions gives you more control over which AWS CodeDeploy applications have access to a GitHub repository\. 

 If you receive a GitHub token error, you might have an older token that is now invalid\. 

**To fix an invalid GitHub OAuth token:**

1.  Use [ DeleteGitHubAccountToken](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteGitHubAccountToken.html) to remove the old token\. 

1.  Add a new OAuth token\. For more information, see [Integrating AWS CodeDeploy with GitHub](integrations-partners-github.md)\. 

## Maximum Number of GitHub OAuth Tokens Exceeded<a name="troubleshooting-too-many-github-tokens"></a>

When you create an AWS CodeDeploy deployment, the maximum number of allowed GitHub tokens is 10\. If you receive an error about GitHub OAuth tokens, make sure you have 10 or fewer tokens\. If you have more than 10 tokens, the first tokens that were created are invalid\. For example, if you have 11 tokens, the first token you created is invalid\. If you have 12 tokens, the first two tokens you created are invalid\. For information about using the AWS CodeDeploy API to remove old tokens, see [ DeleteGitHubAccountToken](http://docs.aws.amazon.com/codedeploy/latest/APIReference/API_DeleteGitHubAccountToken.html)\. 
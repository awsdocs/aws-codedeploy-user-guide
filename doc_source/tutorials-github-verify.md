# Step 7: Monitor and verify the deployment<a name="tutorials-github-verify"></a>

In this step, you will use the CodeDeploy console or the AWS CLI to verify the success of the deployment\. You will use your web browser to view the web page that was deployed to the instance you created or configured\.

**Note**  
If you're deploying to an Ubuntu Server instance, use your own testing strategy to determine whether the deployed revision works as expected on the instance, and then go to the next step\.

**To monitor and verify the deployment \(console\)**

1. In the navigation pane, expand **Deploy**, and then choose **Deployments**\.

1. In the list of deployments, look for the row with an **Application** value of **CodeDeployGitHubDemo\-App** and a **Deployment Group** value of **CodeDeployGitHubDemo\-DepGrp**\. If **Succeeded** or **Failed** do not appear in the **Status** column, choose the **Refresh** button periodically\.

1. If **Failed** appears in the **Status** column, follow the instructions in [View instance details \(console\)](instances-view-details.md#instances-view-details-console) to troubleshoot the deployment\.

1. If **Succeeded** appears in the **Status** column, you can now verify the deployment through your web browser\. Our sample revision deploys a single web page to the instance\. If you're deploying to an Amazon EC2 instance, in your web browser, go to `http://public-dns` for the instance \(for example, `http://ec2-01-234-567-890.compute-1.amazonaws.com`\)\.

1. If you can see the web page, then congratulations\! Now that you've successfully used AWS CodeDeploy to deploy a revision from GitHub, you can skip ahead to [Step 8: Clean up](tutorials-github-clean-up.md)\.

**To monitor and verify the deployment \(CLI\)**

1. Call the list\-deployments command to get the deployment ID for the application named `CodeDeployGitHubDemo-App` and the deployment group named `CodeDeployGitHubDemo-DepGrp`:

   ```
   aws deploy list-deployments --application-name CodeDeployGitHubDemo-App --deployment-group-name CodeDeployGitHubDemo-DepGrp --query "deployments" --output text
   ```

1. Call the get\-deployment command, supplying the ID of the deployment in the output from the list\-deployments command:

   ```
   aws deploy get-deployment --deployment-id deployment-id --query "deploymentInfo.[status, creator]" --output text
   ```

1. If **Failed** is returned, follow the instructions in [View instance details \(console\)](instances-view-details.md#instances-view-details-console) to troubleshoot the deployment\.

1. If **Succeeded** is returned, you can now try verifying the deployment through your web browser\. Our sample revision is a single web page deployed to the instance\. If you're deploying to an Amazon EC2 instance, you can view this page in your web browser by going to `http://public-dns` for the Amazon EC2 instance \(for example, `http://ec2-01-234-567-890.compute-1.amazonaws.com`\)\.

1. If you can see the web page, then congratulations\! You have successfully used AWS CodeDeploy to deploy from your GitHub repository\.
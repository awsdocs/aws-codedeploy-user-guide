# Step 3: Deploy the updated Lambda function<a name="tutorial-lambda-sam-deploy-update"></a>

 In this step, you use your updated `myDateTimeFunction.js` to update and initiate the deployment of your Lambda function\. You can monitor the deployment progress in the CodeDeploy or AWS Lambda console\. 

 The `AutoPublishAlias: live` line in your AWS SAM template causes your infrastructure to detect updates to functions that use the `live` alias\. An update to your function triggers a deployment by CodeDeploy that shifts production traffic from the original version of the function to the updated version\. 

 The sam package and sam deploy commands are used to update and trigger the deployment of your Lambda function\. You executed these commands in [ Package the AWS SAM application ](tutorial-lambda-sam-package.md) and [Deploy the AWS SAM application](tutorial-lambda-sam-deploy.md)\. 

**To deploy your updated Lambda function**

1.  In the `SAM-Tutorial` directory, run the following command\. 

   ```
   sam package \
     --template-file template.yml \
     --output-template-file package.yml  \
     --s3-bucket your-S3-bucket
   ```

    This creates a new set of artifacts that reference your updated Lambda function in your S3 bucket\. 

1.  In the `SAM-Tutorial` directory, run the following command\. 

   ```
   sam deploy \
     --template-file package.yml \
     --stack-name my-date-time-app \
     --capabilities CAPABILITY_IAM
   ```

   Because the stack name is still `my-date-time-app`, AWS CloudFormation recognizes that this is a stack update\. To view your updated stack, return the AWS CloudFormation console, and from the navigation pane, choose **Stacks**\.

**\(Optional\) to view traffic during a deployment \(CodeDeploy console\)**

1. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy/](https://console.aws.amazon.com/codedeploy/)\.

1.  In the navigation pane, expand **Applications**, and then choose your **my\-date\-time\-app\-ServerlessDeploymentApplication** application\. 

1.  In **Deployment groups**, choose your application's deployment group\. Its status should be **In progress**\. 

1.  In **Deployment group history**, choose the deployment that is in progress\. 

   The **Traffic shifting** progress bar and the percentages in the **Original** and **Replacement** boxes on this page display its progress\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lambda-tutorial-codedeploy-console-20-percent-deployed.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

**\(Optional\) to view traffic during a deployment \(Lambda console\)**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1.  From the navigation pane, choose your `my-date-time-app-myDateTimeFunction` function\. In the console, its name contains an identifier, so it looks like `my-date-time-app-myDateTimeFunction-123456ABCDEF`\. 

1.  Choose **Qualifiers**, choose **Aliases**, and then choose **live**\. 

The weights next to your original function version \(version 1\) and your updated function version \(version 2\) show how much traffic is served to each version at the time this AWS Lambda console page was loaded\. The page does not update the weights over time\. If you refresh the page once a minute, the weight for version 1 decreases by 10 percent and the weight for version 2 increases by 10 percent until the weight for version 2 is 100\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/lambda-tutorial-lambda-console-20-percent-deployed.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)
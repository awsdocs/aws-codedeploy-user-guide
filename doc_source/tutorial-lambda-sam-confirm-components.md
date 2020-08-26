# \(Optional\) inspect and test your infrastructure<a name="tutorial-lambda-sam-confirm-components"></a>

 This topic shows how to view your infrastructure components and test your Lambda function\. 

**To see the result of your stack after you run `sam deploy`**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1.  In the navigation pane, choose **Stacks**\. The `my-date-time-app` stack appears at the top\. 

1.  Choose the **Events** tab to see which events are complete\. You can view the events while the stack creation is in progress\. When creation of the stack is complete, you can see all stack creation events\. 

1.  With your stack selected, choose **Resources**\. In the **Type** column, you can see your Lambda functions, `myDateTimeFunction`, `CodeDeployHook_beforeAllowTraffic`, and `CodeDeployHook_afterAllowTraffic`\. The **Physical ID** column of each of your Lambda functions contains a link to view the functions in the Lambda console\. 
**Note**  
 The name of the `myDateTimeFunction` Lambda function is prepended with the name of the AWS CloudFormation stack and has an identifier added to it, so it looks like `my-date-time-app-myDateTimeFunction-123456ABCDEF`\. 

1. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy/](https://console.aws.amazon.com/codedeploy/)\.

1.  In the navigation pane, expand **Deploy**, and then choose **Applications**\. 

1.  You should see a new CodeDeploy application created by AWS CloudFormation with a name that starts with `my-date-time-app-ServerlessDeploymentApplication`\. Choose this application\. 

1.  You should see a deployment group with a name that starts with `my-date-time-app-myDateTimeFunctionDeploymentGroup`\. Choose this deployment group\. 

    Under **Deployment configuration**, you should see **CodeDeployDefault\.LambdaLinear10PercentEvery1Minute**\. 

**\(Optional\) to test your function \(console\)**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1.  From the navigation pane, choose your `my-date-time-app-myDateTimeFunction` function\. In the console, its name contains an identifier, so it looks like `my-date-time-app-myDateTimeFunction-123456ABCDEF`\. 

1.  Choose **Test**\. 

1.  In **Event name**, enter a name for your test event\. 

1.  Enter the following for your test event, and then choose **Create**\. 

   ```
   {
     "option": "date",
     "period": "today"
   }
   ```

1.  Choose **Test**\. You should see only your test event in the list of test events\. 

    For **Execution result**, you should see **succeeded**\. 

1.  Under **Execution result**, expand **Details** to see the results\. You should see the current month, day, and year\. 

**\(Optional\) to test your function \(AWS CLI\)**

1.  Locate the ARN of your Lambda function\. It appears at the top of the Lambda console when you are viewing your function\. 

1.  Run the following command\. Replace *your\-function\-arn* with the function ARN\. 

   ```
   aws lambda invoke \
   --function your-function-arn \
   --payload "{\"option\": \"date\", \"period\": \"today\"}" out.txt
   ```

1.  Open `out.txt` to confirm the result contains the current month, day, and year\. 
# Step 3: Create a lifecycle hook Lambda function<a name="tutorial-ecs-with-hooks-create-hooks"></a>

In this section, you implement one Lambda function for your Amazon ECS deployment's `AfterAllowTestTraffic` hook\. The Lambda function runs a validation test before the updated Amazon ECS application is installed\. For this tutorial, the Lambda function returns `Succeeded`\. During a real world deployment, validation tests return `Succeeded` or `Failed`, depending on the result of the validation test\. Also during a real world deployment, you might implement a Lambda test function for one or more of the other Amazon ECS deployment lifecycle event hooks \(`BeforeInstall`, `AfterInstall`, `BeforeAllowTraffic`, and `AfterAllowTraffic`\)\. For more information, see [List of lifecycle event hooks for an Amazon ECS deployment](reference-appspec-file-structure-hooks.md#reference-appspec-file-structure-hooks-list-ecs)\.

 An IAM role is required to create your Lambda function\. The role grants the Lambda function permission to write to CloudWatch Logs and set the status of a CodeDeploy lifecycle hook\. 

**To create an IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. From the naviation pane, choose **Roles**, and then choose **Create role**\.

1.  Create a role with the following properties: 
   +  **Trusted entity**: **AWS Lambda**\. 
   +  **Permissions**: **AWSLambdaBasicExecutionRole**\. This grants your Lambda function permission to write to CloudWatch Logs\. 
   +  **Role name**: **`lambda-cli-hook-role`**\. 

   For more information, see [ Create an AWS Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/with-userapp.html#with-userapp-walkthrough-custom-events-create-iam-role)\. 

1.  Attach the permission `codedeploy:PutLifecycleEventHookExecutionStatus` to the role you created\. This grants your Lambda functions permission to set the status of a CodeDeploy lifecycle hook during a deployment\. For more information, see [Adding IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-policies-console) in the *AWS Identity and Access Management User Guide* and [PutLifecycleEventHookExecutionStatus](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_PutLifecycleEventHookExecutionStatus.html) in the *CodeDeploy API Reference*\. 

**To create an `AfterAllowTestTraffic` hook Lambda function**

1.  Create a file named `AfterAllowTestTraffic.js` with the following contents\. 

   ```
   'use strict';
    
    const AWS = require('aws-sdk');
    const codedeploy = new AWS.CodeDeploy({apiVersion: '2014-10-06'});
    
    exports.handler = (event, context, callback) => {
    
    	console.log("Entering AfterAllowTestTraffic hook.");
    	
    	// Read the DeploymentId and LifecycleEventHookExecutionId from the event payload
     var deploymentId = event.DeploymentId;
    	var lifecycleEventHookExecutionId = event.LifecycleEventHookExecutionId;
    	var validationTestResult = "Failed";
    	
    	// Perform AfterAllowTestTraffic validation tests here. Set the test result 
    	// to "Succeeded" for this tutorial.
    	console.log("This is where AfterAllowTestTraffic validation tests happen.")
    	validationTestResult = "Succeeded";
    	
    	// Complete the AfterAllowTestTraffic hook by sending CodeDeploy the validation status
    	var params = {
    		deploymentId: deploymentId,
    		lifecycleEventHookExecutionId: lifecycleEventHookExecutionId,
    		status: validationTestResult // status can be 'Succeeded' or 'Failed'
    	};
    	
    	// Pass AWS CodeDeploy the prepared validation test results.
    	codedeploy.putLifecycleEventHookExecutionStatus(params, function(err, data) {
    		if (err) {
    			// Validation failed.
    			console.log('AfterAllowTestTraffic validation tests failed');
    			console.log(err, err.stack);
    			callback("CodeDeploy Status update failed");
    		} else {
    			// Validation succeeded.
    			console.log("AfterAllowTestTraffic validation tests succeeded");
    			callback(null, "AfterAllowTestTraffic validation tests succeeded");
    		}
    	});
    }
   ```

1.  Create a Lambda deployment package\. 

   ```
   zip AfterAllowTestTraffic.zip AfterAllowTestTraffic.js 
   ```

1.  Use the `create-function` command to create a Lambda function for your `AfterAllowTestTraffic` hook\. 

   ```
   aws lambda create-function --function-name AfterAllowTestTraffic \
          --zip-file fileb://AfterAllowTestTraffic.zip \
          --handler AfterAllowTestTraffic.handler \
          --runtime nodejs10.x \
          --role arn:aws:iam::aws-account-id:role/lambda-cli-hook-role
   ```

1.  Make a note of your Lambda function ARN in the `create-function` response\. You use this ARN when you update your CodeDeploy deployment's AppSpec file in the next step\. 
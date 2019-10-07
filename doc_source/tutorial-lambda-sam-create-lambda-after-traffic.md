[Back to contents](index.md)

# Create a File for Your AfterAllowTraffic Lambda Function<a name="tutorial-lambda-sam-create-lambda-after-traffic"></a>

Create the file for your `afterAllowTraffic` hook Lambda function\.

1.  Create a text file and save it as `>afterAllowTraffic.js` in the `SAM-Tutorial` directory\. 

1.  Copy the following Node\.js code into `afterAllowTraffic.js`\. This function executes during your deployment's `AfterAllowTraffic` hook\. 

   ```
   'use strict';
       
       const AWS = require('aws-sdk');
       const codedeploy = new AWS.CodeDeploy({apiVersion: '2014-10-06'});
       var lambda = new AWS.Lambda();
       
       exports.handler = (event, context, callback) => {
       
       	console.log("Entering PostTraffic Hook!");
       	
       	// Read the DeploymentId and LifecycleEventHookExecutionId from the event payload
         var deploymentId = event.DeploymentId;
       	var lifecycleEventHookExecutionId = event.LifecycleEventHookExecutionId;
       
       	var functionToTest = process.env.NewVersion;
       	console.log("AfterAllowTraffic hook tests started");
       	console.log("Testing new function version: " + functionToTest);
       
       	// Create parameters to pass to the updated Lambda function that
       	// include the original "date" parameter. If the function did not 
       	// update as expected, then the "date" option might be invalid. If 
       	// the parameter is invalid, the function returns
       	// a statusCode of 400 indicating it failed.
       	var lambdaParams = {
       		FunctionName: functionToTest,    
       		Payload: "{\"option\": \"date\", \"period\": \"today\"}", 
       		InvocationType: "RequestResponse"
       	};
       
       	var lambdaResult = "Failed";
       	// Invoke the updated Lambda function.
       	lambda.invoke(lambdaParams, function(err, data) {
       		if (err){	// an error occurred
       			console.log(err, err.stack);
       			lambdaResult = "Failed";
       		}
       		else{	// successful response
       			var result = JSON.parse(data.Payload);
       			console.log("Result: " +  JSON.stringify(result));
             console.log("statusCode: " + result.statusCode);
             
             // Check if the status code returned by the updated
             // function is 400. If it is, then it failed. If 
             // is not, then it succeeded.
       			if (result.statusCode != "400"){
               console.log("Validation of time parameter succeeded");
       				lambdaResult = "Succeeded";
             }
             else {
               console.log("Validation failed");
             }
       
       			// Complete the PostTraffic Hook by sending CodeDeploy the validation status
       			var params = {
       				deploymentId: deploymentId,
       				lifecycleEventHookExecutionId: lifecycleEventHookExecutionId,
       				status: lambdaResult // status can be 'Succeeded' or 'Failed'
       			};
       			
       			// Pass AWS CodeDeploy the prepared validation test results.
       			codedeploy.putLifecycleEventHookExecutionStatus(params, function(err, data) {
       				if (err) {
       					// Validation failed.
       					console.log("CodeDeploy Status update failed");
       					console.log(err, err.stack);
       					callback("CodeDeploy Status update failed");
       				} else {
       					// Validation succeeded.
       					console.log("CodeDeploy status updated successfully");
       					callback(null, "CodeDeploy status updated successfully");
       				}
       			});
       		}  
       	});
       }
   ```
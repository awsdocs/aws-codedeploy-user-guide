# Create your AWS SAM template<a name="tutorial-lambda-sam-template"></a>

Create an AWS SAM template file that specifies the components in your infrastructure\.

**To create your AWS SAM template**

1.  Create a directory named `SAM-Tutorial`\. 

1.  In your `SAM-Tutorial` directory, create a file named `template.yml`\. 

1.  Copy the following YAML code into `template.yml`\. This is your AWS SAM template\. 

   ```
   AWSTemplateFormatVersion : '2010-09-09'
   Transform: AWS::Serverless-2016-10-31
   Description: A sample SAM template for deploying Lambda functions.
   
   Resources:
   # Details about the myDateTimeFunction Lambda function
     myDateTimeFunction:
       Type: AWS::Serverless::Function
       Properties:
         Handler: myDateTimeFunction.handler
         Runtime: nodejs10.x
   # Instructs your myDateTimeFunction is published to an alias named "live".      
         AutoPublishAlias: live
   # Grants this function permission to call lambda:InvokeFunction
         Policies:
           - Version: "2012-10-17"
             Statement: 
             - Effect: "Allow"
               Action: 
                 - "lambda:InvokeFunction"
               Resource: '*'
         DeploymentPreference:
   # Specifies the deployment configuration      
             Type: Linear10PercentEvery1Minute
   # Specifies Lambda functions for deployment lifecycle hooks
             Hooks:
               PreTraffic: !Ref beforeAllowTraffic
               PostTraffic: !Ref afterAllowTraffic
               
   # Specifies the BeforeAllowTraffic lifecycle hook Lambda function
     beforeAllowTraffic:
       Type: AWS::Serverless::Function
       Properties:
         Handler: beforeAllowTraffic.handler
         Policies:
           - Version: "2012-10-17"
   # Grants this function permission to call codedeploy:PutLifecycleEventHookExecutionStatus        
             Statement: 
             - Effect: "Allow"
               Action: 
                 - "codedeploy:PutLifecycleEventHookExecutionStatus"
               Resource:
                 !Sub 'arn:aws:codedeploy:${AWS::Region}:${AWS::AccountId}:deploymentgroup:${ServerlessDeploymentApplication}/*'
           - Version: "2012-10-17"
   # Grants this function permission to call lambda:InvokeFunction        
             Statement: 
             - Effect: "Allow"
               Action: 
                 - "lambda:InvokeFunction"
               Resource: !Ref myDateTimeFunction.Version
         Runtime: nodejs10.x
   # Specifies the name of the Lambda hook function      
         FunctionName: 'CodeDeployHook_beforeAllowTraffic'
         DeploymentPreference:
           Enabled: false
         Timeout: 5
         Environment:
           Variables:
             NewVersion: !Ref myDateTimeFunction.Version
             
   # Specifies the AfterAllowTraffic lifecycle hook Lambda function
     afterAllowTraffic:
       Type: AWS::Serverless::Function
       Properties:
         Handler: afterAllowTraffic.handler
         Policies:
           - Version: "2012-10-17"
             Statement: 
   # Grants this function permission to call codedeploy:PutLifecycleEventHookExecutionStatus         
             - Effect: "Allow"
               Action: 
                 - "codedeploy:PutLifecycleEventHookExecutionStatus"
               Resource:
                 !Sub 'arn:aws:codedeploy:${AWS::Region}:${AWS::AccountId}:deploymentgroup:${ServerlessDeploymentApplication}/*'
           - Version: "2012-10-17"
             Statement: 
   # Grants this function permission to call lambda:InvokeFunction          
             - Effect: "Allow"
               Action: 
                 - "lambda:InvokeFunction"
               Resource: !Ref myDateTimeFunction.Version
         Runtime: nodejs10.x
   # Specifies the name of the Lambda hook function      
         FunctionName: 'CodeDeployHook_afterAllowTraffic'
         DeploymentPreference:
           Enabled: false
         Timeout: 5
         Environment:
           Variables:
             NewVersion: !Ref myDateTimeFunction.Version
   ```

This template specifies the following\. For more information, see [AWS SAM template concepts](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-basics.html)\.

**A Lambda function called `myDateTimeFunction`**  
 When this Lambda function is published, the `AutoPublishAlias` line in the template links it to an alias named `live`\. Later in this tutorial, an update to this function triggers a deployment by AWS CodeDeploy that incrementally shifts production traffic from the original version to the updated version\. 

**Two Lambda deployment validation functions**  
 The following Lambda functions are executed during CodeDeploy lifecycle hooks\. The functions contain code that validate the deployment of the updated `myDateTimeFunction`\. The result of the validation tests are passed to CodeDeploy using its `PutLifecycleEventHookExecutionStatus` API method\. If a validation test fails, the deployment fails and is rolled back\.   
+  `CodeDeployHook_beforeAllowTraffic` runs during the `BeforeAllowTraffic` hook\. 
+  `CodeDeployHook_afterAllowTraffic` runs during the `AfterAllowTraffic` hook\. 
The name of both functions start with `CodeDeployHook_`\. The `CodeDeployRoleForLambda` role allows calls to the Lambda `invoke` method only in Lambda functions with names that start with this prefix\. For more information, see [AppSpec 'hooks' section for an AWS Lambda deployment](reference-appspec-file-structure-hooks.md#appspec-hooks-lambda) and [PutLifecycleEventHookExecutionStatus](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_PutLifecycleEventHookExecutionStatus.html) in the *CodeDeploy API Reference*\. 

**Automatic detection of an updated Lambda function**  
 The `AutoPublishAlias` term tells the framework to detect when the `myDateTimeFunction` function changes, and then deploy it using the `live` alias\. 

**A deployment configuration**  
 The deployment configuration determines the rate at which your CodeDeploy application shifts traffic from the original version of the Lambda function to the new version\. This template specifies the predefined deployment configuration `Linear10PercentEvery1Minute`\.   
 You cannot specify a custom deployment configuration in an AWS SAM template\. For more information, see [Create a deployment configuration with CodeDeploy](deployment-configurations-create.md)\.

**Deployment lifecycle hook functions**  
 The `Hooks` section specifies the functions to run during lifecycle event hooks\. `PreTraffic` specifies the function that runs during the `BeforeAllowTraffic` hook\. `PostTraffic` specifies the function that runs during the `AfterAllowTraffic` hook\. 

**Permissions for Lambda to invoke another Lambda function**  
 The specified `lambda:InvokeFunction` permission grants the role used by the AWS SAM application permission to invoke a Lambda function\. This is required when the `CodeDeployHook_beforeAllowTraffic` and `CodeDeployHook_afterAllowTraffic` functions invoke the deployed Lambda function during validation tests\. 
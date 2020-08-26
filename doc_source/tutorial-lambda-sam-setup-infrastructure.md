# Step 1: Set up your infrastructure<a name="tutorial-lambda-sam-setup-infrastructure"></a>

 This topic shows you how to use AWS SAM to create files for your AWS SAM template and your Lambda functions\. Then, you use the AWS SAM `package` and `deploy` commands to generate the components in your infrastructure\. When your infrastructure is ready, you have a CodeDeploy application and deployment group, the Lambda function to update and deploy, and two Lambda functions that contain validation tests that run when you deploy the Lambda function\. When complete, you can use AWS CloudFormation to view your components and the Lambda console or the AWS CLI to test your Lambda function\. 

**Topics**
+ [Create your files](tutorial-lambda-create-files.md)
+ [Package the AWS SAM application](tutorial-lambda-sam-package.md)
+ [Deploy the AWS SAM application](tutorial-lambda-sam-deploy.md)
+ [\(Optional\) inspect and test your infrastructure](tutorial-lambda-sam-confirm-components.md)
# Tutorial: Deploy an Updated Lambda Function with CodeDeploy and the AWS Serverless Application Model<a name="tutorial-lambda-sam"></a>

AWS SAM is an open\-source framework for building serverless applications\. It transforms and expands YAML syntax in a AWS SAM template into AWS CloudFormation syntax to build serverless applications, such as a Lambda function\. For more information, see [What Is the AWS Serverless Application Model?](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) 

 In this tutorial, you use AWS SAM to create a solution that does the following: 
+  Creates your Lambda function\. 
+  Creates your CodeDeploy application and deployment group\. 
+  Creates two Lambda functions that execute deployment validation tests during CodeDeploy lifecycle hooks\. 
+  Detects when your Lambda function is updated\. The updating of the Lambda function triggers a deployment by CodeDeploy that incrementally shifts production traffic from the original version of your Lambda function to the updated version\. 

**Note**  
This tutorial requires you to create resources that might result in charges to your AWS account\. These include possible charges for CodeDeploy, Amazon CloudWatch, and AWS Lambda\. For more information, see [CodeDeploy Pricing](https://aws.amazon.com/codedeploy/pricing/), [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/), and [AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)\.

**Topics**
+ [Prerequisites](tutorial-lambda-sam-prereqs.md)
+ [Step 1: Set Up Your Infrastructure](tutorial-lambda-sam-setup-infrastructure.md)
+ [Step 2: Update the Lambda Function](tutorial-lambda-sam-update-function.md)
+ [Step 3: Deploy the Updated Lambda Function](tutorial-lambda-sam-deploy-update.md)
+ [Step 4: View Your Deployment Results](tutorial-lambda-sam-deploy-view-results.md)
+ [Step 5: Clean Up](tutorial-lambda-clean-up.md)
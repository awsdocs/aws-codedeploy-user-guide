# Deploy the AWS SAM application<a name="tutorial-lambda-sam-deploy"></a>

 Use the AWS SAM sam deploy command with the `package.yml` file to create your Lambda functions and CodeDeploy application and deployment group using AWS CloudFormation\. 

**Note**  
For more information on the sam deploy command, see [AWS SAM CLI command reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-command-reference.html) in the *AWS Serverless Application Model Developer Guide*\. 

 In the `SAM-Tutorial` directory, run the following command\. 

```
sam deploy \
  --template-file package.yml \
  --stack-name my-date-time-app \
  --capabilities CAPABILITY_IAM
```

 The `--capabilities CAPABILITY_IAM` parameter is required to authorize AWS CloudFormation to create IAM roles\. 
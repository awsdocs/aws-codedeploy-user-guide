# Deploy the AWS SAM application<a name="tutorial-lambda-sam-deploy"></a>

 Use the AWS SAM sam deploy command with the `package.yml` file to create your Lambda functions and CodeDeploy application and deployment group using AWS CloudFormation\. 

**Note**  
 The sam deploy command is an alias for the aws cloudformation deploy AWS CLI command\. For more information, see [deploy](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/deploy.html) in the *AWS CloudFormation CLI Reference*\. 

 In the `SAM-Tutorial` directory, run the following command\. 

```
sam deploy \
  --template-file package.yml \
  --stack-name my-date-time-app \
  --capabilities CAPABILITY_IAM
```

 The `--capabilities CAPABILITY_IAM` parameter is required to authorize AWS CloudFormation to create IAM roles\. 
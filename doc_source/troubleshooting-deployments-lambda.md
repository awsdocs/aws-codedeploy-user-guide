# Troubleshoot AWS Lambda deployment issues<a name="troubleshooting-deployments-lambda"></a>

**Topics**
+ [AWS Lambda deployments fail after manually stopping a Lambda deployment that does not have configured rollbacks](#troubleshooting-manually-stopped-lambda-deployment)

## AWS Lambda deployments fail after manually stopping a Lambda deployment that does not have configured rollbacks<a name="troubleshooting-manually-stopped-lambda-deployment"></a>

In some cases, the alias of a Lambda function specified in a deployment might reference two different versions of the function\. The result is that subsequent attempts to deploy the Lambda function fail\. A Lambda deployment can get in this state when it does not have rollbacks configured and is manually stopped\. To proceed, use the AWS Lambda console to make sure the function is not configured to shift traffic between two versions:

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. From the left pane, choose **Functions**\.

1. Select the name of the Lambda function that is in your CodeDeploy deployment\.

1. From **Qualifiers**, choose the alias used in your CodeDeploy deployment\.

1. From **Additional Version**, choose **<none>**\. This ensures the alias is not configured to shift a percentage, or weight, of traffic to more than one version\. Make a note of the version selected in **Version**\.

1. Choose **Save and test**\.

1. Open the CodeDeploy console and attempt a deployment of the version displayed in the drop\-down menu in step 5\.
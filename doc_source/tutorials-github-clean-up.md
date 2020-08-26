# Step 8: Clean up<a name="tutorials-github-clean-up"></a>

To avoid further charges for resources you used during this tutorial, you must terminate the Amazon EC2 instance and its associated resources\. Optionally, you can delete the CodeDeploy deployment component records associated with this tutorial\. If you were using a GitHub repository just for this tutorial, you can delete it now, too\.

## To delete a AWS CloudFormation stack \(if you used the AWS CloudFormation template to create an Amazon EC2 instance\)<a name="tutorials-github-clean-up-cloudformation-template"></a>

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the **Stacks** column, choose the stack starting with `CodeDeploySampleStack`\.

1. Choose **Delete**\.

1. When prompted, choose **Delete stack**\. The Amazon EC2 instance and the associated IAM instance profile and service role are deleted\.

## To manually deregister and clean up an on\-premises instance \(if you provisioned an on\-premises instance\)<a name="tutorials-github-clean-up-on-premises-instance"></a>

1. Use the AWS CLI to call the [deregister](https://docs.aws.amazon.com/cli/latest/reference/deploy/deregister.html) command against the on\-premises instance represented here by *your\-instance\-name* and the associated region by *your\-region*:

   ```
   aws deploy deregister --instance-name your-instance-name --delete-iam-user --region your-region
   ```

1. From the on\-premises instance, call the [uninstall](https://docs.aws.amazon.com/cli/latest/reference/deploy/uninstall.html) command:

   ```
   aws deploy uninstall
   ```

## To manually terminate an Amazon EC2 instance \(if you manually launched an Amazon EC2 instance\)<a name="tutorials-github-clean-up-ec2-instance"></a>

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\.

1. Select the box next to the Amazon EC2 instance you want to terminate\. In the **Actions** menu, point to **Instance State**, and then choose **Terminate**\.

1. When prompted, choose **Yes, Terminate**\. 

## To delete the CodeDeploy deployment component records<a name="tutorials-github-clean-up-codedeploy-records"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Applications**\.

1. Choose **CodeDeployGitHubDemo\-App**\.

1. Choose **Delete application**\.

1. When prompted, enter **Delete**, and then choose **Delete**\. 

## To delete your GitHub repository<a name="tutorials-github-clean-up-github-repository"></a>

See [Deleting a repository](https://help.github.com/articles/deleting-a-repository/) in [GitHub help](https://help.github.com)\.
# Grant Amazon SNS permissions to a CodeDeploy service role<a name="monitoring-sns-event-notifications-permisssions"></a>

Before your triggers can generate notifications, the service role you use in your CodeDeploy operations must be granted permission to access the Amazon SNS resources\. 

**To grant Amazon SNS permissions to a service role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Roles**\.

1. Choose the name of the service role you use in your AWS CodeDeploy operations\.

1. On the **Permissions** tab, in the **Inline Policies** area, choose **Create Role Policy**\.

   –or–

   If the **Create Role Policy** button is not available, expand the **Inline Policies** area, and then choose **click here**\.

1. On the **Set Permissions** page, choose **Custom Policy**, and then choose **Select**\.

1. On the **Review Policy** page, in the **Policy Name** field, enter a name to identify this policy, such as `SNSPublish`\.

1. Paste the following into the **Policy Document** field: 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "sns:Publish",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Apply Policy**\.
# Get Information About a Single On\-Premises Instance<a name="on-premises-instances-operations-view-details-single"></a>

You can get information about a single on\-premises instance by following the instructions in [View Deployment Details with AWS CodeDeploy](deployments-view-details.md)\. You can use the AWS CLI or the AWS CodeDeploy console to get more information about a single on\-premises instance\.

**To get information about a single on\-premises instance \(CLI\)**
+ Call the [get\-on\-premises\-instance](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-on-premises-instance.html) command, specifying the name that uniquely identifies the on\-premises instance \(with the `--instance-name` option\):

  ```
  aws deploy get-on-premises-instance --instance-name AssetTag12010298EX
  ```

**To get information about a single on\-premises instance \(console\)**

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. In the AWS CodeDeploy menu, choose **On\-premises instances**\.

1. In the list of on\-premises instances, choose the arrow next to the on\-premises instance\. Details about the on\-premises instance are displayed\.
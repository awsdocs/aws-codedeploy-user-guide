# Step 5: Check your results again<a name="tutorials-auto-scaling-group-reverify"></a>

In this step, you'll check to see if CodeDeploy installed the SimpleDemoApp revision on the new instance in the Amazon EC2 Auto Scaling group\.

**Topics**
+ [To check automatic deployment results \(CLI\)](#tutorials-auto-scaling-group-reverify-cli)
+ [To check automatic deployment results \(console\)](#tutorials-auto-scaling-group-reverify-console)

## To check automatic deployment results \(CLI\)<a name="tutorials-auto-scaling-group-reverify-cli"></a>

1. Before you call the get\-deployment command, you will need the ID of the automatic deployment\. To get the ID, call the list\-deployments command against the application named **SimpleDemoApp** and the deployment group named **SimpleDemoDG**:

   ```
   aws deploy list-deployments --application-name SimpleDemoApp --deployment-group-name SimpleDemoDG --query "deployments" --output text
   ```

   There should be two deployment IDs\. Use the one you have not yet used in a call to the get\-deployment command:

   ```
   aws deploy get-deployment --deployment-id deployment-id --query "deploymentInfo.[status, creator]" --output text
   ```

   In addition to the deployment status, you should see `autoScaling` in the command output\. \(`autoScaling` means Amazon EC2 Auto Scaling created the deployment\.\) 

   Do not proceed until the deployment status shows `Succeeded`\.

1. Before you call the describe\-instances command, you will need the ID of the new Amazon EC2 instance\. To get this ID, make another call to the describe\-auto\-scaling\-groups command against **CodeDeployDemo\-AS\-Group**:

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].InstanceId" --output text
   ```

   Now make a call to the describe\-instances command:

   ```
   aws ec2 describe-instances --instance-id instance-id --query "Reservations[0].Instances[0].PublicDnsName" --output text
   ```

   In the output of the describe\-instances command, note the public DNS for the new Amazon EC2 instance\.

1. Using a web browser, show the `SimpleDemoApp` revision deployed to that Amazon EC2 instance, using a URL like the following:

   ```
   http://ec2-01-234-567-890.compute-1.amazonaws.com
   ```

   If the congratulations page appears, you've used CodeDeploy to deploy a revision to a scaled\-up Amazon EC2 instance in an Amazon EC2 Auto Scaling group\!

## To check automatic deployment results \(console\)<a name="tutorials-auto-scaling-group-reverify-console"></a>

1. Sign in to the AWS Management Console and open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information that you used in [Getting started with CodeDeploy](getting-started-codedeploy.md)\.

1. In the navigation pane, expand **Deploy**, and then choose **Deployments**\.

1. Choose the deployment ID of the deployment that Amazon EC2 Auto Scaling created\.

   \.

1.  The **Deployment** page displays information about the deployment\. Normally, you would create a deployment on your own, but Amazon EC2 Auto Scaling created one on your behalf to deploy your revision to the new Amazon EC2 instance\.

1. After **Succeeded** is displayed at the top of the page, verify the results on the instance\. You first need to get the public DNS of the instance:

1. In the Amazon EC2 navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**, and then choose the **CodeDeployDemo\-AS\-Group** entry\.

1. On the **Instances** tab, choose the ID of the new Amazon EC2 instance\.

1. On the **Instances** page, on the **Description** tab, note the **Public DNS** value\. It should look something like this: **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**\.

Show the `SimpleDemoApp` revision deployed to the instance using a URL like the following:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com
```

If the congratulations page appears, you've used CodeDeploy to deploy a revision to a scaled\-up Amazon EC2 instance in an Amazon EC2 Auto Scaling group\!
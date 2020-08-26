# Step 3: Check your results<a name="tutorials-auto-scaling-group-verify"></a>

In this step, you'll check to see that CodeDeploy installed the **SimpleDemoApp** revision on the single Amazon EC2 instance in the Amazon EC2 Auto Scaling group\.

**Topics**
+ [To check the results \(CLI\)](#tutorials-auto-scaling-group-verify-cli)
+ [To check the results \(console\)](#tutorials-auto-scaling-group-verify-console)

## To check the results \(CLI\)<a name="tutorials-auto-scaling-group-verify-cli"></a>

First, you'll need the public DNS of the Amazon EC2 instance\.

Use the AWS CLI to get the public DNS of the Amazon EC2 instance in the Amazon EC2 Auto Scaling group by calling the describe\-instances command\. 

Before you call this command, you will need the ID of the Amazon EC2 instance\. To get the ID, call the describe\-auto\-scaling\-groups against **CodeDeployDemo\-AS\-Group** as you did before:

```
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].InstanceId" --output text
```

Now call the describe\-instances command:

```
aws ec2 describe-instances --instance-id instance-id --query "Reservations[0].Instances[0].PublicDnsName" --output text
```

The returned value is the public DNS of the Amazon EC2 instance\.

Using a web browser, show the SimpleDemoApp revision deployed to that Amazon EC2 instance, using a URL like the following:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com
```

If you see the congratulations page, you've successfully used CodeDeploy to deploy a revision to a single Amazon EC2 instance in an Amazon EC2 Auto Scaling group\!

Next, you'll add an Amazon EC2 instance to the Amazon EC2 Auto Scaling group\. After Amazon EC2 Auto Scaling adds the Amazon EC2 instance, CodeDeploy will deploy your revision to the new instance\.

## To check the results \(console\)<a name="tutorials-auto-scaling-group-verify-console"></a>

First, you'll need the public DNS of the Amazon EC2 instance\.

Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

In the Amazon EC2 navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**, and then choose the **CodeDeployDemo\-AS\-Group** entry\.

On the **Instances** tab, choose the Amazon EC2 instance ID in the list\.

On the **Instances** page, on the **Description** tab, note the **Public DNS** value\. It should look something like this: **ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com**\.

Using a web browser, show the SimpleDemoApp revision deployed to that Amazon EC2 instance, using a URL like the following:

```
http://ec2-01-234-567-890.compute-1.amazonaws.com
```

If you see the congratulations page, you've successfully used CodeDeploy to deploy a revision to a single Amazon EC2 instance in an Amazon EC2 Auto Scaling group\!

Next, you add an Amazon EC2 instance to the Amazon EC2 Auto Scaling group\. After Amazon EC2 Auto Scaling adds the Amazon EC2 instance, CodeDeploy will deploy your revision to the new Amazon EC2 instance\.
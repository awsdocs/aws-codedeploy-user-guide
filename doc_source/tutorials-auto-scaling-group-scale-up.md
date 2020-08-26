# Step 4: Increase the number of Amazon EC2 instances in the Amazon EC2 Auto Scaling group<a name="tutorials-auto-scaling-group-scale-up"></a>

In this step, you instruct the Amazon EC2 Auto Scaling group to create an additional Amazon EC2 instance\. After Amazon EC2 Auto Scaling creates the instance, CodeDeploy deploys your revision to it\.

**Topics**
+ [To scale up the number of Amazon EC2 instances in the Amazon EC2 Auto Scaling group \(CLI\)](#tutorials-auto-scaling-group-scale-up-cli)
+ [To scale up the number of Amazon EC2 instances in the deployment group \(console\)](#tutorials-auto-scaling-group-scale-up-console)

## To scale up the number of Amazon EC2 instances in the Amazon EC2 Auto Scaling group \(CLI\)<a name="tutorials-auto-scaling-group-scale-up-cli"></a>

1. Call the update\-auto\-scaling\-group command to increase the Amazon EC2 instances in the Amazon EC2 Auto Scaling group named **CodeDeployDemo\-AS\-Group** from one to two\.

   On local Linux, macOS, or Unix machines:

   ```
   aws autoscaling update-auto-scaling-group \
     --auto-scaling-group-name CodeDeployDemo-AS-Group \
     --min-size 2 \
     --max-size 2 \
     --desired-capacity 2
   ```

   On local Windows machines:

   ```
   aws autoscaling update-auto-scaling-group --auto-scaling-group-name CodeDeployDemo-AS-Group --min-size 2 --max-size 2 --desired-capacity 2
   ```

1. Make sure the Amazon EC2 Auto Scaling group now has two Amazon EC2 instances\. Call the describe\-auto\-scaling\-groups command against **CodeDeployDemo\-AS\-Group**:

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CodeDeployDemo-AS-Group --query "AutoScalingGroups[0].Instances[*].[HealthStatus, LifecycleState]" --output text
   ```

   Do not proceed until both of the returned values show `Healthy` and `InService`\.

## To scale up the number of Amazon EC2 instances in the deployment group \(console\)<a name="tutorials-auto-scaling-group-scale-up-console"></a>

1. In the Amazon EC2 navigation bar, under **Auto Scaling**, choose **Auto Scaling Groups**, and then choose **CodeDeployDemo\-AS\-Group**\.

1. Choose **Actions**, and then choose **Edit**\.

1. On the **Details** tab, in the **Desired**, **Min**, and **Max** boxes, type **2**, and then choose** Save**\.

1. Choose the **Instances** tab\. The new Amazon EC2 instance should appear in the list\. \(If the instance does not appear, you may need to choose the **Refresh** button a few times\.\) Do not proceed until the value of **InService** appears in the **Lifecycle** column and the value of **Healthy** appears in the **Health Status** column\.
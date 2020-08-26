# Set up a load balancer, target groups, and listeners for CodeDeploy Amazon ECS deployments<a name="deployment-groups-create-load-balancer-for-ecs"></a>

 Before you run a deployment using the Amazon ECS compute platform, you must create an Application Load Balancer or a Network Load Balancer, two target groups, and one or two listeners\. This topic shows you how to create an Application Load Balancer\. For more information, see [Before you begin an Amazon ECS deployment](deployment-steps-ecs.md#deployment-steps-prerequisites-ecs)\. 

 One of the target groups directs traffic to your Amazon ECS application's original task set\. The other target group directs traffic to its replacement task set\. During deployment, CodeDeploy creates a replacement task set and reroutes traffic from the original task set to the new one\. CodeDeploy determines which target group is used for each task set\. 

 A listener is used by your load balancer to direct traffic to your target groups\. One production listener is required\. You can specify an optional test listener that directs traffic to your replacement task set while you run validation tests\. 

 The load balancer must use a VPC with two public subnets in different Availability Zones\. The following steps show you how to confirm your default VPC, create an Amazon EC2 Application Load Balancer, and then create two target groups for your load balancer\. For more information, see [Target groups for your network load balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-target-groups.html)\. 

## Verify your default VPC, public subnets, and security group<a name="deployment-groups-create-load-balancer-for-ecs-verify-vpc"></a>

 This topic shows how to create an Amazon EC2 Application Load Balancer, two target groups, and two ports that can be used during an Amazon ECS deloyment\. One of the ports is optional and needed only if you direct traffic to a test port for validation tests during your deployment\. 

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Verify the default VPC to use\. In the navigation pane, choose **Your VPCs**\. Note which VPC shows **Yes** in the **Default VPC** column\. This is your default VPC\. It contains default subnets that you use\.

1. Choose **Subnets**\. Make a note of the subnet IDs of two subnets that show **Yes** in the **Default subnet** column\. You use these IDs when you create your load balancer\.

1. Choose each subnet, and then choose the **Description** tab\. Verify that the subnets you want to use are in different Availability Zones\.

1. Choose the subnets, and then choose the **Route Table** tab\. To verify that each subnet you want to use is a public subnet, confirm that a row with a link to an internet gateway is included in the route table\.

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation pane, choose **Security Groups**\.

1. Verify the security group you want to use is available and make a note of its group ID \(for example, **sg\-abcd1234**\)\. You use this when you create your load balancer\.

## Create an Amazon EC2 Application Load Balancer, two target groups, and listeners \(console\)<a name="deployment-groups-create-load-balancer-for-ecs-console"></a>

To use the Amazon EC2 console to create an Amazon EC2 Application Load Balancer:

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Load Balancers**\. 

1. Choose **Create Load Balancer**\.

1. Choose **Application Load Balancer**, and then choose **Create**\.

1. In **Name**, enter the name of your load balancer\.

1. In **Scheme**, choose **internet\-facing**\.

1. In **IP address type**, choose **ipv4**\.

1. \(Optional\) Configure a second listener port for your load balancer\. You can run deployment validation tests using test traffic that is served to this port\.

   1. Under **Load Balancer Protocol**, choose **Add listener**\.

   1. Under **Load Balancer Protocol** for the second listener, choose **HTTP**\. 

   1. Under **Load Balancer Port**, enter **8080**\.

1. Under **Availability Zones**, in **VPC**, choose the default VPC, and then select the two default subnets you want to use\.

1. Choose **Next: Configure Security Settings**\.

1. Choose **Next: Configure Security Groups**\.

1. Choose **Select an existing security group**, choose the default security group, and then make a note of its ID\.

1. Choose **Next: Configure Routing**\.

1. In **Target group**, choose **New target group**, and configure your first target group: 

   1. In **Name**, enter a target group name \(for example, **target\-group\-1**\)\.

   1. In **Target type**, choose **IP**\.

   1. In **Protocol** choose **HTTP**\. In **Port**, enter **80**\.

   1. Choose **Next: Register Targets**\.

1. Choose **Next: Review**, and then choose **Create**\.

**To create a second target group for your load balancer**

1. After your load balancer is provisioned, open the Amazon EC2 console\. In the navigation pane, choose **Target Groups**\.

1. Choose **Create target group**\.

1. In **Name**, enter a target group name \(for example, **target\-group\-2**\)\.

1. In **Target type**, choose **IP**\.

1. In **Protocol** choose **HTTP**\. In **Port**, enter **80**\.

1. In **VPC**, choose the default VPC\.

1. Choose **Create**\.
**Note**  
You must have two target groups created for your load balancer in order for your Amazon ECS deployment to run\. You use the ARN of one of your target groups when you create your Amazon ECS service\. For more information, see [Step 4: Create an Amazon ECS service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-blue-green.html#create-blue-green-taskdef) in the *Amazon ECS User Guide*\.

## Create an Amazon EC2 Application Load Balancer, two target groups, and listeners \(CLI\)<a name="deployment-groups-create-load-balancer-for-ecs-cli"></a>

To create an Application Load Balancer using the AWS CLI:

1. Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command to create an Application Load Balancer\. Specify two subnets that aren't in the same Availability Zone and a security group\.

   ```
   aws elbv2 create-load-balancer --name bluegreen-alb \
   --subnets subnet-abcd1234 subnet-abcd5678 --security-groups sg-abcd1234 --region us-east-1
   ```

   The output includes the Amazon Resource Name \(ARN\) of the load balancer, in the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create your first target group\. CodeDeploy routes this target group's traffic to the original or the replacement task set in your service\.

   ```
   aws elbv2 create-target-group --name bluegreentarget1 --protocol HTTP --port 80 \
   --target-type ip --vpc-id vpc-abcd1234 --region us-east-1
   ```

   The output includes the ARN of the first target group, in the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create your second target group\. CodeDeploy routes target group's traffic to the task set that is not served by your first target group\. For example, if your first target group routes traffic to the original task set, this target group routes traffic to the replacement task set\.

   ```
   aws elbv2 create-target-group --name bluegreentarget2 --protocol HTTP --port 80 \
   --target-type ip --vpc-id vpc-abcd1234 --region us-east-1
   ```

   The output includes the ARN of the second target group, in the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget2/209a844cd01825a4
   ```

1. Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a listener with a default rule that forwards production traffic to port 80\.

   ```
   aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642 \
   --protocol HTTP --port 80 \
   --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4 --region us-east-1
   ```

   The output includes the ARN of the listener, in the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:listener/app/bluegreen-alb/e5ba62739c16e642/665750bec1b03bd4
   ```

1. \(Optional\) Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a second listener with a default rule that forwards test traffic to port 8080\. You can run deployment validation tests using test traffic that is served this port\.

   ```
   aws elbv2 create-listener --load-balancer-arn arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642 \
   --protocol HTTP --port 8080 \
   --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget2/209a844cd01825a4 --region us-east-1
   ```

   The output includes the ARN of the listener, in the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:listener/app/bluegreen-alb/e5ba62739c16e642/665750bec1b03bd4
   ```
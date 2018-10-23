# Set Up a Load Balancer in Elastic Load Balancing for AWS CodeDeploy Deployments<a name="deployment-groups-create-load-balancer"></a>

Before you run any blue/green deployment, or an in\-place deployment for which you want to specify an optional load balancer in the deployment group, you must have created a Classic Load Balancer or Application Load Balancer in Elastic Load Balancing\. For blue/green deployments, you use that load balancer to register the instances that make up your replacement environment\. Instances in your original environment can optionally be registered with this same load balancer\.

To configure a Classic Load Balancer, follow the instructions in [Tutorial: Create a Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html) in *User Guide for Classic Load Balancers*\. Note the following:
+ In **Step 2: Define Load Balancer**, in **Create LB Inside**, choose the same VPC you selected when you created your instances\.
+ In **Step 5: Register EC2 Instances with Your Load Balancer**, select the instances currently in your deployment group \(in\-place deployments\) or that you have designated to be in your original environment \(blue/green deployments\)\.
+ In **Step 7: Create and Verify Your Load Balancer**, make a note of the DNS address of your load balancer\.

  For example, if you named your load balancer `my-load-balancer`, your DNS address would appear in a format such as `my-load-balancer-1234567890.us-east-2.elb.amazonaws.com`\.

To configure an Application Load Balancer, follow the instructions in one of the following topics:
+ [Create an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html)
+ [Tutorial: Create an Application Load Balancer Using the AWS CLI](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-application-load-balancer-cli.html)
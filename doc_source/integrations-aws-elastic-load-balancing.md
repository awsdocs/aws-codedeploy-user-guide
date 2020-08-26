# Integrating CodeDeploy with Elastic Load Balancing<a name="integrations-aws-elastic-load-balancing"></a>

During CodeDeploy deployments, a load balancer prevents internet traffic from being routed to instances when they are not ready, are currently being deployed to, or are no longer needed as part of an environment\. The exact role the load balancer plays, however, depends on whether it is used in a blue/green deployment or an in\-place deployment\.

**Note**  
The use of Elastic Load Balancing load balancers is mandatory in blue/green deployments and optional in in\-place deployments\.

## Elastic Load Balancing types<a name="integrations-aws-elastic-load-balancing-types"></a>

Elastic Load Balancing provides three types of load balancers that can be used in CodeDeploy deployments: Classic Load Balancers, Application Load Balancers, and Network Load Balancers\.

Classic Load Balancer  
Routes and load balances either at the transport layer \(TCP/SSL\) or the application layer \(HTTP/HTTPS\)\. It supports either EC2\-Classic or a VPC\.

Application Load Balancer  
Routes and load balances at the application layer \(HTTP/HTTPS\) and supports path\-based routing\. It can route requests to ports on each EC2 instance or container instance in your virtual private cloud \(VPC\)\.  
 The Application Load Balancer target groups must have a target type of `instance`\. For more information, see [Target type](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html#target-type)\. 

Network Load Balancer  
Routes and load balances at the transport layer \(TCP/UDP Layer\-4\) based on address information extracted from the TCP packet header, not from packet content\. Network Load Balancers can handle traffic bursts, retain the source IP of the client, and use a fixed IP for the life of the load balancer\. 

To learn more about Elastic Load Balancing load balancers, see the following topics:
+ [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
+ [What is a Classic Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html)
+ [What is an Application Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
+ [What is a Network Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

## Blue/Green deployments<a name="integrations-aws-elastic-load-balancing-blue-green"></a>

Rerouting instance traffic behind an Elastic Load Balancing load balancer is fundamental to CodeDeploy blue/green deployments\. 

During a blue/green deployment, the load balancer allows traffic to be routed to the new instances in a deployment group that the latest application revision has been deployed to \(the replacement environment\), according to the rules you specify, and then blocks traffic from the old instances where the previous application revision was running \(the original environment\)\.

After instances in a replacement environment are registered with a load balancer, instances from the original environment are deregistered and, if you choose, terminated\.

For a blue/green deployment, you can specify a Classic Load Balancer, Application Load Balancer, or Network Load Balancer in your deployment group\. You use the CodeDeploy console or AWS CLI to add the load balancer to a deployment group\.

For more information about load balancers in blue/green deployments, see the following topics:
+ [Set up a load balancer in Elastic Load Balancing for CodeDeploy Amazon EC2 deployments](deployment-groups-create-load-balancer.md)
+ [Create an application for a blue/green deployment \(console\)](applications-create-blue-green.md)
+ [Create a deployment group for an EC2/On\-Premises blue/green deployment \(console\)](deployment-groups-create-blue-green.md)

## In\-place deployments<a name="integrations-aws-elastic-load-balancing-in-place"></a>

During an in\-place deployment, a load balancer prevents internet traffic from being routed to an instance while it is being deployed to, and then makes the instance available for traffic again after the deployment to that instance is complete\.

If a load balancer isn't used during an in\-place deployment, internet traffic may still be directed to an instance during the deployment process\. As a result, your customers might encounter broken, incomplete, or outdated web applications\. When you use an Elastic Load Balancing load balancer with an in\-place deployment, instances in a deployment group are deregistered from a load balancer, updated with the latest application revision, and then reregistered with the load balancer as part of the same deployment group after the deployment is successful\. CodeDeploy will wait for up to 1 hour for the instance to become healthy behind the load balancer\. If the instance is not marked as healthy by the load balancer during the waiting period, CodeDeploy either moves onto the next instance or fails the deployment, based on the deployment configuration\.

For an in\-place deployment, you can specify a Classic Load Balancer, Application Load Balancer, or Network Load Balancer\. You can specify the load balancer as part of the deployment group's configuration, or use a script provided by CodeDeploy to implement the load balancer\.

### Specify in\-place deployment load balancer using a deployment group<a name="integrations-aws-elastic-load-balancing-in-place-deployment-group"></a>

To add the load balancer to a deployment group, you use the CodeDeploy console or AWS CLI\. For information about specifying a load balancer in a deployment group for in\-place deployments, see the following topics:
+ [Create an application for an in\-place deployment \(console\)](applications-create-in-place.md)
+ [Create a deployment group for an in\-place deployment \(console\)](deployment-groups-create-in-place.md)
+ [Set up a load balancer in Elastic Load Balancing for CodeDeploy Amazon EC2 deployments](deployment-groups-create-load-balancer.md)

### Specify in\-place deployment load balancer using a script<a name="integrations-aws-elastic-load-balancing-in-place-script"></a>

Use the steps in the following procedure to use deployment lifecycle scripts to set up load balancing for in\-place deployments\.
**Note**  
You should use the CodeDeployDefault\.OneAtATime deployment configuration only when you are using a script to set up a load balancer for an in\-place deployment\. Concurrent runs are not supported, and the CodeDeployDefault\.OneAtATime setting ensures a serial execution of the scripts\. For more information about deployment configurations, see [Working with deployment configurations in CodeDeploy](deployment-configurations.md)\.

In the CodeDeploy Samples repository on GitHub, we provide instructions and samples you can adapt to use CodeDeploy Elastic Load Balancing load balancers\. These repositories include three sample scripts—`register_with_elb.sh`, `deregister_from_elb.sh`, and `common_functions.sh`—that provide all of the code you need to get going\. Simply edit the placeholders in these three scripts, and then reference these scripts from your `appspec.yml` file\.

To set up in\-place deployments in CodeDeploy with Amazon EC2 instances that are registered with Elastic Load Balancing load balancers, do the following:

1. Download the samples for the type of load balancer you want to use for an in\-place deployment:
   + [Classic Load Balancer](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb)
   + [Application Load Balancer[ or Network Load Balancer](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb-v2) \(the same script can be used for either type\)](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb-v2)

1. Make sure each of your target Amazon EC2 instances has the AWS CLI installed\. 

1. Make sure each of your target Amazon EC2 instances has an IAM instance profile attached with, at minimum, the elasticloadbalancing:\* and autoscaling:\* permissions\.

1. Include in your application's source code directory the deployment lifecycle event scripts \(`register_with_elb.sh`, `deregister_from_elb.sh`, and `common_functions.sh`\)\.

1. In the `appspec.yml` for the application revision, provide instructions for CodeDeploy to run the `register_with_elb.sh` script during the **ApplicationStart** event and the `deregister_from_elb.sh` script during the **ApplicationStop** event\.

1. If the instance is part of an Amazon EC2 Auto Scaling group, you can skip this step\.

   In the `common_functions.sh` script:
   + If you are using the [Classic Load Balancer](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb), specify the names of the Elastic Load Balancing load balancers in `ELB_LIST=""`, and make any changes you need to the other deployment settings in the file\.
   + If you are using the [Application Load Balancer[ or Network Load Balancer](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb-v2)](https://github.com/awslabs/aws-codedeploy-samples/tree/master/load-balancing/elb-v2), specify the names of the Elastic Load Balancing target group names in `TARGET_GROUP_LIST=""`, and make any changes you need to the other deployment settings in the file\.

1. Bundle your application's source code, the `appspec.yml`, and the deployment lifecycle event scripts into an application revision, and then upload the revision\. Deploy the revision to the Amazon EC2 instances\. During the deployment, the deployment lifecycle event scripts will deregister the Amazon EC2 instance with the load balancers, wait for the connection to drain, and then re\-register the Amazon EC2 instance with the load balancers after the deployment is complete\.
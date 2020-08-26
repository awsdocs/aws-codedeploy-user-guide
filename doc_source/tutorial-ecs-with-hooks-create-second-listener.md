# Step 1: Create a test listener<a name="tutorial-ecs-with-hooks-create-second-listener"></a>

 An Amazon ECS deployment with validation tests requires a second listener\. This listener is used to serve test traffic to your updated Amazon ECS application in a replacement task set\. Your validation tests run against the test traffic\. 

 The listener for your test traffic can use either of your target groups\. Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) AWS CLI command to create a second listener with a default rule that forwards test traffic to port 8080\. Use the ARN of your load balancer and the ARN of one of your target groups\.

```
aws elbv2 create-listener --load-balancer-arn your-load-balancer-arn \
--protocol HTTP --port 8080 \
--default-actions Type=forward,TargetGroupArn=your-target-group-arn --region your-aws-region
```
# Troubleshoot Deployment Group Issues<a name="troubleshooting-deployment-groups"></a>

## Tagging an instance as part of a deployment group does not automatically deploy your application to the new instance<a name="troubleshooting-adding-instance-to-group"></a>

AWS CodeDeploy does not automatically deploy your application to a newly tagged instance\. You must create a new deployment in the deployment group\.

You can use AWS CodeDeploy to enable automatic deployments to new Amazon EC2 instances in Auto Scaling groups\. For more information, see [Integrating AWS CodeDeploy with Auto Scaling](integrations-aws-auto-scaling.md)\.
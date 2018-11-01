--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Troubleshoot Deployment Group Issues<a name="troubleshooting-deployment-groups"></a>

## Tagging an instance as part of a deployment group does not automatically deploy your application to the new instance<a name="troubleshooting-adding-instance-to-group"></a>

AWS CodeDeploy does not automatically deploy your application to a newly tagged instance\. You must create a new deployment in the deployment group\.

You can use AWS CodeDeploy to enable automatic deployments to new Amazon EC2 instances in Amazon EC2 Auto Scaling groups\. For more information, see [Integrating AWS CodeDeploy with Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md)\.
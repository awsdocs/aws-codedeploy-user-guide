# Delete a deployment configuration with CodeDeploy<a name="deployment-configurations-delete"></a>

You can use the AWS CLI or the CodeDeploy APIs to delete custom deployment configurations associated with your AWS account\. You cannot delete built\-in deployment configurations, such as `CodeDeployDefault.AllAtOnce`, `CodeDeployDefault.HalfAtATime`, and `CodeDeployDefault.OneAtATime`\.

**Warning**  
You cannot delete a custom deployment configuration that is still in use\. If you delete an unused, custom deployment configuration, you will no longer be able to associate it with new deployments and new deployment groups\. This action cannot be undone\.

To use the AWS CLI to delete a deployment configuration, call the [delete\-deployment\-config](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-config.html) command, specifying the deployment configuration name\. To view a list of deployment configuration names, call the [list\-deployment\-configs](https://docs.aws.amazon.com/cli/latest/reference/deploy/list-deployment-configs.html) command\.

The following example deletes a deployment configuration named ThreeQuartersHealthy\.

```
aws deploy delete-deployment-config --deployment-config-name ThreeQuartersHealthy
```
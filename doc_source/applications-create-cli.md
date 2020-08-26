# Create an application \(CLI\)<a name="applications-create-cli"></a>

To use the AWS CLI to create an application, call the [create\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-application.html) command, specifying a name that uniquely represents the application\. \(In an AWS account, a CodeDeploy application name can be used only once per region\. You can reuse an application name in different regions\.\)

After you use the AWS CLI to create an application, the next step is to create a deployment group that specifies the instances to which to deploy revisions\. For instructions, see [Create a deployment group with CodeDeploy](deployment-groups-create.md)\.

After you create the deployment group, the next step is to prepare a revision to deploy to the application and deployment group\. For instructions, see [Working with application revisions for CodeDeploy](application-revisions.md)\.
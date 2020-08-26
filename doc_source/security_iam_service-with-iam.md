# How AWS CodeDeploy works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to CodeDeploy, you should understand which IAM features are available to use with CodeDeploy\. For more information, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [CodeDeploy identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [CodeDeploy resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on CodeDeploy tags](#security_iam_service-with-iam-tags)
+ [CodeDeploy IAM roles](#security_iam_service-with-iam-roles)

## CodeDeploy identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources and the conditions under which actions are allowed or denied\. CodeDeploy supports actions, resources, and condition keys\. For information about the elements that you use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

Policy actions in CodeDeploy use the `codedeploy:` prefix before the action\. For example, the `codedeploy:GetApplication` permission grants the user permissions to perform the `GetApplication` operation\. Policy statements must include either an `Action` or `NotAction` element\. CodeDeploy defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "codedeploy:action1",
      "codedeploy:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, include the following action to specify all actions that begin with the word `Describe`:

```
"Action": "ec2:Describe*"
```



For a list of CodeDeploy actions, see [Actions Defined by AWS CodeDeploy](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodedeploy.html#awscodedeploy-actions-as-permissions) in the *IAM User Guide*\.

For a table that lists all of the CodeDeploy API actions and the resources they apply to, see [CodeDeploy permissions reference](auth-and-access-control-permissions-reference.md)\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.



For example, you can indicate a deployment group \(*myDeploymentGroup*\) in your statement using its ARN as follows:

```
"Resource": "arn:aws:codedeploy:us-west-2:123456789012:deploymentgroup/myDeploymentGroup"
```

You can also specify all deployment groups that belong to an account by using the wildcard character \(\*\) as follows:

```
"Resource": "arn:aws:codedeploy:us-west-2:123456789012:deploymentgroup/*"
```

To specify all resources, or if an API action does not support ARNs, use the wildcard character \(\*\) in the `Resource` element as follows:

```
"Resource": "*"
```

Some CodeDeploy API actions accept multiple resources \(for example, `BatchGetDeploymentGroups`\)\. To specify multiple resources in a single statement, separate their ARNs with commas, as follows:

```
"Resource": ["arn1", "arn2"]
```

CodeDeploy provides a set of operations to work with the CodeDeploy resources\. For a list of available operations, see [CodeDeploy permissions reference](auth-and-access-control-permissions-reference.md)\.

For a list of CodeDeploy resource types and their ARNs, see [Resources Defined by AWS CodeDeploy](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodedeploy.html) in the *IAM User Guide*\. For information about the actions in which you can specify the ARN of each resource, see [Actions Defined by AWS CodeDeploy](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodedeploy.html#awscodedeploy-actions-as-permissions)\.

#### CodeDeploy resources and operations<a name="arn-formats"></a>

In CodeDeploy, the primary resource is a deployment group\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. CodeDeploy supports other resources that can be used with deployment groups, including applications, deployment configurations, and instances\. These are referred to as subresources\. These resources and subresources have unique ARNs associated with them\. For more information, see [Amazon resource names \(ARNs\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.


| Resource type | ARN format | 
| --- | --- | 
| Deployment group |  `arn:aws:codedeploy:region:account-id:deploymentgroup/deployment-group-name`  | 
| Application |  `arn:aws:codedeploy:region:account-id:application/application-name`  | 
| Deployment configuration |  `arn:aws:codedeploy:region:account-id:deploymentconfig/deployment-configuration-name`   | 
| Instance |  `arn:aws:codedeploy:region:account-id:instance/instance-ID`  | 
|  All CodeDeploy resources  |  `arn:aws:codedeploy:*`  | 
|  All CodeDeploy resources owned by the specified account in the specified Region  |  `arn:aws:codedeploy:region:account-id:*`  | 

**Note**  
Most services in AWS treat a colon \(:\) or a forward slash \(/\) as the same character in ARNs\. However, CodeDeploy uses an exact match in resource patterns and rules\. Be sure to use the correct ARN characters when you create event patterns so that they match the ARN syntax in the resource\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

CodeDeploy does not provide any service\-specific condition keys, but it does support the use of some global condition keys\. For more information, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of CodeDeploy identity\-based policies, see [AWS CodeDeploy identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## CodeDeploy resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

CodeDeploy does not support resource\-based policies\. To view an example of a detailed resource\-based policy page, see [Using resource\-based policies for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)\.

## Authorization based on CodeDeploy tags<a name="security_iam_service-with-iam-tags"></a>

CodeDeploy does not support tagging resources or controlling access based on tags\.

## CodeDeploy IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity in your AWS account that has specific permissions\.

### Using temporary credentials with CodeDeploy<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

CodeDeploy supports the use of temporary credentials\. 

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

CodeDeploy does not support service\-linked roles\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

CodeDeploy supports service roles\. 

### Choosing an IAM role in CodeDeploy<a name="security_iam_service-with-iam-roles-choose"></a>

When you create a deployment group resource in CodeDeploy, you must choose a role to allow CodeDeploy to access Amazon EC2 on your behalf\. If you have previously created a service role or service\-linked role, CodeDeploy provides you with a list of roles to choose from\. It's important to choose a role that allows access to start and stop EC2 instances\.
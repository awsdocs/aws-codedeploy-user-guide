# Overview of Managing Access Permissions to Your CodeDeploy Resources<a name="auth-and-access-control-iam-access-control-identity-based"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and some services \(such as AWS Lambda and Amazon ECS\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [CodeDeploy Resources and Operations](#arn-formats)
+ [Understanding Resource Ownership](#understanding-resource-ownership)
+ [Managing Access to Resources](#managing-access-resources)
+ [Specifying Policy Elements: Actions, Effects, and Principals](#actions-effects-principals)
+ [Specifying Conditions in a Policy](#policy-conditions)

## CodeDeploy Resources and Operations<a name="arn-formats"></a>

In CodeDeploy, the primary resource is a deployment group\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. CodeDeploy supports other resources that can be used with deployment groups, including applications, deployment configurations and instances\. These are referred to as subresources\. These resources and subresources have unique Amazon Resource Names \(ARNs\) associated with them\. For more information about ARNs, see [Amazon Resource Names \(ARN\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.


| Resource Type | ARN Format | 
| --- | --- | 
| Deployment group |  arn:aws:codedeploy:*region*:*account\-id*:deploymentgroup/*deployment\-group\-name*  | 
| Application |  arn:aws:codedeploy:*region*:*account\-id*:application/*application\-name*  | 
| Deployment configuration |  arn:aws:codedeploy:*region*:*account\-id*:deploymentconfig/*deployment\-configuration\-name*   | 
| Instance |  arn:aws:codedeploy:*region*:*account\-id*:instance/*instance\-ID*  | 
|  All CodeDeploy resources  |  arn:aws:codedeploy:\*  | 
|  All CodeDeploy resources owned by the specified account in the specified region  |  arn:aws:codedeploy:*region*:*account\-id*:\*  | 

**Note**  
Most services in AWS treat a colon \(:\) or a forward slash \(/\) as the same character in ARNs\. However, CodeDeploy uses an exact match in resource patterns and rules\. Be sure to use the correct ARN characters when creating event patterns so that they match the ARN syntax in the resource\.

For example, you can indicate a specific deployment group \(*myDeploymentGroup*\) in your statement using its ARN as follows:

```
"Resource": "arn:aws:codedeploy:us-west-2:123456789012:deploymentgroup/myDeploymentGroup"
```

You can also specify all deployment groups that belong to a specific account by using the wildcard character \(\*\) as follows:

```
"Resource": "arn:aws:codedeploy:us-west-2:123456789012:deploymentgroup/*"
```

To specify all resources, or if a specific API action does not support ARNs, use the wildcard character \(\*\) in the `Resource` element as follows:

```
"Resource": "*"
```

Some CodeDeploy API actions accept multiple resources \(for example, `BatchGetDeploymentGroups`\)\. To specify multiple resources in a single statement, separate their ARNs with commas, as follows:

```
"Resource": ["arn1", "arn2"]
```

CodeDeploy provides a set of operations to work with the CodeDeploy resources\. For a list of available operations, see [CodeDeploy Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Understanding Resource Ownership<a name="understanding-resource-ownership"></a>

The AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to create a rule, your AWS account is the owner of the CodeDeploy resource\.
+ If you create an IAM user in your AWS account and grant permissions to create CodeDeploy resources to that user, the user can create CodeDeploy resources\. However, your AWS account, to which the user belongs, owns the CodeDeploy resources\.
+ If you create an IAM role in your AWS account with permissions to create CodeDeploy resources, anyone who can assume the role can create CodeDeploy resources\. Your AWS account, to which the role belongs, owns the CodeDeploy resources\.

## Managing Access to Resources<a name="managing-access-resources"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of CodeDeploy\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as identity\-based policies \(IAM policies\) and policies attached to a resource are referred to as resource\-based policies\. CodeDeploy supports only identity\-based \(IAM policies\)\.

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#identity-based-policies)
+ [Resource\-Based Policies](#resource-based-policies-overview)

### Identity\-Based Policies \(IAM Policies\)<a name="identity-based-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following: 
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to view applications, deployment groups, and other CodeDeploy resources in the CodeDeploy console, you can attach a permissions policy to a user or group that the user belongs to\.

   
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

   

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

      

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\.

      

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. The principal in the trust policy an also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

      

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

In CodeDeploy, identity\-based policies are used to manage permissions to the various resources related to the deployment process\. You can control access to all the following resource types:
+ Applications and application revisions
+ Deployments
+ Deployment configurations
+ Instances and on\-premises instances

The capabilities controlled by resource\-based policies vary depending on the resource type, as outlined in the following table:


****  

| Resource types | Capabilities | 
| --- | --- | 
| All | View and list details about resources | 
| Applications Deployment configurationsDeployment groups | Create resourcesDelete resources | 
| Deployments | Create deploymentsStop deployments | 
| Application revisions | Register application revisions | 
| ApplicationsDeployment groups | Update resources | 
|  On\-premises instances  |  Add tags to instances Remove tags from instances Register instances Deregister instances  | 

You can create specific IAM policies to restrict the calls and resources that users in your account have access to, and then attach those policies to IAM users\. For more information about how to create IAM roles and to explore example IAM policy statements for CodeDeploy, see [Overview of Managing Access Permissions to Your CodeDeploy Resources](#auth-and-access-control-iam-access-control-identity-based)\. 

### Resource\-Based Policies<a name="resource-based-policies-overview"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. CodeDeploy doesn't support resource\-based policies\. 

## Specifying Policy Elements: Actions, Effects, and Principals<a name="actions-effects-principals"></a>

For each CodeDeploy resource, the service defines a set of API operations\. To grant permissions for these API operations, CodeDeploy defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action in order to perform the API operation\. For more information about resources and API operations, see [CodeDeploy Resources and Operations](#arn-formats) and [CodeDeploy Permissions Reference](auth-and-access-control-permissions-reference.md)\.

The following are the basic policy elements:
+ **Resource** – You use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For more information, see [CodeDeploy Resources and Operations](#arn-formats)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, the `codedeploy:GetApplication` permission allows the user permissions to perform the `GetApplication` operation\.
+ **Effect** – You specify the effect, either allow or deny, when the user requests the specific action\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the CodeDeploy API actions and the resources that they apply to, see [CodeDeploy Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Specifying Conditions in a Policy<a name="policy-conditions"></a>

When you grant permissions, you can use the access policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are no condition keys specific to CodeDeploy\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 
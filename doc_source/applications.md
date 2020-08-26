# Working with applications in CodeDeploy<a name="applications"></a>

After you configure instances, but before you can deploy a revision, you must create an application in CodeDeploy\. An *application* is simply a name or container used by CodeDeploy to ensure the correct revision, deployment configuration, and deployment group are referenced during a deployment\. 

Use the information in the following table for next steps:


| Compute platform | Scenario | Information for next step | 
| --- | --- | --- | 
|  **EC2/On\-Premises**  |  I haven't created instances yet\.  |  See [Working with instances for CodeDeploy](instances.md), and then return to this page\.  | 
|  **EC2/On\-Premises**  | I have created instances, but I haven't finished tagging them\. |  See [Tagging instances for deployment groups in CodeDeploy](instances-tagging.md), and then return to this page\.  | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   |  I haven't created an application yet\.  |  See [Create an application with CodeDeploy](applications-create.md)   | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   |  I have already created an application, but I haven't created a deployment group\.  |  See [Create a deployment group with CodeDeploy](deployment-groups-create.md)\.  | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   | I have already created an application and deployment group, but I haven't created an application revision\. | See [Working with application revisions for CodeDeploy](application-revisions.md)\. | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   | I have already created an application and deployment group, and I have already uploaded my application revision\. I'm ready to deploy\. | See [Create a deployment with CodeDeploy](deployments-create.md)\. | 

**Topics**
+ [Create an application](applications-create.md)
+ [View application details](applications-view-details.md)
+ [Create a notification rule](notification-rule-create.md)
+ [Rename an application](applications-rename.md)
+ [Delete an application](applications-delete.md)
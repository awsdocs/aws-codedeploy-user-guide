--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Working with Applications in AWS CodeDeploy<a name="applications"></a>

After you configure instances, but before you can deploy a revision, you must create an application in AWS CodeDeploy\. An *application* is simply a name or container used by AWS CodeDeploy to ensure the correct revision, deployment configuration, and deployment group are referenced during a deployment\. 

Use the information in the following table for next steps:


| Compute platform | Scenario | Information for next step | 
| --- | --- | --- | 
|  **EC2/On\-Premises**  |  I haven't created instances yet\.  |  See [Working with Instances for AWS CodeDeploy](instances.md), and then return to this page\.  | 
|  **EC2/On\-Premises**  | I have created instances, but I haven't finished tagging them\. |  See [Tagging Instances for Deployment Groups in AWS CodeDeploy](instances-tagging.md), and then return to this page\.  | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   |  I haven't created an application yet\.  |  See [Create an Application with AWS CodeDeploy](applications-create.md)   | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   |  I have already created an application, but I haven't created a deployment group\.  |  See [Create a Deployment Group with AWS CodeDeploy](deployment-groups-create.md)\.  | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   | I have already created an application and deployment group, but I haven't created an application revision\. | See [Working with Application Revisions for AWS CodeDeploy](application-revisions.md)\. | 
|   **EC2/On\-Premises**, **AWS Lambda**, and **Amazon ECS**   | I have already created an application and deployment group, and I have already uploaded my application revision\. I'm ready to deploy\. | See [Create a Deployment with AWS CodeDeploy](deployments-create.md)\. | 

**Topics**
+ [Create an Application](applications-create.md)
+ [View Application Details](applications-view-details.md)
+ [Rename an Application](applications-rename.md)
+ [Delete an Application](applications-delete.md)
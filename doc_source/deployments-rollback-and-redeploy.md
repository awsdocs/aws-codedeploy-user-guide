# Redeploy and roll back a deployment with CodeDeploy<a name="deployments-rollback-and-redeploy"></a>

CodeDeploy rolls back deployments by redeploying a previously deployed revision of an application as a new deployment\. These rolled\-back deployments are technically new deployments, with new deployment IDs, rather than restored versions of a previous deployment\.

Deployments can be rolled back automatically or manually\.

**Topics**
+ [Automatic rollbacks](#deployments-rollback-and-redeploy-automatic-rollbacks)
+ [Manual rollbacks](#deployments-rollback-and-redeploy-manual-rollbacks)
+ [Rollback and redeployment workflow](#deployments-rollback-and-redeploy-workflow)
+ [Rollback behavior with existing content](#deployments-rollback-and-redeploy-content-options)

## Automatic rollbacks<a name="deployments-rollback-and-redeploy-automatic-rollbacks"></a>

You can configure a deployment group or deployment to automatically roll back when a deployment fails or when a monitoring threshold you specify is met\. In this case, the last known good version of an application revision is deployed\. You configure automatic rollbacks when you create an application or create or update a deployment group\.

When you create a new deployment, you can also choose to override the automatic rollback configuration that were specified for the deployment group\.

**Note**  
You can use Amazon Simple Notification Service to receive a notification whenever a deployment is rolled back automatically\. For information, see [Monitoring deployments with Amazon SNS event notifications](monitoring-sns-event-notifications.md)\.

For more information about configuring automatic rollbacks, see [Configure advanced options for a deployment group](deployment-groups-configure-advanced-options.md)\. 

## Manual rollbacks<a name="deployments-rollback-and-redeploy-manual-rollbacks"></a>

If you have not set up automatic rollbacks, you can manually roll back a deployment by creating a new deployment that uses any previously deployed application revision and following the steps to redeploy a revision\. You might do this if an application has gotten into an unknown state\. Rather than spending a lot of time troubleshooting, you can redeploy the application to a known working state\. For more information, see [Create a deployment with CodeDeploy](deployments-create.md)\. 

**Note**  
If you remove an instance from a deployment group, CodeDeploy does not uninstall anything that might have already been installed on that instance\.

## Rollback and redeployment workflow<a name="deployments-rollback-and-redeploy-workflow"></a>

When automatic rollback is initiated, or when you manually initiate a redeployment or manual rollback, CodeDeploy first tries to remove from each participating instance all files that were last successfully installed\. CodeDeploy does this by checking the cleanup file:

 `/opt/codedeploy-agent/deployment-root/deployment-instructions/deployment-group-ID-cleanup` file \(for Amazon Linux, Ubuntu Server, and RHEL instances\) 

`C:\ProgramData\Amazon\CodeDeploy\deployment-instructions\deployment-group-ID-cleanup` file \(for Windows Server instances\) 

If it exists, CodeDeploy uses the cleanup file to remove from the instance all listed files before starting the new deployment\. 

For example, the first two text files and two script files were already deployed to an Amazon EC2 instance running Windows Server, and the scripts created two more text files during deployment lifecycle events:

```
c:\temp\a.txt (previously deployed by CodeDeploy)
c:\temp\b.txt (previously deployed by CodeDeploy)
c:\temp\c.bat (previously deployed by CodeDeploy)
c:\temp\d.bat (previously deployed by CodeDeploy)
c:\temp\e.txt (previously created by c.bat)
c:\temp\f.txt (previously created by d.bat)
```

The cleanup file will list only the first two text files and two script files:

```
c:\temp\a.txt
c:\temp\b.txt 
c:\temp\c.bat 
c:\temp\d.bat
```

Before the new deployment, CodeDeploy will remove only the first two text files and the two script files, leaving the last two text files untouched:

```
c:\temp\a.txt will be removed
c:\temp\b.txt will be removed
c:\temp\c.bat will be removed
c:\temp\d.bat will be removed
c:\temp\e.txt will remain
c:\temp\f.txt will remain
```

As part of this process, CodeDeploy will not try to revert or otherwise reconcile any actions taken by any scripts in previous deployments during subsequent redeployments, whether manual or automatic rollbacks\. For example, if the `c.bat` and `d.bat` files contain logic to not re\-create the `e.txt` and `f.txt` files if they already exist, then the old versions of `e.txt` and `f.txt` will remain untouched whenever CodeDeploy runs `c.bat` and `d.bat` in subsequent deployments\. You can add logic to `c.bat` and `d.bat` to always check for and delete old versions of `e.txt` and `f.txt` before creating new ones\. 

## Rollback behavior with existing content<a name="deployments-rollback-and-redeploy-content-options"></a>

As part of the deployment process, the CodeDeploy agent removes from each instance all the files installed by the most recent deployment\. If files that weren’t part of a previous deployment appear in target deployment locations, you can choose what CodeDeploy does with them during the next deployment:
+ **Fail the deployment** — An error is reported and the deployment status is changed to Failed\.
+ **Overwrite the content** — The version of the file from the application revision replaces the version already on the instance\.
+ **Retain the content** — The file in the target location is kept and the version in the application revision is not copied to the instance\. 

You might choose to retain files that you want to be part of the next deployment without having to add them to the application revision package\. For example, you might upload files directly to the instance that are required for the deployment but weren’t added to the application revision bundle\. Or you might upload files to the instance if your applications are already in your production environment but you want to use CodeDeploy for the first time to deploy them\.

In the case of rollbacks, where the most recent successfully deployed application revision is redeployed due to a deployment failure, the content\-handling option for that last successful deployment is applied to the rollback deployment\. 

However, if the deployment that failed was configured to overwrite, instead of retain files, an unexpected result can occur during the rollback\. Specifically, the files you expected to be retained might be removed by the failed deployment\. The files are not on the instance when the rollback deployment runs\.

In the following example, there are three deployments\. Any file that is overwritten \(deleted\) during the failed second deployment is no longer available \(cannot be retained\) when application revision 1 is deployed again during deployment 3:


****  

|  Deployment  |  Application revision  |  Content overwrite option  |  Deployment status  |  Behavior and result  | 
| --- | --- | --- | --- | --- | 
|  deployment 1  |  application revision 1  |  RETAIN  |  Succeeded  |  CodeDeploy detects files in the target locations that were not deployed by the previous deployment\. These files might be placed there intentionally to become part of the current deployment\. They are kept and recorded as part of the current deployment package\.  | 
|  deployment 2  |  application revision 2  |  OVERWRITE  |  Failed  |  During the deployment process, CodeDeploy deletes all the files that are part of the previous successful deployment\. This includes the files that were retained during deployment 1\. However, the deployment fails for unrelated reasons\.   | 
|  deployment 3  |  application revision 1  |  RETAIN  |   | Because auto rollback is enabled for the deployment or deployment group, CodeDeploy deploys the last known good application revision, application revision 1\.However, the files that you wanted to retain in deployment 1 were deleted before deployment 2 failed and cannot be retrieved by AWS CodeDeploy\. You can add them to the instance yourself if they are required for application revision 1, or you can create a new application revision\. | 
# Step 2: Create a sample application revision<a name="tutorials-on-premises-instance-2-create-sample-revision"></a>

In this step, you create a sample application revision to deploy to your on\-premises instance\. 

Because it is difficult to know which software and features are already installed—or are allowed to be installed by your organization's policies—on your on\-premises instance, the sample application revision we offer here simply uses batch scripts \(for Windows Server\) or shell scripts \(for Ubuntu Server and RHEL\) to write text files to a location on your on\-premises instance\. One file is written for each of several CodeDeploy deployment lifecycle events, including **Install**, **AfterInstall**, **ApplicationStart**, and **ValidateService**\. During the **BeforeInstall** deployment lifecycle event, a script will run to remove old files written during previous deployments of this sample and create a location on your on\-premises instance to which to write the new files\. 

**Note**  
This sample application revision may fail to be deployed if any of the following are true:  
The user account that starts the CodeDeploy agent on the on\-premises instance does not have permission to execute scripts\.
The user account does not have permission to create or delete folders in the locations listed in the scripts\.
The user account does not have permission to create text files in the locations listed in the scripts\.

**Note**  
If you configured a Windows Server instance and want to deploy a different sample, you may want to use the one in [Step 2: Configure your source content to deploy to the Windows Server Amazon EC2 instance](tutorials-windows-configure-content.md) in the [Tutorial: Deploy a "hello, world\!" application with CodeDeploy \(Windows Server\)](tutorials-windows.md) tutorial\.  
If you configured a RHEL instance and want to deploy a different sample, you may want to use the one in [Step 2: Configure your source content to be deployed to the Amazon Linux or Red Hat Enterprise Linux Amazon EC2 instance](tutorials-wordpress-configure-content.md) in the [Tutorial: Deploy WordPress to an Amazon EC2 instance \(Amazon Linux or Red Hat Enterprise Linux and Linux, macOS, or Unix\)](tutorials-wordpress.md) tutorial\.  
Currently, there is no alternative sample for Ubuntu Server\.

1. On your development machine, create a subdirectory \(subfolder\) named `CodeDeployDemo-OnPrem` that will store the sample application revision's files, and then switch to the subfolder\. For this example, we assume you'll use the `c:\temp` folder as the root folder for Windows Server or the `/tmp` folder as the root folder for Ubuntu Server and RHEL\. If you use a different folder, be sure to substitute it for ours throughout this tutorial: 

   For Windows:

   ```
   mkdir c:\temp\CodeDeployDemo-OnPrem
   cd c:\temp\CodeDeployDemo-OnPrem
   ```

   For Linux, macOS, or Unix:

   ```
   mkdir /tmp/CodeDeployDemo-OnPrem
   cd /tmp/CodeDeployDemo-OnPrem
   ```

1. In the root of the `CodeDeployDemo-OnPrem` subfolder, use a text editor to create two files named `appspec.yml` and `install.txt`:

   `appspec.yml` for Windows Server:

   ```
   version: 0.0
   os: windows
   files:
     - source: .\install.txt
       destination: c:\temp\CodeDeployExample
   hooks:
     BeforeInstall:
       - location: .\scripts\before-install.bat
         timeout: 900
     AfterInstall:
       - location: .\scripts\after-install.bat     
         timeout: 900
     ApplicationStart:
       - location: .\scripts\application-start.bat  
         timeout: 900
     ValidateService:
       - location: .\scripts\validate-service.bat    
         timeout: 900
   ```

   `appspec.yml` for Ubuntu Server and RHEL:

   ```
   version: 0.0
   os: linux
   files:
     - source: ./install.txt
       destination: /tmp/CodeDeployExample
   hooks:
     BeforeInstall:
       - location: ./scripts/before-install.sh
         timeout: 900
     AfterInstall:
       - location: ./scripts/after-install.sh
         timeout: 900
     ApplicationStart:
       - location: ./scripts/application-start.sh
         timeout: 900
     ValidateService:
       - location: ./scripts/validate-service.sh
         timeout: 900
   ```

   For more information about AppSpec files, see [Add an application specification file to a revision for CodeDeploy](application-revisions-appspec-file.md) and [CodeDeploy AppSpec File reference](reference-appspec-file.md)\.

   `install.txt`:

   ```
   The Install deployment lifecycle event successfully completed.
   ```

1. Under the root of the `CodeDeployDemo-OnPrem` subfolder, create a `scripts` subfolder, and then switch to it:

   For Windows:

   ```
   mkdir c:\temp\CodeDeployDemo-OnPrem\scripts
   cd c:\temp\CodeDeployDemo-OnPrem\scripts
   ```

   For Linux, macOS, or Unix:

   ```
   mkdir -p /tmp/CodeDeployDemo-OnPrem/scripts
   cd /tmp/CodeDeployDemo-OnPrem/scripts
   ```

1. In the root of the `scripts` subfolder, use a text editor to create four files named `before-install.bat`, `after-install.bat`, `application-start.bat`, and `validate-service.bat` for Windows Server, or `before-install.sh`, `after-install.sh`, `application-start.sh`, and `validate-service.sh` for Ubuntu Server and RHEL:

   For Windows Server:

   `before-install.bat`:

   ```
   set FOLDER=%HOMEDRIVE%\temp\CodeDeployExample
   
   if exist %FOLDER% (
     rd /s /q "%FOLDER%"
   )
   
   mkdir %FOLDER%
   ```

   `after-install.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The AfterInstall deployment lifecycle event successfully completed. > after-install.txt
   ```

   `application-start.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The ApplicationStart deployment lifecycle event successfully completed. > application-start.txt
   ```

   `validate-service.bat`:

   ```
   cd %HOMEDRIVE%\temp\CodeDeployExample
   
   echo The ValidateService deployment lifecycle event successfully completed. > validate-service.txt
   ```

   For Ubuntu Server and RHEL:

   `before-install.sh`:

   ```
   #!/bin/bash
   export FOLDER=/tmp/CodeDeployExample
   
   if [ -d $FOLDER ]
   then
    rm -rf $FOLDER
   fi
   
   mkdir -p $FOLDER
   ```

   `after-install.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The AfterInstall deployment lifecycle event successfully completed." > after-install.txt
   ```

   `application-start.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The ApplicationStart deployment lifecycle event successfully completed." > application-start.txt
   ```

   `validate-service.sh`:

   ```
   #!/bin/bash
   cd /tmp/CodeDeployExample
   
   echo "The ValidateService deployment lifecycle event successfully completed." > validate-service.txt
   
   unset FOLDER
   ```

1. For Ubuntu Server and RHEL only, make sure the four shell scripts have execute permissions:

   ```
   chmod +x ./scripts/*
   ```
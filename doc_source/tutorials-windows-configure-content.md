# Step 2: Configure your source content to deploy to the Windows Server Amazon EC2 instance<a name="tutorials-windows-configure-content"></a>

Now it's time to configure your application's source content so you have something you can deploy to the Amazon EC2 instance\. For this tutorial, you'll deploy a single web page to the Amazon EC2 instance running Windows Server, which will run Internet Information Services \(IIS\) as its web server\. This web page will display a simple "Hello, World\!" message\.

**Topics**
+ [Create the web page](#tutorials-windows-configure-content-download-code)
+ [Create a script to run your application](#tutorials-windows-configure-content-create-scripts)
+ [Add an application specification file](#tutorials-windows-configure-content-add-appspec-file)

## Create the web page<a name="tutorials-windows-configure-content-download-code"></a>

1. Create a subdirectory \(subfolder\) named `HelloWorldApp` in your `c:\temp` folder, and then switch to that folder\.

   ```
   mkdir c:\temp\HelloWorldApp
   cd c:\temp\HelloWorldApp
   ```
**Note**  
You don't have to use the location of `c:\temp` or the subfolder name of `HelloWorldApp`\. If you use a different location or subfolder name, be sure to use it throughout this tutorial\.

1. Use a text editor to create a file inside of the folder\. Name the file `index.html`\.

   ```
   notepad index.html
   ```

1. Add the following HTML code to the file, and then save the file\.

   ```
   <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
   <html>
   <head>
     <title>Hello, World!</title>
     <style>
       body {
         color: #ffffff;
         background-color: #0188cc;
         font-family: Arial, sans-serif;  
         font-size:14px;
       }
     </style>
   </head>
   <body>
     <div align="center"><h1>Hello, World!</h1></div>
     <div align="center"><h2>You have successfully deployed an application using CodeDeploy</h2></div>
     <div align="center">
       <p>What to do next? Take a look through the <a href="https://aws.amazon.com/codedeploy">CodeDeploy Documentation</a>.</p>
     </div>
   </body>
   </html>
   ```

## Create a script to run your application<a name="tutorials-windows-configure-content-create-scripts"></a>

Next, you will create a script that CodeDeploy will use to set up the web server on the target Amazon EC2 instance\.

1. In the same subfolder where the `index.html` file is saved, use a text editor to create another file\. Name the file `before-install.bat`\.

   ```
   notepad before-install.bat
   ```

1. Add the following batch script code to the file, and then save the file\.

   ```
   REM Install Internet Information Server (IIS).
   c:\Windows\Sysnative\WindowsPowerShell\v1.0\powershell.exe -Command Import-Module -Name ServerManager
   c:\Windows\Sysnative\WindowsPowerShell\v1.0\powershell.exe -Command Install-WindowsFeature Web-Server
   ```

## Add an application specification file<a name="tutorials-windows-configure-content-add-appspec-file"></a>

Next, you will add an application specification file \(AppSpec file\) in addition to the web page and batch script file\. The AppSpec file is a [YAML](http://www.yaml.org)\-formatted file used by CodeDeploy to: 
+ Map the source files in your application revision to their destinations on the instance\.
+ Specify scripts to be run on the instance during the deployment\.

The AppSpec file must be named `appspec.yml`\. It must be placed in the application source code's root folder\.

1. In the same subfolder where the `index.html` and `before-install.bat` files are saved, use a text editor to create another file\. Name the file `appspec.yml`\.

   ```
   notepad appspec.yml
   ```

1. Add the following YAML code to the file, and then save the file\.

   ```
   version: 0.0
   os: windows
   files:
     - source: \index.html
       destination: c:\inetpub\wwwroot
   hooks:
     BeforeInstall:
       - location: \before-install.bat
         timeout: 900
   ```

CodeDeploy will use this AppSpec file to copy the `index.html` file in the application source code's root folder to the `c:\inetpub\wwwroot` folder on the target Amazon EC2 instance\. During the deployment, CodeDeploy will run the `before-install.bat` batch script on the target Amazon EC2 instance during the **BeforeInstall** deployment lifecycle event\. If this script takes longer than 900 seconds \(15 minutes\) to run, CodeDeploy will stop the deployment and mark the deployment to the Amazon EC2 instance as failed\.

For more information about these settings, see the [CodeDeploy AppSpec File reference](reference-appspec-file.md)\.

**Important**  
The locations and numbers of spaces between each of the items in this file are important\. If the spacing is incorrect, CodeDeploy will raise an error that may be difficult to debug\. For more information, see [AppSpec File spacing](reference-appspec-file.md#reference-appspec-file-spacing)\.
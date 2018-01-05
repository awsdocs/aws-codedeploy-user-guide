# Step 2: Configure Your Source Content to Deploy to the Amazon Linux or Red Hat Enterprise Linux Amazon EC2 Instance<a name="tutorials-wordpress-configure-content"></a>

Now it's time to configure your application's source content so you have something to deploy to the instance\.


+ [Get the Source Code](#tutorials-wordpress-configure-content-download-code)
+ [Create Scripts to Run Your Application](#tutorials-wordpress-configure-content-create-scripts)
+ [Add an Application Specification File](#tutorials-wordpress-configure-content-add-appspec-file)

## Get the Source Code<a name="tutorials-wordpress-configure-content-download-code"></a>

For this tutorial, you'll deploy the WordPress content publishing platform from your development machine to the target Amazon EC2 instance\. To get the WordPress source code, you can use built\-in command\-line calls\. Or, if you have Git installed on your development machine, you can use that instead\.

For these steps, we assume you'll download a copy of the WordPress source code to the `/tmp` directory on your development machine\. \(You can choose any directory you like, but remember to substitute your chosen location for `/tmp` wherever it is specified in these steps\.\)


+ [To get a copy of the WordPress source code \(built\-in command\-line calls\)](#tutorials-wordpress-configure-content-download-code-command-line)
+ [To get a copy of the WordPress source code \(Git\)](#tutorials-wordpress-configure-content-download-code-git)

### To get a copy of the WordPress source code \(built\-in command\-line calls\)<a name="tutorials-wordpress-configure-content-download-code-command-line"></a>

1. Call the wget command to download a copy of the WordPress source code, as a \.zip file, to the current directory:

   ```
   wget https://github.com/WordPress/WordPress/archive/master.zip
   ```

1. Call the unzip, mkdir, cp, and rm commands to unpack the `master` \.zip file into the `/tmp/WordPress_Temp` directory \(folder\), copy its unzipped contents to the `/tmp/WordPress` destination folder, and then delete the temporary `/tmp/WordPress_Temp` folder and `master` file\. Run the commands one at a time:

   ```
   unzip master -d /tmp/WordPress_Temp
   ```

   ```
   mkdir -p /tmp/WordPress
   ```

   ```
   cp -paf /tmp/WordPress_Temp/WordPress-master/* /tmp/WordPress
   ```

   ```
   rm -rf /tmp/WordPress_Temp
   ```

   ```
   rm -f master
   ```

   This leaves you with a clean set of WordPress source code files in the `/tmp/WordPress` folder\.

### To get a copy of the WordPress source code \(Git\)<a name="tutorials-wordpress-configure-content-download-code-git"></a>

1. Download and install [Git](http://git-scm.com) on your development machine\.

1. In the `/tmp/WordPress` folder, call the git init command\. 

1. Call the git clone command to clone the public WordPress repository, making your own copy of it in the `/tmp/WordPress` destination folder:

   ```
   git clone https://github.com/WordPress/WordPress.git /tmp/WordPress
   ```

   This leaves you with a clean set of WordPress source code files in the `/tmp/WordPress` folder\.

## Create Scripts to Run Your Application<a name="tutorials-wordpress-configure-content-create-scripts"></a>

Next, you will create a folder and scripts in the directory\. AWS CodeDeploy will use these scripts to set up and deploy your application revision on the target Amazon EC2 instance\. You can use any text editor to create the scripts\.

1. Create a scripts directory in your copy of the WordPress source code:

   ```
   mkdir -p /tmp/WordPress/scripts
   ```

1. Create an `install_dependencies.sh` file in `/tmp/WordPress/scripts`\. Add the following lines to the file\. This `install_dependencies.sh` script will install Apache, MySQL, and PHP\. It will also add MySQL support to PHP\.

   ```
   #!/bin/bash
   yum groupinstall -y "Web Server" "MySQL Database" "PHP Support"
   yum install -y php-mysql
   ```

1. Create a `stop_server.sh` file in `/tmp/WordPress/scripts`\. Add the following lines to the file\. This `stop_server.sh` script will stop Apache and MySQL\.

   ```
   #!/bin/bash
   isExistApp=`pgrep httpd`
   if [[ -n  $isExistApp ]]; then
      service httpd stop
   fi
   isExistApp=`pgrep mysqld`
   if [[ -n  $isExistApp ]]; then
       service mysqld stop
   fi
   ```

1. Create a `start_server.sh` file in `/tmp/WordPress/scripts`\. Add the following lines to the file\. This `start_server.sh` script will start Apache and MySQL\.

   ```
   #!/bin/bash
   service httpd start
   service mysqld start
   ```

1. Finally, create a `change_permissions.sh` script in `/tmp/WordPress/scripts`\. This will be used to change the folder permissions in Apache\.

   ```
   #!/bin/bash
   chmod -R 755 /var/www/html/WordPress
   ```

1. Give all of the scripts executable permissions\. On the command line, type:

   ```
   chmod +x /tmp/WordPress/scripts/*
   ```

## Add an Application Specification File<a name="tutorials-wordpress-configure-content-add-appspec-file"></a>

Next, you will add an application specification file \(AppSpec file\), a [YAML](http://www.yaml.org)\-formatted file used by AWS CodeDeploy to:

+ Map the source files in your application revision to their destinations on the target Amazon EC2 instance\.

+ Specify custom permissions for deployed files\.

+ Specify scripts to be run on the target Amazon EC2 instance during the deployment\.

The AppSpec file must be named `appspec.yml`\. It must be placed in the application's source code's root directory\.

With your text editor, create a file named `appspec.yml`\. Add the following lines to the file:

```
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html/WordPress
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
```

AWS CodeDeploy will use this AppSpec file to copy all of the files in the `/tmp/WordPress` folder on the development machine to the `/var/www/html/WordPress` folder on the target Amazon EC2 instance\. During the deployment, AWS CodeDeploy will run the specified scripts as `root` in the `/var/www/html/WordPress/scripts` folder on the target Amazon EC2 instance at specified events during the deployment lifecycle, such as **BeforeInstall** and **AfterInstall**\. If any of these scripts take longer than 300 seconds \(5 minutes\) to run, AWS CodeDeploy will stop the deployment and mark the deployment as failed\.

For more information about these settings, see the [AWS CodeDeploy AppSpec File Reference](reference-appspec-file.md)\.

**Important**  
The locations and numbers of spaces between each of the items in this file are important\. If the spacing is incorrect, AWS CodeDeploy will raise an error that may be difficult to debug\. For more information, see [AppSpec File Spacing](reference-appspec-file.md#reference-appspec-file-spacing)\.
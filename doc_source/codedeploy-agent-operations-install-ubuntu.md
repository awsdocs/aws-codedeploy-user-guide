# Install the CodeDeploy agent for Ubuntu Server<a name="codedeploy-agent-operations-install-ubuntu"></a>

**Note**  
We recommend installing the CodeDeploy agent with AWS Systems Manager to be able to configure scheduled updates of the agent\. For more information, see [Install the CodeDeploy agent using AWS Systems Manager](codedeploy-agent-operations-install-ssm.md)\.

**To install the CodeDeploy agent on Ubuntu Server**

1. Sign in to the instance\.

1. Do one of the following, depending on the version of Ubuntu Server:
   + On Ubuntu Server 14\.04, enter the following commands, one after the other:

     ```
     sudo apt-get update
     ```

     ```
     sudo apt-get install ruby2.0
     ```

     ```
     sudo apt-get install wget
     ```
   + On Ubuntu Server 16\.04 and later, enter the following commands, one after the other:

     ```
     sudo apt update
     ```

     ```
     sudo apt install ruby-full
     ```

     ```
     sudo apt install wget
     ```

1. Enter the following command:

   ```
   cd /home/ubuntu
   ```

   */home/ubuntu* represents the default user name for an Ubuntu Server instance\. If your instance was created using a custom AMI, the AMI owner might have specified a different default user name\. 

1. Enter the following command:

   ```
   wget https://bucket-name.s3.region-identifier.amazonaws.com/latest/install
   ```

   *bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. *region\-identifier* is the identifier for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2` and replace *region\-identifier* with `us-east-2`\. For a list of bucket names and region identifiers, see [Resource kit bucket names by Region](resource-kit.md#resource-kit-bucket-names)\.

1. Enter the following command:

   ```
   chmod +x ./install
   ```

1. Do one of the following:
   + To install the latest version of the CodeDeploy agent on Ubuntu 14\.04, 16\.04, and 18\.04:

     ```
     sudo ./install auto
     ```
   + To install the latest version of the CodeDeploy agent on Ubuntu 20\.04:
**Note**  
Writing the output to a temporary log file is a workaround that should be used while we address a known bug with the `install` script on Ubuntu 20\.04\.

     ```
     sudo ./install auto > /tmp/logfile
     ```
   + To install a specific version of the CodeDeploy agent on Ubuntu 14\.04, 16\.04, and 18\.04:
     + List the available versions in your region:

       ```
       aws s3 ls s3://aws-codedeploy-region-identifier/releases/ | grep '\.deb$'
       ```
     + Install one of the versions:

       ```
       sudo ./install auto -v releases/codedeploy-agent-###.deb
       ```
**Note**  
The minimum supported version of the CodeDeploy agent is 1\.1\.0\. Use of an earlier CodeDeploy agent might cause deployments to fail\.
   + To install a specific version of the CodeDeploy agent on Ubuntu 20\.04:
     + List the available versions in your region:

       ```
       aws s3 ls s3://aws-codedeploy-region-identifier/releases/ | grep '\.deb$'
       ```
     + Install one of the versions:

       ```
       sudo ./install auto -v releases/codedeploy-agent-###.deb > /tmp/logfile
       ```
**Note**  
Writing the output to a temporary log file is a workaround that should be used while we address a known bug with the `install` script on Ubuntu 20\.04\.
**Note**  
The minimum supported version of the CodeDeploy agent is 1\.1\.0\. Use of an earlier CodeDeploy agent might cause deployments to fail\.

**To check that the service is running**

1. Enter the following command:

   ```
   sudo service codedeploy-agent status
   ```

   If the CodeDeploy agent is installed and running, you should see a message like `The AWS CodeDeploy agent is running`\.

1. If you see a message like `error: No AWS CodeDeploy agent running`, start the service and run the following two commands, one at a time:

   ```
   sudo service codedeploy-agent start
   ```

   ```
   sudo service codedeploy-agent status
   ```
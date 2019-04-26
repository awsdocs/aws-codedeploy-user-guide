# Install or reinstall the CodeDeploy agent for Ubuntu Server<a name="codedeploy-agent-operations-install-ubuntu"></a>

Sign in to the instance, and run the following commands, one at a time\. 

**Note**  
In the fifth command, `/home/ubuntu` represents the default user name for an Ubuntu Server instance\. If your instance was created using a custom AMI, the AMI owner might have specified a different default user name\. 

```
sudo apt-get update
```

On Ubuntu Server 14\.04:
+ 

  ```
  sudo apt-get install ruby2.0
  ```

On Ubuntu Server 16\.04:
+ 

  ```
  sudo apt-get install ruby
  ```

```
sudo apt-get install wget
```

```
cd /home/ubuntu
```

```
wget https://bucket-name.s3.amazonaws.com/latest/install
```

```
chmod +x ./install
```

```
sudo ./install auto
```

*bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2`\. For a list of bucket names, see [Resource Kit Bucket Names by Region](resource-kit.md#resource-kit-bucket-names)\.

To check that the service is running, run the following command:

```
sudo service codedeploy-agent status
```

If the CodeDeploy agent is installed and running, you should see a message like `The AWS CodeDeploy agent is running`\.

If you see a message like `error: No AWS CodeDeploy agent running`, start the service and run the following two commands, one at a time:

```
sudo service codedeploy-agent start
```

```
sudo service codedeploy-agent status
```